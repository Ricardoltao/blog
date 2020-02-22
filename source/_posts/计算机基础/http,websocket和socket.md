---
title: 'TCP、UDP、HTTP、SOCKET、WebSocket之间的区别'
date: 2020-02-20 13:02:36
tags:
- TCP
- UDP
- HTTP
- SOCKET
- WebSocket
categories: 计算机基础
---

# TCP、UDP、HTTP、SOCKET、WebSocket之间的区别

TCP/IP协议族主要分为四层：应用层、传输层、网络层、数据链路层

每层协议都有响应的协议，如下图：
![img](http://images.cnblogs.com/cnblogs_com/goodcandle/socket2.jpg)

## HTTP

应用层协议，HTTP（超文本传输协议）是利用TCP在两台电脑（通常是Web服务器和客户端）之间传输信息的协议。客户端使用Web浏览器发起HTTP请求给Web服务器，Web服务器接收到请求处理后发送响应给客户端。



## SOCKET

Socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。socket是在应用层和传输层之间的一个抽象层，它把TCP/IP层复杂的操作抽象为几个简单的接口供应用层调用已实现进程在网络中通信。



## TCP与UPP

### TCP

传输控制协议（Transmission Control Protocol）

**优点**：面向连接、传输可靠（保证数据正确性）、有序（保证数据顺序）、传输大量数据

速度慢、对系统资源要求多、程序结构较复杂。

每一条TCP连接只能是点对点，TCP首部开销20字节。



### UDP

用户数据报协议（User Data Protocol）

无连接、传输不可靠（丢包）、无序、传输少量数据、速度快、对系统资源要求少、程序结构较简单

UDP支持一对一、一对多、多对一和多对多的交互通信

UDP首部开销小，只有8个字节



## Websocket

Websocket协议解决了服务器与客户端全双工通信的问题

Websocket协议包含两部分：一部分是握手，一部分是数据传输

### 什么是单工，半双工，全双工

信息只能单向传送为单工

信息双向传送但不能同时双向传送为半双工

信息能够同时双向传送则称为全双工



## WebSocket和Socket区别

可以把WebSocket想象成HTTP(应用层)，HTTP和Socket什么关系，WebSocket和Socket就是什么关系。

HTTP 协议有一个缺陷：通信只能由客户端发起，做不到服务器主动向客户端推送信息。

它的最大特点就是，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。
