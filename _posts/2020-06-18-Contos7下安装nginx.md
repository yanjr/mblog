---
layout: post
title: Contos7下安装nginx
author: yanjr
tags:
  - contos7
  - nginx
categories:
  - contos7
date: 2020-06-18 18:28:00 +0800
---


## 配置 EPEL源
``` bash
[root@localhost ~]# sudo yum install -y epel-release
[root@localhost ~]# sudo yum -y update
```
## 安装Nginx

``` bash
[root@localhost ~]# sudo yum install -y nginx
```
安装成功后，默认的网站目录为： /usr/share/nginx/html

默认的配置文件为：/etc/nginx/nginx.conf

自定义配置文件目录为: /etc/nginx/conf.d/

## 开启端口80和443
如果你的服务器打开了防火墙，你需要运行下面的命令，打开80和443端口。
``` bash
[root@localhost ~]# sudo firewall-cmd --permanent --zone=public --add-service=http
[root@localhost ~]# sudo firewall-cmd --permanent --zone=public --add-service=https
[root@localhost ~]# sudo firewall-cmd --reload
```
如果你的服务器是阿里云ECS，你还可以通过控制台安全组，打开80和443端口，或者其他自定义端口。

## 操作Nginx
1.启动 Nginx
``` bash
[root@localhost ~]# systemctl start nginx
```
2.停止Nginx
``` bash
[root@localhost ~]# systemctl stop nginx
```
3.重启Nginx
``` bash
[root@localhost ~]# systemctl restart nginx
```
4.查看Nginx状态
``` bash
[root@localhost ~]# systemctl status nginx
```
5.启用开机启动Nginx
``` bash
[root@localhost ~]# systemctl enable nginx
```
6.禁用开机启动Nginx
``` bash
[root@localhost ~]# systemctl disable nginx
```
