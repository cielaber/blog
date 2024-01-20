---
title: Node.js-Web模块
tags:
  - Node.js
  - Web服务器
  - HTTP
categories: Node开发
img: /image/17.jpg
summary: 第六章：Node.js Web模块
abbrlink: 4fce
date: 2020-09-14 23:11:48
---

# Node开发学习笔记

## 第六章 Node.js Web模块

### 什么是Web服务器？

Web服务器一般指网站服务器，是指驻留与因特网上某种类型计算机的程序，Web服务器的基本功能，就是提供Web信息浏览服务。它只需要支持HTTP协议、HTML文档格式及URL，与客户端的网络浏览器配合。

大多数web服务器都支持服务端的脚本语言php、python、ruby等，并通过脚本语言从数据库获取数据，将结果返回给客户浏览器。

目前最主流的web服务器是Apache、Nginx、IIS。



### Web应用架构

- Client：客户端，一般指浏览器，浏览器可以通过HTTP协议向服务器请求数据。
- Server：服务端，一般指Web服务器，可以接收客户端请求，并向客户端发送响应数据。
- Business：业务层，通过Web服务器处理应用程序，如数据库交互、逻辑运算、调用外部程序等。
- Data：数据层，一般由数据库组成。



### 使用Node创建Web服务器

Node.js提供了http模块，http模块主要用于搭建HTTP服务端和客户端，使用HTTP服务器或客户端功能必须调用http模块。

```js
var http = require('http')
```

```js
//创建一个服务，如果有url访问到了这个服务，回调函数执行。
//回调函数形参request代表请求对象，里面存储请求的信息。
//形参response代表响应对象
http.createServer((request,response)=>{})
//把服务监听3000端口号
serve.listen('3000',(err)=>{})
```

request对象中有以下属性：

- url：请求的url地址

response响应对象中有如下方法：

- write()：向响应对象添加信息
- end()：代表本次响应结束
- writeHead(status,{‘Content-Type’:‘text/html;charset=utf-8’})：设置响应头，有两个参数，第一个参数是状态码，第二个参数是对象，包含响应类型和编码格式。



### HTTP

HTTP协议（超文本传输协议）是因特网上应用最为广泛的一种网络传输协议，所有的www文件都必须遵守这个标准。HTTP是一个基于TCP/IP通信协议来传递数据。（HTTP协议属于应用层，TCP/IP协议属于传输层和网络层，关于网络通信详见[TCP/IP协议](https://baike.baidu.com/item/TCP/IP%E5%8D%8F%E8%AE%AE/212915)百度百科。）

#### 特点

- 支持客户端/服务器模型

- 简单快速

  向服务器请求服务时，只需要传送请求方法和路径。请求方法常用的有GET、POST、HEAD。每种方法规定了客户于服务器的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度快。

- 灵活

  HTTP允许传输任意类型对的数据对象，正在传输的类型由Content-Type加以标记，Content-Type类型可以百度MIME类型。

- 无连接

  无连接的含义是限制每次连接只处理一个请求，服务器处理完客户端的请求，然后响应，并且收到应答之后就断开连接，这种方式可以节省传输时间。

- 无状态

  HTTP协议是无状态协议，无状态协议是指协议对于事物处理没有记忆能力，这种方式的坏处就是如果后续的处理需要用到之前的信息，则必须要重传，这样就导致了每次连接传输的数据量增大。好处就是如果后续的连接不需要之前提供的信息，响应就会比较快，而为了解决HTTP的无状态特性，出现了Cookie和Session技术。

#### URL

HTTP协议是一个基于请求和应答模式的，存在于传输层的之上的应用协议，是一个无状态的协议，通常是基于TCP的连接方式。HTTP的URL是一中特殊类型的URL，包含了用于定位查找某个网络资源的路径，格式为包括如下：

- 协议
- 域名/IP
- 端口号
- 相对路径

#### HTTP请求

HTTP请求信息由如下三部分组成：

- 请求方法URL协议/版本
- 请求头（Request Header）
- 请求正文

#### HTTP响应

在接收和解释请求信息后，服务器返回一个HTTP响应信息。

HTTP响应由三部分组成：

- 状态行，常见状态代码、状态描述、说明有：
  - 200：OK	//客户端请求成功
  - 400：Bad Request    //客户端请求有语法错误，不能被服务器所理解
  - 401：Unauthorized    //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用。
  - 403：Forbidden    //服务器收到请求，但是拒绝提供服务。
  - 404：Not Found    //请求资源不存在，如URL错误
  - 500：Internal Server Error    //服务器发生不可预期的错误
  - 503：Server Unavailiable    //服务器当前不能处理客户端的请求，一段时间后可能恢复正常
- 消息报头：响应报头后述
- 响应正文：服务器返回的资源内容

#### HTTP工作过程

- 客户端连接到web服务器

  一个HTTP客户端，通常是浏览器，与Web服务器的HTTP端口（默认80）建立一个TCP套接字连接。

- 客户端发送HTTP请求

  通过TCP套接字，客户端向Web服务器发送一个文本的请求报文，一个请求报文由请求行、请求头、空行、请求体四个部分构成。

- 服务器就收解释请求并返回HTTP响应

  Web解析请求，定位请求资源，服务器将资源复写到TCP套接字，由客户端获取，一个响应由状态行、响应头、空行、响应数据4个部分。

- 客户端释放TCP连接

  若Connection模式为close，则服务器主动关闭TCP连接，客户端被动关闭TCP连接，释放TCP连接。若Connection为keepalive，则该连接会保存一段时间，该时间内可以持续使用该连接接收请求，做出响应。



### 常用模块

#### path模块

专注于文件路径

#### URL模块

专注于处理url地址

#### querystring

专注于url中使用get请求后面参数解析