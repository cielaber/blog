---
title: JavaScript-初识
tags: JavaScript
categories: JavaScript
img: /image/0.jpg
cover: false
summary: 第一章：JavaScript初识
abbrlink: '4160'
date: 2020-08-22 22:00:15
---

# JavaScript学习笔记



## 第一章 JavaScript初识

### 1、JavaScript介绍

#### 1-1、什么是JavaScript？

JavaScript是一种具有面向对象能力的、解释型的弱类型程序设计语言，是基于对象和事件驱动的并且具有相对安全性的、可跨平台的客户端脚本语言。（不需要在一个语言环境下运行，而只需要支持它的浏览器即可，浏览器可以直接解析js代码。）

#### 1-2、JavaScript的组成？

- 核心(ECMAScript)
- 文档流对象(DOM)
- 浏览器对象模型(BOM)

#### 1-3、JavaScript的特点

- 解释性
- 基于对象
- 事件驱动
- 跨平台

#### 1-4、JavaScript的作用

- 客户端云计算
- 客户端表单合法验证
- 浏览器对象的调用
- 浏览器事件的触发
- 网页特殊显示效果制作
- …

### 2、JavaScript引入方式

- 行间引入，在标签内部使用js代码，不建议使用。
- 通过script标签内嵌JavaScript代码，一般放在head标签中或body标签末尾，建议放在body标签末尾。
- 通过script标签的src属性外链js文件。

> js操作a标签时比较特殊，代码如下：
>
> ```js
> <a href='javascript:;'></a>
> ```

### 3、JavaScript注释

- 单行注释：//
- 多行注释：/**/

### 4、JavaScript入门

#### 4-1、入门三部曲

- 谁？哪个标签
- 事？触发了什么事件
- 做？事件触发之后做了什么事

#### 4-2、window.onload

window.onload的作用是，当文档和资源都加载完成后执行里面的代码。

当我们把script标签放到head里面去获取元素的时候，会发现获取到的值为null，是因为代码从上往下执行，当获取标签的时候，标签还没有被加载。

```js
window.onload  = function(){
	...
}
```

window.onload每个页面只能出现一次，如果重复，后面的会覆盖前面的，如果window.onload之外的内容多，如页面图片资源过大会导致页面等待时间过长，用户体验差。

更优质的方法是使用事件监听，当页面结构(HTML)加载完成后加载监听事件，可以出现多次。

```js
window.addEventListener("load",function(){
    ...;
});
```

#### 4-3、js的鼠标事件

- onclick	单击事件
- ondblclick	双击事件
- onmouseover	移入事件（支持冒泡）
- onmouseout	移出事件（支持冒泡）
- onmouseenter	鼠标移入事件（不支持冒泡）
- onmouseleave	鼠标移出事件（不支持冒泡）
- onmousedown	鼠标按下事件
- onmouseup	鼠标松开事件
- onmousemove	鼠标移动事件
- oncontentmenu	右键菜单事件

### 5、JavaScript变量

#### 5-1、定义方式

使用var关键字声明。

- 1）声明的同时赋值。

  ```js
  var num = 10;
  ```

- 2）同时定义多个变量并赋值，每个变量之间用逗号隔开。

  ```js
   var a= 10,b = 20,c = 30; 
  ```

- 同时定义多个变量，并且分别赋值，每个变量之间用逗号隔开。

  ```js
  var a,b,c;
  a = 10;
  b = 20;
  c = 30;
  ```

注意：变量要先声明（定义+赋值）后使用。

#### 5-2、变量的命名规则

- 只能以数字、字母、下划线、$开头。
- 不能以数字开头。
- 不能使用关键字、保留字（现在不是关键字但以后可能成为关键字）。
- 具有语义化，见名知义。
- 采用驼峰命名法。

### 6、JavaScript调试语句

- alert()

  js自带的弹出框，具有阻塞作用，只在测试语句时使用。

- console.log()

  在控制台打印数据，一次可以多个，并且不会阻塞。

- prompt()

  用于显示可提示用户进行输入的对话框，也具有阻塞作用。

### 7、JavaScript获取元素

- document.getElementById()

  通过id值获取，只返回一个元素。

- document.getElementsByTagName()

  通过标签名称获取，获取的标签会形成一个伪数组返回（即使只有一个元素也是返回一个集合），使用其中的具体标签时，需要加下标。

- document.getElementsByClassName()

  通过类名称获取，获取的标签会形成一个伪数组返回（即使只有一个元素也是返回一个集合），使用其中的具体标签时，需要加下标。

- document.querySelector()

  该方法接收一个css选择符(与css中如何书写选择符规则相同，如：.nav li > p)，只返回匹配的第一个元素。

- document.querySelectorAll()

  该方法也是接收一个css选择符，但是返回的是符合匹配的所有元素集合。

### 8、JavaScript操作元素内容

- 操作表单元素

  获取表单元素：表单元素.value

  设置表单元素：表单元素.value = 值

- 操作闭合标签内容

  获取内容：标签.innerHTML，标签.innerText

  设置内容：标签.innerHTML = ‘值’，标签.innerText = ‘值’

  注意：

  - 操作闭合标签的内容会有覆盖作用，后面写的会覆盖前面写的。

  - innerHTML能识别标签，而innerText不能识别标签。

    ```js
    var p = document.querySelectorAll('p');
    p[0].innerHTML = '<span>一个span</span>';
    //一个span
    p[1].innerText = '<span>一个span</span>';
    //<span>一个span</span>
    ```

### 9、JavaScript操作元素属性

- 获取属性：标签.属性名
- 设置属性：标签.属性名 = ‘属性值’

注意：class是保留字，不能直接使用，操作标签的class属性语法为：标签.className。

上面的操作都是使用点操作符，也可以使用中括号操作符。某一个具体的属性名用点操作符，**当属性存在在变量中，使用中括号操作该属性**。

### 10、JavaScript操作元素样式

- 获取属性：元素.style.样式名
- 设置属性：元素.style.样式名 = ‘值’

注意：添加类似font-size、background-color这种由‘-’连接而成的属性时，需要去掉‘-’，并且把后面的单词首字母大写，符合驼峰命名法。

上面的方法每次只能设置一种样式，cssText可以同时设置多种样式，cssText本质是设置HTML元素的style属性，但是它会覆盖原来用cssText给标签设置的行内样式（不会覆盖原css样式），可以使用+操作符解决覆盖问题。在样式值前面加‘ ; ’可以解决兼容问题。

```js
box.style.cssText  += ";width:100px;height:50px;";
```

使用js设置的样式最终都成为行内样式。

### 11、其他

- js中除了数字和变量，其他数据操作时需要用引号引起来。
- js中的单引号与双引号没有区别，但是嵌套使用时需要成对出现，一般双引号嵌套单引号。
- 一般情况下js每一条语句结束后换行之后，不加分号对后面的语句没有影响，但是建议加上分号，使代码更美观可读，不加分号可能会引起某些不知名错误。
- js区分大小写。