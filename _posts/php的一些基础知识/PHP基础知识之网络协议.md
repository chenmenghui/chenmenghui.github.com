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

 | 代码 | 含义 | 
 | --- | --- | 
 | 1**(信息类) | 表示接收到请求并且继续处理 |
 | 100 | 客户必须继续发出请求|
 | 101 | 客户要求服务器根据请求转换HTTP协议版本|
 | | |
 | 2**(响应成功) | 表示动作被成功接收、理解和接受|
 | 200 | 表明该请求被成功地完成，所请求的资源发送回客户端|
 | 201 | 提示知道新文件的URL|
 | 202 | 接受和处理、但处理未完成|
 | 203 | 返回信息不确定或不完整|
 | 204 | 请求收到，但返回信息为空|
 | 205 | 服务器完成了请求，用户代理必须复位当前已经浏览过的文件|
 | 206 | 服务器已经完成了部分用户的GET请求|
 | | |
 | 3**(重定向类) | 为了完成指定的动作，必须接受进一步处理|
 | 300 | 请求的资源可在多处得到|
 | 301 | 本网页被永久性转移到另一个URL|
 | 302 | 请求的网页被转移到一个新的地址，但客户访问仍继续通过原始URL地址，重定向，新的URL会在response中的Location中返回，浏览器将会使用新的URL发出新的Request。|
 | 303 | 建议客户访问其他URL或访问方式|
 | 304 | 自从上次请求后，请求的网页未修改过，服务器返回此响应时，不会返回网页内容，代表上次的文档已经被缓存了，还可以继续使用|
 | 305 | 请求的资源必须从服务器指定的地址得到|
 | 306 | 前一版本HTTP中使用的代码，现行版本中不再使用|
 | 307 | 申明请求的资源临时性删除|
 | | |
 | 4**(客户端错误类) | 请求包含错误语法或不能正确执行|
 | 400 | 客户端请求有语法错误，不能被服务器所理解|
 | 401 | 请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用|
 | 401.2 | 未授权  服务器配置问题导致登录失败|
 |  401.3 | ACL 禁止访问资源|
 |  401.4 | 未授权 授权被筛选器拒绝|
 | 401.5 | 未授权 ISAPI 或 CGI 授权失败|
 | 402 | 保留有效ChargeTo头响应|
 | 403 | 禁止访问，服务器收到请求，但是拒绝提供服务|
 | 403.1 | 禁止访问 禁止可执行访问|
 | 403.2 | 禁止访问 禁止读访问|
 | 403.3 | 禁止访问 禁止写访问|
 | 403.4 | 禁止访问 要求 SSL|
 | 403.5 | 禁止访问 要求 SSL 128|
 | 403.6 | 禁止访问 IP 地址被拒绝|
 | 403.7 | 禁止访问 要求客户证书|
 | 403.8 | 禁止访问 禁止站点访问|
 | 403.9 | 禁止访问 连接的用户过多|
 | 403.10 | 禁止访问 配置无效|
 | 403.11 | 禁止访问 密码更改|
 | 403.12 | 禁止访问 映射器拒绝访问|
 | 403.13 | 禁止访问 客户证书已被吊销|
 | 403.15 | 禁止访问 客户访问许可过多|
 | 403.16 | 禁止访问 客户证书不可信或者无效|
 | 403.17 | 禁止访问 客户证书已经到期或者尚未生效|
 | 404 | 一个404错误表明可连接服务器，但服务器无法取得所请求的网页，请求资源不存在。eg  输入了错误的URL|
 | 405 | 用户在Request-Line字段定义的方法不允许|
 | 406 | 根据用户发送的Accept拖，请求资源不可访问|
 | 407 | 类似401，用户必须首先在代理服务器上得到授权|
 | 408 | 客户端没有在用户指定的饿时间内完成请求|
 | 409 | 对当前资源状态，请求不能完成|
 | 410 | 服务器上不再有此资源且无进一步的参考地址|
 | 411 | 服务器拒绝用户定义的Content-Length属性请求|
 | 412 | 一个或多个请求头字段在当前请求中错误|
 | 413 | 请求的资源大于服务器允许的大小|
 | 414 | 请求的资源URL长于服务器允许的长度|
 | 415 | 请求资源不支持请求项目格式|
 | 416 | 请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段|
 | 417 | 服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求长。|
 | | |
 | 5**(服务端错误类) | 服务器不能正确执行一个正确的请求|
 | 500 | 服务器遇到错误，无法完成请求|
 | 500.100 | 内部服务器错误 ASP 错误|
 | 500-11 | 服务器关闭|
 | 500-12 | 应用程序重新启动|
 | 500-13 | 服务器太忙|
 | 500-14 | 应用程序无效|
 | 500-15 | 不允许请求 global.asa|
 | 501 | 未实现|
 | 502 | 网关错误|
 | 503 | 由于超载或停机维护，服务器目前无法使用，一段时间后可能恢复正常

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

| 协议头 | 说明 | 示例 | 状态 |
| --- | --- |  --- |  --- | 
| Accept-Encoding | 可接受的响应内容的编码方式。 | <code>Accept-Encoding:gzip,deflate</code> | 固定 |
| Accept-Language | 可接受的响应内容语言列表。 | <code>Accept-Language:en-US</code> | 固定 |
| Accept-Datetime | 可接受的按照时间来表示的响应内容版本 | Accept-Datetime:Sat,26Dec201517:30:00GMT | 临时 |
| Authorization | 用于表示HTTP协议中需要认证资源的认证信息 | Authorization:BasicOSdjJGRpbjpvcGVuIANlc2SdDE== | 固定 |
| Cache-Control | 用来指定当前的请求/回复中的，是否使用缓存机制。 | <code>Cache-Control:no-cache</code> | 固定 |
| Connection | 客户端（浏览器）想要优先使用的连接类型 | <code>Connection:keep-alive</code><p><code>Connection:Upgrade</code></p> | 固定 |
| Cookie | 由之前服务器通过<code>Set-Cookie</code>（见下文）设置的一个HTTP协议Cookie | <code>Cookie:$Version=1;Skin=new;</code> | 固定：标准 |
| Content-Length | 以8进制表示的请求体的长度 | <code>Content-Length:348</code> | 固定 |
| Content-MD5 | 请求体的内容的二进制MD5散列值（数字签名），以Base64编码的结果 | Content-MD5:oD8dH2sgSW50ZWdyaIEd9D== | 废弃 |
| Content-Type | 请求体的MIME类型（用于POST和PUT请求中） | Content-Type:application/x-www-form-urlencoded | 固定 |
| Date | 发送该消息的日期和时间（以<ahref="http://tools.ietf.org/html/rfc7231#section-7.1.1.1"rel="nofollow"target="_blank">RFC7231</a>中定义的"HTTP日期"格式来发送） | Date:Dec,26Dec201517:30:00GMT | 固定 |
| Expect | 表示客户端要求服务器做出特定的行为 | <code>Expect:100-continue</code> | 固定 |
| From | 发起此请求的用户的邮件地址 | <code>From:user@itbilu.com</code> | 固定 |
| Host | 表示服务器的域名以及服务器所监听的端口号。如果所请求的端口是对应的服务的标准端口（80），则端口号可以省略。 | <code>Host:www.itbilu.com:80</code><p><code>Host:www.itbilu.com</code></p> | 固定 |
| If-Match | 仅当客户端提供的实体与服务器上对应的实体相匹配时，才进行对应的操作。主要用于像PUT这样的方法中，仅当从用户上次更新某个资源后，该资源未被修改的情况下，才更新该资源。 | If-Match:"9jd00cdj34pss9ejqiw39d82f20d0ikd" | 固定 |
| If-Modified-Since | 允许在对应的资源未被修改的情况下返回304未修改 | If-Modified-Since:Dec,26Dec201517:30:00GMT | 固定 |
| If-None-Match | 允许在对应的内容未被修改的情况下返回304未修改（304NotModified），参考超文本传输协议的实体标记 | If-None-Match:"9jd00cdj34pss9ejqiw39d82f20d0ikd" | 固定 |
| If-Range | 如果该实体未被修改过，则向返回所缺少的那一个或多个部分。否则，返回整个新的实体 | If-Range:"9jd00cdj34pss9ejqiw39d82f20d0ikd" | 固定 |
| If-Unmodified-Since | 仅当该实体自某个特定时间以来未被修改的情况下，才发送回应。 | If-Unmodified-Since:Dec,26Dec201517:30:00GMT | 固定 |
| Max-Forwards | 限制该消息可被代理及网关转发的次数。 | <code>Max-Forwards:10</code> | 固定 |
| Origin | 发起一个针对<ahref="http://itbilu.com/javascript/js/VkiXuUcC.html"rel="nofollow"target="_blank">跨域资源共享</a>的请求（该请求要求服务器在响应中加入一个<code>Access-Control-Allow-Origin</code>的消息头，表示访问控制所允许的来源）。 | <code>Origin:http://www.itbilu.com</code> | 固定:标准 |
| Pragma | 与具体的实现相关，这些字段可能在请求/回应链中的任何时候产生。 | <code>Pragma:no-cache</code> | 固定 |
| Proxy-Authorization | 用于向代理进行认证的认证信息。 | Proxy-Authorization:BasicIOoDZRgDOi0vcGVuIHNlNidJi2== | 固定 |
| Range | 表示请求某个实体的一部分，字节偏移以0开始。 | <code>Range:bytes=500-999</code> | 固定 |
| Referer | 表示浏览器所访问的前一个页面，可以认为是之前访问页面的链接将浏览器带到了当前页面。<code>Referer</code>其实是<code>Referrer</code>这个单词，但RFC制作标准时给拼错了，后来也就将错就错使用<code>Referer</code>了。 | Referer:http://itbilu.com/nodejs | 固定 |
| TE | 浏览器预期接受的传输时的编码方式：可使用回应协议头<code>Transfer-Encoding</code>中的值（还可以使用"trailers"表示数据传输时的分块方式）用来表示浏览器希望在最后一个大小为0的块之后还接收到一些额外的字段。 | <code>TE:trailers,deflate</code> | 固定 |
| User-Agent | 浏览器的身份标识字符串 | <code>User-Agent:Mozilla/……</code> | 固定 |
| Upgrade | 要求服务器升级到一个高版本协议。 | Upgrade:HTTP/2.0,SHTTP/1.3,IRC/6.9,RTA/x11 | 固定 |
| Via | 告诉服务器，这个请求是由哪些代理发出的。 | Via:1.0fred,1.1itbilu.com.com(Apache/1.1) | 固定 |
| Warning | 一个一般性的警告，表示在实体内容体中可能存在错误。 | Warning:199Miscellaneouswarning | 固定 |

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

--- 

[HTTP状态码大全](https://www.cnblogs.com/jianying/p/7992622.html)
[常用的HTTP请求头与响应头](https://blog.csdn.net/qq_30553235/article/details/79282113)



