---
title: "Nginx支持WebSocket"
author: Ryan
tags: [websocket]
categories: [nginx]
date: 2019-08-06 18:48:00 +0800
password: 123
---

参考：[散尽浮华](https://www.cnblogs.com/kevingrace/p/9512287.html)



### 错误：<font color=red>Unexpected response code: 400</font>



WebSocket是目前比较成熟的技术了，WebSocket协议为创建客户端和服务器端需要实时双向通讯的webapp提供了一个选择。其为HTML5的一部分，WebSocket相较于原来开发这类app的方法来说，其能使开发更加地简单。大部分现在的浏览器都支持WebSocket，比如Firefox，IE，Chrome，Safari，Opera，并且越来越多的服务器框架现在也同样支持WebSocket。

在实际的生产环境中，要求多个WebSocket服务器必须具有高性能和高可用，那么WebSocket协议就需要一个负载均衡层，NGINX从1.3版本开始支持WebSocket，其可以作为一个反向代理和为WebSocket程序做负载均衡。

WebSocket协议与HTTP协议不同，但WebSocket握手与HTTP兼容，使用HTTP升级工具将连接从HTTP升级到WebSocket。这允许WebSocket应用程序更容易地适应现有的基础架构。例如，WebSocket应用程序可以使用标准HTTP端口80和443，从而允许使用现有的防火墙规则。

WebSocket应用程序可以在客户端和服务器之间保持长时间运行的连接，从而有助于开发实时应用程序。用于将连接从HTTP升级到WebSocket的HTTP升级机制使用Upgrade和Connection头。反向代理服务器在支持WebSocket时面临一些挑战。一个是WebSocket是一个逐跳协议，因此当代理服务器拦截客户端的升级请求时，需要向后端服务器发送自己的升级请求，包括相应的头文件。此外，由于WebSocket连接长期存在，与HTTP使用的典型短期连接相反，反向代理需要允许这些连接保持打开状态，而不是关闭它们，因为它们似乎处于空闲状态。

允许在客户机和后端服务器之间建立隧道，NGINX支持WebSocket。对于NGINX将升级请求从客户端发送到后台服务器，必须明确设置Upgrade和Connection标题。

## Nginx开启websocket代理功能的配置如下：
1）编辑nginx.conf，在http区域内一定要添加下面配置：
```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}
```
map指令的作用：
该作用主要是根据客户端请求中$http_upgrade 的值，来构造改变$connection_upgrade的值，即根据变量$http_upgrade的值创建新的变量$connection_upgrade，
创建的规则就是{}里面的东西。其中的规则没有做匹配，因此使用默认的，即 $connection_upgrade 的值会一直是 upgrade。然后如果 $http_upgrade为空字符串的话，
那值会是 close。

2）编辑vhosts下虚拟主机的配置文件，在location匹配配置中添加如下内容：
```
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "Upgrade";
```

示例如下：
``` 
 location / {
            proxy_pass http://127.0.0.1:8080;
            proxy_set_header Host $host:$server_port;  # 
            proxy_http_version 1.1;						# 表明使用http版本为1.1,支持websocket
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
```







