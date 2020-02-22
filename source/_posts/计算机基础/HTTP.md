---
title: HTTP
date: 2020-02-20 15:52:57
tags: HTTP
categories: 计算机基础
---
# HTTP

超文本传输协议（HyperText Transfer Protocol）

HTTP协议是建立在TCP协议基础之上的，当浏览器需要从服务器获取网页数据时，会发出一次HTTP请求。HTTP会通过TCP建立起一个到服务器的连接通道，当本次请求完毕后，HTTP会立即将TCP连接断开，这个过程很短。所以HTTP连接是一个短连接，是一种无状态连接。

## http传输流

![img](https://user-gold-cdn.xitu.io/2018/4/17/162d19df66466f6b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 状态码

常用状态码：

### 2XX 成功

+ 200 OK，表示从客户端发来的请求在服务器端被正确处理
+ 204 No content，表示请求成功，但响应报文不含实体的主体部分
+ 206 Partial Content，进行范围请求

### 3XX 重定向

+ 301 moved permanently，永久性重定向，表示资源已被分配了新的URL
+ 302 found，临时性重定向，表示资源临时被分配了新的URL
+ 303 see other，表示资源存在着另一个URL，应使用GET方法定向获取资源
+ 304 not modified，表示服务器允许访问资源，但因发生请求未满足条件的情况
+ 307 temporary redirect，临时重定向，和302含义相同

### 4XX 客户端错误

+ 400 bad request，请求报文存在语法错误
+ 401 unauthorized，表示发送的请求需要有通过 HTTP 认证的认证信息
+ 403 forbidden，表示对请求资源的访问被服务器拒绝
+ 404 not found，表示在服务器上没有找到请求的资源

### 5XX 服务器错误

+ 500 internal sever error，表示服务器端在执行请求时发生了错误
+ 503 service unavailable，表明服务器暂时处于超负载或正在停机维护，无法处理请求

