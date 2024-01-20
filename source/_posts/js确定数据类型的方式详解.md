---
title: js确定数据类型的方式详解
tags: JavaScript
categories: JavaScript
img: /image/26.jpg
summary: js确定数据类型的四种方式
abbrlink: 36a8
date: 2020-11-20 19:52:27
---

# JavaScript学习笔记

## js确定数据类型

### 1、typeof

适用与简单数据类型

```js
let s = "abcd"
console.log(typeof s)//string

let num = 1
console.log(typeof num)//number

let b = true
console.log(typeof b)//bollean

let n = null
console.log(typeof n)//object

let u;
console.log(typeof u)//undefined

let obj = new Object()
console.log(typeof obj);//object

let arr = new Array()
console.log(typeof arr);//object

let fun = new Function()
console.log(typeof fun);//function
```

由此可见，该方法除了对除null之外的简单数据类型有效外，对引用类型没有得出我们想要的结果。

解释：

- null在js中被认为是一个特殊的对象，是一个空指针对象，所以返回object。
- 创建Function或者Array这些类型的实例的时候，其实都是基于Object实例进行的一种扩展，所以都归为object。至于Function会返回function的原因，JavaScript权威指南中给出的解释是，函数有它特有的特性。

### 2、instanceof

instanceof检测的是原型，适合用来检测引用类型。（关于原型，参考阅读：[面向对象编程](https://www.eternitywith.xyz/posts/c9c1.html)）

A instanceof B用来判断A是否是B的实例，如果A是B的实例则返回true，否则返回false。

但是也有个缺点，因为function和array都是object的实例，所以当它们对object进行检测时，也都返回true。

```js
let fun = new Function()
console.log(fun instanceof Function)//true
console.log(fun instanceof Object)//true

let arr = new Array();
console.log(arr instanceof Array)//true
console.log(arr instanceof Object)//true

let obj = new Object();
console.log(obj instanceof Object)//true
```

### 3、constructor

constructor是原型对象的一个属性，它指向了对象原型本身。

constructor是原型对象的属性，之所以我下面的例子可以直接用实例.constructor判断是因为这里涉及到了原型链。

```js
console.log("".constructor)//string()
console.log(new Number(1).constructor);//Number()
console.log(true.constructor);//Boolean()
console.log([].constructor);//Array()
console.log(new Function().constructor);//Function()
console.log({}.constructor);//Object()
```

但是这种方法判断也是有问题的：

- null和undefined没有constructor，所以无法用这种方法判断。
- 如果原型对象被重写，那么constructor就可能被丢失。

### 4、Object.prototype.toString.call()

toString()是Object的原型方法，调用该方法，默认返回当前对象的[[class]]。

```js
console.log(Object.prototype.toString.call('abc'));// [object String]
console.log(Object.prototype.toString.call(123));// [object Number]
console.log(Object.prototype.toString.call(true));// [object Boolean]
console.log(Object.prototype.toString.call({}));//[object Object]
console.log(Object.prototype.toString.call([]));// [object Array]
console.log(Object.prototype.toString.call(new Function()));//[object Function]
console.log(Object.prototype.toString.call(null));// [object Null]
console.log(Object.prototype.toString.call(undefined));// [object Undefined]
```

该方法是最合适用来判断数据类型的方法。