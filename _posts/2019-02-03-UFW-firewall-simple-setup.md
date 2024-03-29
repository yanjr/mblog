---
layout: post
title: "UFW firewall simple setup"
date: 2019-02-03 10:22:00 +0800
category: [linux]
tag: [linux, ufw, 防火墙, firewall]
---

ufw是一个主机端的iptables类防火墙配置工具，比较容易上手。一般桌面应用使用ufw已经可以满足要求了。

### 安装方法
``` bash
$ sudo apt-get install ufw
```
### 使用方法
1 启用
``` bash
sudo ufw enable
sudo ufw default deny 
```
作用：开启了防火墙并随系统启动同时关闭所有外部对本机的访问（本机访问外部正常）。

2 关闭
``` bash
sudo ufw disable 
```
2 查看防火墙状态
``` bash
sudo ufw status
```
3 开启/禁用相应端口或服务举例
``` bash
sudo ufw allow 80
```
允许外部访问80端口
``` bash
 sudo ufw delete allow 80
```
禁止外部访问80 端口
``` bash
sudo ufw allow from 192.168.1.1
```
允许此IP访问所有的本机端口
``` bash
sudo ufw deny smtp
```
禁止外部访问smtp服务
``` bash
sudo ufw delete allow smtp
```
删除上面建立的某条规则
``` bash
 sudo ufw deny proto tcp from 10.0.0.0/8 to 192.168.0.1 port 22
```
  要拒绝所有的TCP流量从10.0.0.0/8 到192.168.0.1地址的22端口
 可以允许所有RFC1918网络（局域网/无线局域网的）访问这个主机（/8,/16,/12是一种网络分级）：
``` bash
sudo ufw allow from 10.0.0.0/8
sudo ufw allow from 172.16.0.0/12
sudo ufw allow from 192.168.0.0/16
```
### 推荐设置
``` bash
sudo apt-get install ufw
sudo ufw enable
sudo ufw default deny 

```
这样设置已经很安全，如果有特殊需要，可以使用sudo ufw allow开启相应服务。
``` bash
sudo ufw allow 80/tcp
ufw delete allow 80/tcp
```
### 详细使用说明
More info: [Ufw中文使用指南](http://wiki.ubuntu.org.cn/Ufw使用指南)



















