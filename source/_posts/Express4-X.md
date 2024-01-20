---
title: Express4.X
tags:
  - Node.js
  - Express4.X
categories: Node开发
img: /image/18.jpg
summary: 第七章：Express4.X应用框架
abbrlink: cb0e
date: 2020-09-14 22:10:07
---

# Nodejs学习笔记

## 第七章 Express4.X

### Express简介

Express是简介灵活的node.jsWeb应用框架，提供强大的特性帮助创建各种web应用。Express不对node.js已有的特性进行二次抽象，只在它基础上拓展了web应用所需的功能。

Express中主要学习：路由、中间件、模板引擎、其中路由和模板引擎的概念与其他语言一致，比如PHP



### 安装express

```shell
#依赖安装express
npm install express --save
```



### 安装EXpress应用程序生成器

通过应用程序生成器可以快速创建一个应用的骨架

```shell
#安装生成器，如果之前全局安装过，之后可以不用安装
npm install -g express-generator
#创建项目
express --view=ejs projectName   #projectName是项目名 模板引擎为ejs
#创建项目之后进入到项目文件
cd projectName
#将项目文件夹下package.json里的依赖包全部安装
npm i
#启动项目，默认是3000端口
npm start
```



### 项目文件说明

- bin/www文件：启动js，15行左右可以修改服务端口号。

- node_modules目录：存放所有局部安装的包。

- public目录：存放静态文件，如css、image、js文件等。

- routes目录：存放路由文件。

- views目录：存放页面

- app.js文件：项目主文件

- package.json文件：npm配置文件

  npm i：启动该配置文件中dependencies下的依赖包。

  npm start：就是运行该配置文件中scripts下面的start命令。



### 路由

在app.js文件引进路由并开启路由

```js
//引进路由
var listRouter = require('./routes/list');//被引入的文件后缀.js可省略
//开启路由
app.use('/list',listRouter);
```

在routes文件夹下写路由文件（js文件）

```js
const express = require('express');
const router = express.Router();

//get方式请求
router.get('/',function(req,res,next){
    console.log('有人访问list！');
    //req.query属性以对象存放了所有get请求的参数
    console.log(req.query);
    
    //给客户端响应
    // res.send('Welcome list!');

    //render()渲染页面，并返回给客户端
    //第一个参数是页面的名字，页面放在views文件夹下
    //第二个参数是传递给模板引擎的
    res.render('list');//后缀可以省略
})

//post方式请求
router.post('/',(req,res)=>{
    //req.body属性存放了post数据
    console.log(req.body);
})

//动态路由,加上“:”,就成了动态路由，并且通过req.params拿到该数据
router.get('/:name/:age',(req,res)=>{
    console.log(req.params);
    res.send('这是动态路由！')
})

//除了动态路由，地址栏还可以使用正则表达式
//abc+表示ab开始后面至少一个c
router.get('/abc+',(req, res)=>{
    res.send('这是正则！')
})

//访问list下的product网页
router.get('/product',(req,res,next)=>{
    console.log('有人访问list/product!');
    res.send('Welcome list/product!');
})

//定义模块
module.exports = router;
```



### 中间件

#### 中间件简介

Express是一个自身功能极简，完全是由路由和中间件构成一个web开发框架：从本质上来说，一个Express应用就是在调用各种中间件，路由也是特殊的中间件。

中间件是可以访问请求对象（req）、响应对象（res）以及next应用程序请求-响应周期中的函数。使用app.use()来使用/定义中间件。

本质上，中间件就是在请求后，响应前要执行的函数，中间件就是函数使用app.use()方法，可以给express应用添加任意中间件功能函数。

#### 内置中间件

从Express4.X开始，express不再依赖Content，这意味着express将大多数内置中间件都剥离出去了，比如之前的body-parser，cookie-parser等中间件，现使用body-parser，cookie-parser的方法是第三方中间件。

Express4.X内置的中间件是express.static静态资源挂载，Express4.16后又新增了express.json和express.urlencoded，分别用来解析req传入的json格式、urlencoded格式数据。

##### express.static的使用

静态资源指的就是存在于服务器端的文件，例如html/css/js/图片等文件都是静态资源文件，浏览器需要访问，服务器才会返回这些静态资源文件的内容。

express.static()封装了路径和读取/响应文件的方法内，我们可以把某个文件夹暴露给浏览器直接进行路径的访问即可。

```js
const express = require('express');
const app = express();
app.use(express.static(path.join(__dirname, 'public')));//从public下索引前端访问的资源路径
```

这里有个**ejs引擎中的资源路径问题**：ejs项目中，因为有上面这条配置，项目中所有的静态资源引用路径都可以认为ejs文件和静态资源文件在同一个目录下。

##### app.use()虚拟路径使用

第一个参数为虚拟路径（可以理解成浏览器访问路径去掉主目录后的路径），前端必须先访问命中此虚拟路径，才能用接下来的路径匹配后面的路由路径。如果省略第一个参数，则会过滤所有的请求，并且回调函数中的url是（去掉访问路径主目录后的）全地址，因为没有被分组。

```js
const express = require('express');
const app = express();
app.use('/list',routerList);//访问list目录
```

##### 自定义中间件

所谓中间件其实就是一个方法，只需要遵循MiddleWare Funtion就可以了。它有四个参数：

- 第一个参数接收上一个中间件通过next()传递的实参。
- 第二个参数为请求对象，包含了浏览器请求的信息。
- 第三个参数为响应对象，包含了服务器响应的信息。
- 第四个参数为next，方法next()调用下一个中间件函数执行，可以传递参数给下一个中间件。

中间件的执行顺序是从上往下执行的，需要注意调用次序。



### 示例

app.js

```js
var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

//引进路由
var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');
var listRouter = require('./routes/list');
//引入自定义中间件
var customMid = require('./customMiddleWare/customMid1');

var app = express();

// view engine setup
//设置页面的ejs模板引擎
app.set('views', path.join(__dirname, 'views'));//设置了静态页面存放的地址（views）
app.set('view engine', 'ejs');//页面引擎是 ejs

//中间件
//第一个参数：url
//如果省略第一个参数，则会过滤所有的请求。并且回调函数中的url是全地址，因为没有被分组。
app.use(logger('dev'));//打印日志
app.use(express.json());//json序列化客户端传入的数据参数
app.use(express.urlencoded({ extended: false }));//encoded
app.use(cookieParser());//cookie处理
app.use(express.static(path.join(__dirname, 'public')));//返回静态资源，如果是请求静态文件，则返回，并且结束响应。如果不是请求静态文件，则继续往下匹配路由。


//自定义一个中间件
// app.use('/list',(req,res,next)=>{
//   console.log('This is a custom middleware!');
//   console.log(req.url);
//   if(req.url === '/'){
//     res.send('你的访问被拦截，无法访问list！');
//   }else{
//     next();
//   }
//   // next();
// })
 app.use('/list',customMid);//将自定义的中间件通过模块引入

//中间价传值：从上一个中间件next()的参数传值给下一个中间件
//回调函数参数变为四个，第一个参数接收上一个中间件传递的实参
// app.use('/list',(arg,req,res,next)=>{
//   console.log(arg);
//   next();
// })

//启用路由，第一个参数匹配地址的第一段
app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/list',listRouter);

// catch 404 and forward to error handler
//创建错误
app.use(function(req, res, next) {
  next(createError(404));
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  //返回错误页面
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
```

自定义中间件customMid

```js
 function fn(req,res,next){
    console.log('This is a custom middleware!');
    if(req.url === '/'){
      res.send('你的访问被拦截，无法访问list！');
    }else{
      next('这是上一个中间件返回的值！');
    }
    // next();
 }

 module.exports = fn;
```

访问：http://localhost:3000/list，会显示： 你的访问被拦截，无法访问list！ 

