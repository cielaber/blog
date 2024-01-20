---
title: Node.js基础
tags: Node.js
categories: Node开发
cover: false
img: /image/14.jpg
summary: 第四章：Node.js基础
abbrlink: 42fa
date: 2020-09-13 21:43:56
---

# Node开发学习笔记

## 第四章 Node.js基础

### Node js

Node js基于google v8引擎，在服务器端支持JavaScript的一种运行环境。

Node.js主要用于编写想Web服务器一样的网络应用，这和PHP、Pyhton是类似的，但是Node.js与其他语言最大的不同之处在于，PHP等语言是阻塞的而Node.js是非阻塞的。Node.js是事件驱动的，开发者可以在不使用线程的情况下开发出一个能够承载高并发的服务器。其他服务端语言难以开发高并发应用，即使可以，性能也不尽人意。Node.js正是在这个前题下被创造出来。Node.js把JS的易学易用和Unix网络编程的强大结合在了一起。

Node.js和其他语言相比：

- Node.js不是一种独立语言，与PHP、JSP、Python的“既是语言又是平台”不同，Node.js使用JavaScript进行编程，运行在JavaScript引擎上（V8）。
- 与PHP，JSP相比（PHP、JSP、.net都需要运行在服务器程序上，Apache、Nginx、IIS。），Node.js跳过了这些HTTP服务器，它自己不用建设在任何服务器软件之上。Node.js的许多设计理念与经典架构（LAMP=Linux+Apache+MySQL+PHP）有很大的不同，可以提供强大的伸缩能力。

#### Node js特点

- ##### 单线程

  在Java、PHP等服务端语言中，会为每个客户端连接创造一个新的线程，因此，用户数量越多，服务器数量就越多，成本越大。

  Node.js不为每个客户创建新的线程，而仅仅使用一个线程。Node.js处理请求是单线程的，但是后台拥有一个I/O线程池。当有用户连接了，就触发一个内部事件，通过非阻塞I/O、事件驱动机制，让Node.js宏观上也是并行的。

  PHP等多线程中，每个线程耗费大约2M，一个8GB内存服务器同时连接的最大用户数量为4000个。使用Node.js一个8BG内存可以同时处理4万用户的连接。

  单线程的好处还有：操作系统完全不再有线程创建、销毁的时间开销。但是坏处是，一个用户造成了线程的崩溃，整个服务器都崩溃了，其他用户也会崩溃。

- ##### 非阻塞I/O

  I/O操作指的是对磁盘的读写操作。异步I/O的概念和同步I/O相对。一个异步过程调用发出之后，调用者不能立即得到结果。实际处理这个调用的部件在完成后，通过状态、通知和回调来通知调用者。在一个CPU密集型的应用中，有一些需要处理的数据可能放在磁盘上，预先直到这些数据位置，所以预先发起异步I/O读请求。等到真正需要用到这些数据的时候，再等待异步I/O完成。使用了异步I/O，在发起I/O请求到实际使用这些数据的这段时间内，程序还可以继续做其他事情。

  当某个I/O执行完毕时，将以事件的形式通知执行I/O操作的线程，线程执行这个事件的回调函数。为了处理异步I/O，线程必须有事件循环，不断的检查有没有未处理的事件，依次予以处理。

- ##### 事件驱动

  在Node.js中，客户端请求建立连接，提交数据等行为，会触发相应的事件。在Node.js中，在一个时刻，只能执行一个事件回调函数，但是在执行一个事件回调函数时，可以转而处理其他事件（比如，又有新用户连接了），然后返回继续执行原事件的回调函数，这种处理机制，称为“事件环”机制。

  Node.js底层是C++(V8也是C++写的)。在底层代码中，近半数都用于事件队列、回调函数队列的构建。用事件驱动来完成服务器的任务调度。用一个线程，担负了处理非常多的任务使命。

由于单线程，如果一个事情进入，但是I/O被阻塞了，这个线程就被阻塞了。

但由于非阻塞I/O，不会等待I/O结束，而会执行后面的语句。

但如果在处理A业务的同时，B的I/O回调完成怎么办？

事件机制（事件环），不管是新用户的请求还是老用户的I/O完成，都将以事件方式加入事件换，等待调度。

#### Node.js适合开发什么？

Node.js善于I/O，不善于计算。因为Node最擅长的就是任务调度，如果业务上有很多的CPU计算，实际上也相对于这个计算阻塞了单线程，就不适合Node.js开发。

当应用程序需要大量并发的I/O，而在客户端发出响应之前，应用程序内部不需要非常复杂处理的时候，Node.js非常合适。Node.js也非常适合与web socket配合，开发长连接的实时交互应用程序。如：

- Web服务API，比如REST。
- 实时多人游戏
- 后端的Web服务，如跨域、服务端的请求。
- 基于Web的应用
- 多客户端的通信，如即时通信。

#### Node.js安装

Node.js官网：https://nodejs.org/en/

在LTS(稳定版)下，根据不同平台系统选择需要的Node.js安装包下载。安装过程中，均默认即可，安装目录可自定义选择。

#### 顶层对象

- 浏览器：window

- node js：global

- ES6为了统一顶层对象，建议顶层对象为：globalThis。

  在浏览器中它会自动为window，在node js中它自动为global。

  并且，**ES6中顶层对象的属性与全局变量脱离关系，即ES6中通过let、const等声明的全局变量不再global属性上。**

#### 默认的全局变量

- __filename：当前js文件名(绝对路径)
- __dirname：当前js文件路径(绝对路径)



### Buffer

#### 创建方式

- 指定大小创建，没有值时，默认为0

  ```js
  var buffer = Buffer.alloc(10);
  console.log(buffer);//<Buffer 00 00 00 00 00 00 00 00 00 00>
  ```

- 用字符串创建，类型必须是字符串(建议使用)

  ```js
  var buffer = Buffer.from('create a buffer');
  console.log(buffer);//<Buffer 63 72 65 61 74 65 20 61 20 62 75 66 66 65 72>
  ```

#### buffer转字符串

Buffer.toString()，带一个参数，参数是编码方法，一般为utf-8。

```js
var buffer = Buffer.from('create a buffer');
var str = buffer.toString('utf-8');
console.log(str);//create a buffer
```

#### 写入缓存

Buffer.write(string[,offset[,length]] [,encoding])

参数分别是：写入缓冲的字符串、缓冲区开始写入的索引值（默认为0）、写入字节的长度（默认全度写入）、使用的编码方式（默认utf8），除第一个外其他都可省略。

返回实际写入的大小，如果buffer空间不足，则只会写入部分字符串。

```js
var buffer = Buffer.alloc(10);
buffer.write('abcd');
console.log(buffer);//<Buffer 61 62 63 64 00 00 00 00 00 00>
```

#### 读取缓存

Buffer.toString([encoding[,start[,end]]])

三个参数均可省略，分别是编码方式（默认utf8）、指定读取开始位置、结束位置。

返回读取的指定编码的字符串。

```js
buf = Buffer.alloc(10);
buf.write('abcd');
console.log(buf.toString());//abcd
```

#### 将Buffer转换为JSON对象

语法：Buffer.toJSON()，返回一个JSON对象。

当字符串实例化一个Buffer实例时，JSON.stringify()会隐式地调用toJSON()。

```js
const buf = Buffer.from('12345');
const json = buf.toJSON();
console.log(json)//{ type: 'Buffer', data: [ 49, 50, 51, 52, 53 ] }
```

#### 缓冲区合并

语法：Buffer.concat(list[,totalLength])，返回一个新合成的Buffer。

参数描述：

- list：用于合并的Buffer对象数组列表。
- totalLength：指定合并后Buffer对象总长度。

```js
var buffer1 = Buffer.from('张三');
var buffer2 = Buffer.from('18');
var buffer3 = Buffer.concat([buffer1,buffer2]);
console.log(buffer3.toString());//张三18
```

#### 缓冲区长度

Buffer.length，返回Buffer对象所占据的内存长度。

```js
var buf = Buffer.from('张三18岁');
console.log(buf.length);//11
```



### 模块系统

模块：按照特定格式写出来的js文件。

Nodejs中采用CommonJS规范，按照规范书写和引用js文件。

#### 定义模块

- module.exports. = { 自定义名1:模块名1,自定义名2:模块名2 }，简写为module.exports. = { 模块名1,模块名2 }：推荐使用
- module.exports.自定义名 = 模块名
- exports.自定义名 = 模块名：不推荐使用，因为一旦文件中出现了module.exports，用exports方法引用就失效了，它们不能同时出现。

#### 引用模块

```js
var module = require(fileName);
```

module：变量存放模块，可以是对象；require：模块引用关键字；filename：被引用的js文件名，后缀.js可以省略。

模块定义

```js
function sum(a,b){
    console.log(a+b);
}

function sub(a,b){
    console.log(a-b);
}

module.exports = {sum,sub};
```

模块引用

```js
var module = require('./25-module定义');

module.sum(2,3);//5
module.sub(5,2);//3
```

#### 模块标识

模块标识其实就是传递给require()方法的参数，它必须是符合小驼峰命名的字符串，或者相对路径/绝对路径，它可以没有文件名后缀.js。模块的定义十分简单，接口也十分简洁。它的意义在在于将聚类的方法和变量等限定在私有作用域中。每个模块具有独立的空间，它们互不干扰，在引用时也显得干净利落。

模块的标识就是模块的名字或路径，Node.js通过模块的标识来寻找模块。

对于核心模块，直接使用模块的名字对其进行引入。

对于自定义的模块，需要通过文件的路径来对模块进行引入。

#### 内置模块/核心模块

nodejs存在内置模块，require()总是会优先加载核心模块。

- http模块

  ```js
  let http = require('http');
  ```

- fs模块

  ```js
  let fs = require('fs')
  ```

- 事件模块

  ```js
  let EventEmitter = require('events').EventEmitter;
  ```

  

### 事件模块

大多数Node.js核心API构建于惯用的异步事件驱动架构，其中某些类型的对象（又称触发器，Emitter）会触发命名事件来调用函数（又称监听器，Listener）。例如，net.Server会在每次有连接时触发事件，fs.ReadStream会在打开文件时触发事件，stream会在数据可读时触发事件。

所有能触发事件的对象都是EventEmitter类的实例。这些对象有一个eventEmitter.on()函数，用于将一个或多个函数绑定到命名事件上。事件的命名通常是驼峰式的字符串。当EventEmitter对象触发一个事件时，所有绑定在该事件上的函数都会被同步地调用。

#### 事件注册

```js
//加载事件模块
let EventEmitter = require('events').EventEmitter;
//实例化一个事件对象
let event = new EventEmitter();
//实例提供了两个方法
event.on('event1',(e)=>{
	console.log('event1事件发生了')
})
event.emit('event1');//把事件触发
```

nodejs中没有固定事件，事件是自定义的，也存在事件对象e。event.emit()中第一个参数为事件名称，之后所有的参数都会当作参数传到事件的功能函数中，第一个参数事件名称不会当作参数传过去。

```js
let EventEmitter = require('events').EventEmitter;
let event = new EventEmitter();
event.on('event2',(...args)=>{
	console.log(args);
})
event.emit('event2',10,20,30);
//[ 10, 20, 30 ]
```

除了用on方法进行注册事件之外，还有以下方法进行事件注册。

```js
//addListener()方法注册事件
let EventEmitter = require('events').EventEmitter;
let event = new EventEmitter();
event.addListener('event3',()=>{
	console.log('This is event3!');
})
event.emit('event3');

//once()方法注册事件，该方法注册的事件只能只能执行一次，之后会被注销
event.once('event4',()=>{
	console.log('This is event4!')
});
setInterval(()=>{
	event.emit('event4');
},1000)
```

#### 事件移除

```js
//该方法接收两个参数，一个是事件名称，第二个是回调函数名称
//注意：该方法移除的事件，事件在添加事件时，必须是通过函数名添加的，如果是一个匿名函数添加事件，是没法用该方法移除的。
event.removeListener('eName',callback);
```

```js
//移除该事件的所有事件
event.removeAllListeners('eName');
```

#### 数量限制

默认情况下，EventEmitters 如果你添加的监听器超过 10 个就会输出警告信息。 setMaxListeners 函数用于提高监听器的默认限制的数量。 

