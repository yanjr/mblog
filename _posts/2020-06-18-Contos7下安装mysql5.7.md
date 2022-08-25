---
layout: post
title: Contos7下安装mysql5.7
author: yanjr
tags:
  - contos7
categories:
  - contos7
date: 2020-06-18 18:29:00 +0800
---


## 下载并安装MySQL官方的 Yum Repository
``` bash
[root@localhost ~]# wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
```
  使用上面的命令就直接下载了安装用的Yum Repository，大概25KB的样子，然后就可以直接yum安装了。
``` bash
[root@localhost ~]# yum -y install mysql57-community-release-el7-10.noarch.rpm
```
  之后就开始安装MySQL服务器。
``` bash
[root@localhost ~]# yum -y install mysql-community-server
```
  这步可能会花些时间，安装完成后就会覆盖掉之前的mariadb。

至此MySQL就安装完成了，然后是对MySQL的一些设置。

## MySQL数据库设置

  首先启动MySQL
``` bash
[root@localhost ~]# systemctl start  mysqld.service
```
  查看MySQL运行状态：
``` bash
[root@localhost ~]# systemctl status mysqld.service
```
此时MySQL已经开始正常运行，不过要想进入MySQL还得先找出此时root用户的密码，通过如下命令可以在日志文件中找出密码：
``` bash
[root@localhost ~]# grep "password" /var/log/mysqld.log
```



如下命令进入数据库：
``` bash
[root@localhost ~]# mysql -uroot -p
```
  输入初始密码，此时不能做任何事情，因为MySQL默认必须修改密码之后才能操作数据库：
``` bash
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
mysql> flush privileges;
```

## 添加远程登录用户
默认只允许root帐户在本地登录，如果要在其它机器上连接mysql，必须添加一个允许远程连接的帐户。或者修改 root 为允许远程连接（不推荐）

添加一个允许远程连接的帐户
``` bash
mysql> GRANT ALL PRIVILEGES ON *.* TO 'zhangsan'@'%' IDENTIFIED BY 'Zhangsan2018!' WITH GRANT OPTION;
mysql> flush privileges;
```

修改 root 为允许远程连接（不推荐）
``` bash
mysql> use mysql;
mysql> UPDATE user SET Host='%' WHERE User='root';
mysql> flush privileges;
```
## 设置默认编码为 utf8
mysql 安装后默认不支持中文，需要修改编码。
修改 /etc/my.cnf 配置文件，在相关节点（没有则自行添加）下添加编码配置，如下：
``` bash
[mysqld]
character-set-server=utf8
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
```
重启mysql服务，查询编码。可以看到已经改过来了
``` bash
shell> systemctl restart mysqld
shell> mysql -uroot -p
mysql> show variables like 'character%';
```

## 默认配置文件路径：

配置文件：/etc/my.cnf
日志文件：/var/log/mysqld.log
服务启动脚本：/usr/lib/systemd/system/mysqld.service
socket文件：/var/run/mysqld/mysqld.pid

