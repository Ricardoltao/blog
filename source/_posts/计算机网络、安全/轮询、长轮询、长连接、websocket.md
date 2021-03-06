---
title: 轮询、长轮询、长连接、websocket
date: 2020-03-11 16:39:51
tags: 
- 轮询
- SSE
- websocket
categories: 网络
---
# 轮询、长轮询、SSE、websocket

## 短轮询和长轮询

短轮询和长轮询的目的都是用哦关于实现客户端和服务端的一个即时通讯



### 短轮询

短轮询的基本思路就是浏览器每隔一段时间向浏览器发送 http 请求，服务器端在收到请求后，不论是否有数据更新，都直接进行响应。

这种方式实现的即时通信，本质上还是浏览器发送请求，服务器接收请求的一个过程，通过让客户端不断的进行请求，使得客户端能够模拟实时地收到服务端的数据变化

**优缺点**：

这种方式的优点就是比较简单，易于理解。

缺点是这种方式由于需要不断的建立 http 连接，严重浪费了服务端和客户端的资源。当用户增加时，服务端的压力就会变大。



### 长轮询

长轮询的基本思路是，首先由客户端向服务端发送请求，当服务端收到客户端发来的请求后，服务端不会直接进行响应，而是先将这个请求挂起，然后判断服务器数据是否有更新。如果有更新则进行响应，如果一直没有数据，则到达一定的事件限制才返回。客户端 JavaScript 响应处理函数会在处理完服务器返回的信息后，再次发出请求，重新建立连接。

**优缺点**：

长轮询和短轮询比起来，它的优点是明显减少了很多不必要的 http 请求次数，相比之下节约了资源。缺点是连接挂起也会导致资源的浪费。



## SSE（Sever-Send Events）

SSE是 H5 新增的功能，它可以允许服务推送数据到客户端。SSE在本质上就与之前的长轮询和短轮询不同，虽然都是基于 http 协议，但是轮询需要客户端先发送请求。而 SSE 最大的特点就是不需要客户端发送请求，可以实现只要服务端数据有更新，就可以马上发送到客户端。

SSE优点就是不需要建立或保持大量的客户端发送服务端的请求，节约了很多资源，提升应用性能。



## WebSocket

上面三种方式本质上都是基于 http 协议的，我们还可以使用 WebSocket 协议来实现。

WebSocket 是 H5 定义的一个新协议，与传统的 http 协议不同，该协议允许由服务端主动的向客户端推送信息。使用 WebSocket 协议的缺点是在服务端的配置比较复杂。

websocket 是一个全双工协议，也就是通信双方是平等的，可以相互发送消息，而 SSE 的方式是单向通信，只能有服务端向客户端推送信息，如果客户端需要发送消息就是属于下一个 http 请求了。



> 参考资料：https://cloud.tencent.com/developer/article/1076547