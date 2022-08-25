---
title: "Liunx下的日志切割工具-logrotate"
author: yanjr
tags:
  - linux
  - logrotate
categories: linux
date: 2019-02-11 10:08:00 +0800
---


### logrotate介绍
logrotate是个十分有用的工具，它可以自动对日志进行截断（或轮循）、压缩以及删除旧的日志文件。例如，你可以设置logrotate，让/var/log/foo日志文件每30天轮循，并删除超过6个月的日志。配置完后，logrotate的运作完全自动化，不必进行任何进一步的人为干预。另外，旧日志也可以通过电子邮件发送，不过该选项超出了本教程的讨论范围。

### logrotate安装
主流Linux发行版上都默认安装有logrotate包，如果出于某种原因，logrotate没有出现在里头，你可以使用apt-get或yum命令来安装。
在Debian或Ubuntu上：
``` bash
$ apt-get install logrotate cron
```
在Fedora，CentOS或RHEL上：
``` bash
$ yum install logrotate crontabs
```
logrotate的配置文件是/etc/logrotate.conf，通常不需要对它进行修改。日志文件的轮循设置在独立的配置文件中，它（们）放在/etc/logrotate.d/目录下。

### 配置文件
* /etc/logrotate.conf 
* /etc/logrotate.d/
* /etc/logrotate.conf会包含/etc/logrotate.d/目录下的配置文件
由于logratate已经加到cron.daily（/etc/cron.daily/logrotate），不再需要加到计划任务中

### 常用命令
* logrotate /etc/logrotate.conf    用配置文件的话，不能马上有效，直接使用命令马上可以生效；要调用为/etc/lograte.d/下配置的所有日志调用logrotate。
* logrotate -f  /etc/logrotate.conf   强制执行
* /usr/sbin/logrotate -f /etc/logrotate.d/nginx  立即执行logratate；要为某个特定的配置调用logrotate。

### 常用配置
compress：对轮询出来的日志文件进行压缩，默认我gzip  
nocompress：对轮询出来的日志文件不进行压缩  
compresscmd：指定使用什么方式进行压缩，默认为gzip。指定的格式为：compresscmd /usr/bin/bzip2  
compressext： 指定压缩文件的后缀，如：compressext.bz2，默认随压缩方式的  
uncompresscmd：解压方式，默认为gunzip  
copy：日志轮询的时候，对原日志文件进行copy，不改变原文件，相当于对原文件进行了一个快照，此选项使用的时候，create不生效  
nocopy：对原日志文件不进行拷贝，此选项会覆盖copy选项  
create mode owner group, create owner group：日志轮询后会创建一个新的原来名字的文件，可以设置权限和所属者或者所属组，如create 755 root lile  
nocreate：轮询后不创建，也就是把要轮询的日志轮询后不会产生一个原来名字的文件，此选项会覆盖create  
olddir directory：指定轮询出来的日志文件存放的目录  
noolddir：轮询出来的日志文件放在与原日志文件一样的目录，此选项会覆盖olddir  
createolddir mode owner group：当指定轮询日志存放路径的时候，若路径不存在，则创建  
nocreateolddir： 当指定轮询日志存放路径的时候，若路径不存在，不创建，此选项会覆盖createolddir  
hourly：  日志每小时做一次轮询  
daily：     日志每天做一次轮询  
weekly： 日志每个星期做一次轮询  
monthly：日志每月做一次轮询               
yearly：日志每年做一次轮询  
dateext：轮询出的日志名字后加上日期  
nodateext： 轮询出的日志名字不使用日期的方式，此选项会覆盖dateext  
dateformat format_string：日期的格式  
dateyesterday：今天做的轮询的话，文件名的日期写昨天的  
delaycompress：新的轮询日志不会马上压缩，等到下一个新的轮询日志出现的时候才做压缩，要与compress参数一起使用
nodelaycompress：不会延期压缩，轮询日志后会马上进行压缩，要与compress参数一起使用；此选项会覆盖delaycompress选项   
ifempty：即使日志文件为空，也要做轮询，默认为ifempty  
notifempty：日志文件为空的话，不做轮询，会覆盖ifempty参  
missingok：如果要做日志轮询的日志文件不存在，那么忽略，接着做其他的  
nomissingok：如果要做日志轮询的日志文件不存在，那么不会继续执行后面的，而会返回错误，默认为nomissing  
maxage count：删除指定count天前的日志，如果配置了mail的话，删除的日志将会发送到指定的邮箱  
start count：如果不使用dateext，轮询出的日志文件不会使用日期作为后缀，而会使用数字，star count 可以设置这个数字的开始至，如：start 9，那么就会以9开始，如log_file.9  
rotate count：轮询文件的个数，当个数满了之后，会删除最老的  
size size：只有当原日志文件的大小达到指定的size时，才做轮询，单位自定义    
postrotate/endscript：在日志被轮转后执行             
prerotate/endscript：在日志被轮转前并且有需要轮转才执行          
sharedscripts：当匹配文件夹里时，时间久后，会有很多以原文件加日期命名的轮询日志，而我只需要最匹配最原始的文件做轮询，这个选项就是为了做这个设置；如果没有的话，被轮询出来的日志再下一次轮询时也会被匹配到也做轮询  

### 常用实例
* tomcat日志切割
```bash
vim /etc/logrotate.d/tomcat
```
``` vim
/usr/apache-tomcat-8.5.35/logs/catalina.out {
copytruncate
daily
rotate 10
missingok
dateext
compress
notifempty
}
```
* nginx访问日志每天做日志分割
```bash
vim /etc/logrotate.d/nginx
```
```vim
/var/log/nginx/*log {
    create 0644 nginx nginx
    daily
    rotate 10
    missingok
    notifempty
    compress
    sharedscripts
    postrotate
        /bin/kill -USR1 `cat /usr/local/nginx/logs/nginx.pid 2>/dev/null` 2>/dev/null || true
    endscript
}
```
>  参考：  
https://www.cnblogs.com/lemon-le/p/8476648.html  
https://linux.cn/article-4126-1.html

----








