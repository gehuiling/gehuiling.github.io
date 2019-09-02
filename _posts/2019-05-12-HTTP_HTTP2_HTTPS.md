---
layout: post
title:  "HTTP、HTTP2、HTTPS"
date:   2019-05-12 11:31:00
categories: [计算机网络]
tags: [HTTP,HTTP2,HPPTS]
comments: true
---

HTTP(Hyper Text Transfer Protocol)，是用于从万维网(WWWW：World Wide Web)服务器传输超文本到本地浏览器的传输协议。HTTP基于TCP/IP通信协议来传递数据。
<!--more-->

- HTTP 是无连接的：每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
- HTPP 是媒体独立的：只要是客户端或服务端可以处理的数据都可以传输。
- HTTP 是无状态的：请求连接断开之后，不会再有记忆，下一次不会记得上一次的请求内容。

`浏览器显示的内容都有 HTML、XML、GIF、Flash 等，浏览器是通过 MIME Type 区分它们，决定用什么内容什么形式来显示。MIME Type 是该资源的媒体类型，MIME Type 不是个人指定的，是经过互联网（IETF）组织协商`

## HTTP

### HTTP 消息结构

##### 客户端请求消息

客户端发送一个HTTP请求到服务器的请求消息包括：**`请求行方法+URL+协议）`**、**`请求头部`**、**`空行`**、**`请求数据`** 四个部分。

<img src="/image/posts/blog1501.png" style="display:block;margin:0 auto;"> 

##### 服务器响应消息

HTTP响应由四部分组成：**`状态行（协议+状态码+状态码文本描述）`**、**`消息报头`**、**`空行`**、**`响应正文`** 

<img src="/image/posts/blog1502.jpg" style="display:block;margin:0 auto;"> 

### HTTP请求方法

- HTTP1.0 定义了三种请求方法： GET, POST 和 HEAD

- HTTP1.1 新增了六种请求方法：OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT

### HTTP状态码

下面是常见的HTTP状态码：
- 200 - 请求成功
- 301 - 资源（网页等）被永久转移到其它URL
- 404 - 请求的资源（网页等）不存在
- 500 - 内部服务器错误

具体的：
* 1XX，继续，请求已接受，继续处理<br/>
100 继续<br/>
101 切换协议，例如切换到新的HTTP版本协议
* 2XX，成功                         
200 响应成功<br/>
201 成功请求并创建了新的资源<br/>
202 已经接受请求，但没有处理完成<br/>
203 非授权信息，请求成功，但是返回的信息不在原始服务器，而是一个副本<br/>
204 服务器成功处理，但是没有返回内容<br/>
206 服务器成功处理了部分GET请求
* 3XX，需要进一步操作才能完成请求   
304 请求没有被修改 <br/>
302 临时移动，重定向<br/>
305 使用代理，请求的资源需要通过代理来访问
* 4XX，客户端发生错误               
400 客户端语法错误<br/>
401 请求用户的身份认证<br/>
403 服务端理解客户端的请求，但是拒绝执行<br/>
404 服务器无法根据客户端的请求找到资源
* 5XX，服务端发生错误               
500 服务器内部错误<br/>
502 充当网管或代理的服务器，从远端服务器收到了一个无效请求<br/>
501 服务器不支持请求的功能<br/>
505 服务器不支持请求的hTTP协议版本，无法完成请求

## HTTP2

- **HTTP2使用的是二进制传送，HTTP1.X是文本（字符串）传送**
HTTP1.X使用的是明文的文本传送，而HTTP2使用的是二进制传送，二进制传送的单位是帧和流。

- **HTTP2支持请求和响应的多路复用**
因为有流ID，所以通过同一个http请求实现多个http请求传输变成了可能，可以通过流ID来标示究竟是哪个流从而定位到是哪个http请求

- **HTTP2头部压缩**
HTTP2通过gzip和compress压缩头部然后再发送，同时客户端和服务器端同时维护一张头信息表，所有字段都记录在这张表中，这样后面每次传输只需要传输表里面的索引Id就行，通过索引ID就可以知道表头的值了

- **HTTP2支持服务器推送**
HTTP2支持在客户端未经请求许可的情况下，主动向客户端推送内容。也就是说，除了对原始请求的响应之外，服务器还可以向客户端推送额外的资源

*补充*：
+ HTTP 2.0 所有http请求都建立在一个TCP请求上，实现多路复用。
+ HTTP 2.0 浏览器可以在发现资源时立即分派请求，指定每个流的优先级，让服务器决定最优的响应次序
+ HTTP 2.0 支持服务器到客户端的主动推送机制。
+ HTTP/2.0 通过支持首部字段压缩和在同一连接上发送多个并发消息，让应用更有效地利用网络资源，减少感知的延迟时间。

## HTTP与HTTPS
* HTTP明文传输协议，不适合传输一些敏感或者保密的数据
* HTTPS加密传输协议，用于传输保密和敏感的数据，在HTTP下加入了SSL层，在传输过程中客户端与服务端需要验证CA证书，在握手阶段还会协商出一个加密的密钥。

### SSL握手

* Client端请求Https连接，同时发送client_random和支持的加密方式
* server端返回CA证书和server_random（经过server端私钥加密）
* 验证CA证书，使用证书中公钥加密的permaster_secret(随机数)
* client发送permaster_secret至server端，server端通过私钥解密
* client和server端同时通过client_random / server_random/ premaster_secret生成master secret

### CA证书验证

* server端发来证书+CA的signature之后
* client获取CA的公钥来解码server证书中的signature
* 解码完成之后，就确认了signature的正确性，得到message digest和代签名的内容（证书内容）
* 对签名内容进行哈希，得到message digest
* 比较两次message digest是否相同来验证证书内容是否有效
