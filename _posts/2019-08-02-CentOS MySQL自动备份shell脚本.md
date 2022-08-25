---
layout: post
title: "CentOS MySQL自动备份shell脚本"
author: yanjr
tags: [backup, mysql]
categories: [linux]
date: 2019-08-02 11:31:00 +0800
---
转自：[安love](https://www.cnblogs.com/zzablog/p/9487912.html)

![美女](https://wx4.sinaimg.cn/mw690/006T7jotly1g5kgqqq9g9j30u00u00wt.jpg)

在数据库的日常维护工作中，除了保证业务的正常运行以外，就是要对数据库进行备份，以免造成数据库的丢失，从而给企业带来重大经济损失。
通常备份可以按照备份时数据库状态分为热备和冷备，按照备份数据库文件的大小分为增量备份、差异备份和全量备份。
这里，我们讲解一种全量备份的方法，来实现定时备份数据到mysql脚本文件，并且支持过期删除。

1、新建shell脚本

[mysqlBackup.sh](https://ajax.yanjr.cn/mysqlBackup.sh)


2、修改shell脚本属性，赋予执行权
``` bash
chmod 600 /opt/mysqlBackup.sh
chmod +x /opt/mysqlBackup.sh
```
3、定时执行脚本
``` bash
vi /etc/crontab
```
添加
``` bash
00 03 * * * root /root/mysqlBackup.sh
```
分 时

 

``` bash
vi /var/spool/mail/root #可查看脚本执行日志
```
4、MySQL恢复
``` bash
mysql -u username -p databse < backup.sql
```

               用户名           数据库名    备份sql
