---
title: PHP基础知识之网络协议
date: 2018-09-06
categories: 
- PHP
- PHP基础知识
tags: PHP基础知识
---
HTTP/1.1中

HTTP协议状态码

五类响应: 1xx,2xx,3xx,4xx,5xx

延伸: OSI七层模型
- 物理层
    - 建立,维护,断开物理连接
- 数据链路层
    - 建立逻辑连接,进行硬件地址寻址,差错校验等功能
- 网络层
    - 进行逻辑地址寻址,实现不同网络之间的路径选择
- 传输层
    - 定义传输数据的协议端口号,以及流控和差错校验
    - 协议有:TCP UDP,数据包一旦离开网卡即进入网络传输层
- 会话层
    - 建立,管理,终止会话
- 表示层
    - 数据的表示,安全和压缩
- 应用层
    - 网络服务和最终用户的一个接口
    - 协议有HTTPS,FTP等
    
延伸:HTTP协议的工作特点和工作原理
- 工作特点
    - 基于B/S模式
    - 通信开销小,简单快速,传输成本低
    - 使用灵活,可使用超文本传输协议
    - 节省传输时间
    - 无状态
- 工作原理
    - 客户端发送请求给服务器,创建一个TCP连接,指定端口号(默认80),连接到服务器,服务器监听浏览器请求,一旦监听到客户端请求,分析请求类型后,服务器会想客户端返回状态信息和数据内容
延伸:HTTP协议常见请求/响应头和请求方法

> 常见请求/响应头

- Content-Type
- Accept
- Origin
- Cookie
- Cache-Control
- User-Agent
- Referrer
- X-Forwarded-For
- Access-Control-Allow-Origin
- Last-Modified

> 请求方法

get,post,head,options,put,delete,trace

get和post的区别

延伸:HTTPS协议的工作原理

HTTPS是一种基于ssl/tls的http协议,所有的http数据都是在ssl/tls协议封装之上传输的

HTTPs协议在http协议的基础上,添加了ssl/tls握手以及数据加密传输,也属于应用层协议

延伸:常见网络协议含义及端口

- ftp 文件传输
- telnet 远程登录
- smtp 简单邮件传输协议
- pop3 接收邮件
- http 超文本传输协议
- dns 用于域名解析



