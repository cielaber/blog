---
title: JavaScript-数据类型
tags: JavaScript
categories: JavaScript
img: /image/1.jpg
cover: false
summary: 第二章：JavaScript数据类型
abbrlink: 2ddf
date: 2020-08-22 22:13:48
---

# JavaScript学习笔记



## 第二章 JavaScript数据类型

### 1、数据类型

#### 1-1、为什么需要数据类型？

不同的数据类型占用的内存大小不一样，为了合理有效地分配内存空间，划分了数据类型。

#### 1-2、数据类型分类

- 简单数据类型：Number、String、Boolean、Undefined、Null

- 复杂数据类型：Object、Array、Function

#### 1-3、Number类型

Number即数值类型，主要用于参与数学运算，包含：整数、小数、进制、无穷大、无穷小、NaN

注意：

- NaN证明运算表达式是一个非法运算，算数运算不能得到正常地数字都会返回NaN，并且任意两个NaN都是不相等的。
- 精度缺失，小数之间进行运算，由于计算机是通过二进制进行计算，存储时会导致精度缺失，**因此不能使用小数作为判断条件**。解决方法：将运算的小数乘以100，再将结果除以100。

#### 1-4、String类型

即字符串类型，用单引号或双引号引起来的数据，有长度有下标，只可读不可写。需要注意的是，所有从键盘输入到界面的数据都是字符串类型。

基本的操作方法有：

- .length：获取字符串长度，包括空格。
- 字符串[下标]：获取对应下标的字符，ie7及以下获取到undefined。
- .charAt(下标)：获取字符串对应下标的字符，下标从0开始。
- .charCodeAt(下标)：返回对应下标的字符在ASCII表中的值。数字范围：48-57，大写字母范围：65-90，小写字母范围：97-122。

#### 1-5、Boolean类型

真：true（1）、假：false（0），数字0和空字符串为假，其他都为真。

if语句中的判断条件，都会转成Boolean类型。

#### 1-6、null和undefined

null：Null类型，是js中的一个关键字，代表空值，代表一个空指针对象，使用typeof运算得到“object”，存放null的变量存在堆中，null值本身存在栈中，可以认为他是特殊的对象值。

undefined：undefined类型，当变量声明了未初始化时，得到的就是undefined。在ECMAScript第三版引入，为了区分空指针对象和未初始化的变量，它是一个预定义的全局变量。没有返回值的函数为undefined，没有实参的形参也是undefined。

#### 1-7、Object

万物皆对象，通过键值对的方式存储数据，键值对之间以逗号隔开。

声明方式

- new实例化对象

  ```js
  var obj = new Object();
  ```

- 字面量形式声明对象

  ```js
  var obj = {
  	...;
  };
  ```

#### 1-8、Function

函数的作用是用来存储代码块，调用时才执行，typeof运算结果为function。

声明方式

```js
function 函数名(){
	代码块;
};
```

调用方式：函数名()

#### 1-9、Array

数组可以存放任意类型的数据，有长度有下标（从0开始），typeof运算结果为object。

#### 1-10、拓展

在ECMA中，除了Number，String，null，undefined，Boolean，其它的实例都会归为object。而创建Function或者Array这些类型的实例的时候，其实都是基于Object实例进行的一种扩展。只是多了一些特有属性。判断一个对象是一个数组还是普通的Object可以使用Array.isArray的方法。然而这个方法会因为在跨iframe的情况下失效。最简单的办法是Object.prototype.toString.call(array)的方式。因为只有通过Object的原型的toString方法才能拿到每个实例的[class]内部属性。 

```js
var list = [1,2,3,4];
console.log(Object.prototype.toString.call(list));//[object Array]

function fn(){
	console.log(1);
};
console.log(typeof fn);//function
console.log(Object.prototype.toString.call(fn));//[object Function]
```

### 2、数据类型转换

- 强制类型转换

  - Number()

    将其他数据类型转为数值类型，返回一个新的数值，不改变变量本身。

    只能转换纯数字和空字符串(0)，其他的都是NaN，Boolean类型也能转换但是是隐式转换。

  - parseInt()

    转换为Number类型，取整，舍弃小数部分。

    该方法还有第二个参数radix，**代表第一个参数以什么进制参与转换，最终的结果都是转为十进制**。注意：当省略第二个参数且数字以0开头时，在IE8及以下浏览器中会将它当作八进制进行转换，所以建议一般加上第二个参数。

  - parseFloat()

    转换为Number类型，保留小数，最后的0自动省略。

    parseFloat().toFxid(n)，代表保留小数点后n位。

  > parseInt()与parseFloat()方法解析的原则是**尽可能从前往后解析，若有能实别的数字就解析，遇到不能实别的就结束转换，即使后面还有数字也不转换**。如果第一位就不是数字就返回NaN。

  - isNaN()

    is not a number是不是不是一个数字类型，内部调用Number()方法，如果能成功转成数字类型，返回false，不成功返回true。

  - 变量.toString()和String()

    两个方法将其他类型转成字符串类型，不会改变原来的值。

    String(变量)能针对任何数据类型，而变量.toString()不能转换undefined和null。

  - Boolean()

    可以将任何值转为布尔值。

    除了undefined、null、false、0、NaN、空字符串 这六个值之外，其他值均是真值。包括{}和[]。

### 3、运算符

- 算数运算符：+ - * / % ++ --
- 赋值运算符：= += -= *= /=
- 比较运算符：> < >= <= == !==  !=  === 
- 逻辑运算符：&& || ！
- 三目运算符：条件 ？ 条件成立的代码 ： 条件不成立的代码

### 4、数据类型的隐式转换

- “+”进行运算时，只要两边有一边是字符串，“+”就当作字符串连接符，这时候会将两边的数据都调用String()转成字符串进行拼接。两边都是数字或Boolean、null时，才是算数运算符，这时将调用Number()方法将两边的数据转成Number类型进行运算。

  {% asset_img 2-1.jpg %}

- 当关系运算符两边有一边是字符串时，会将其他数据类型使用Number()转换，然后比较。

- 当关系运算符两边都是字符串时，会同时转成Number然后比较关系，但是此时并不是按照Number()的形式转成数字，而是按照字符串对应的ASCII编码，比较ASCII的大小，并且是从左往右依次比较，如果不相等直接得出结果，如果相等则继续比较第二个字符。

  {% asset_img 2-2.jpg %}

- 复杂数据类型在隐式转换时会先转成String，然后再转成Number运算。

  {% asset_img 2-3.jpg %}

- 空数组的toString()方法会得到空字符串，而空对象的toString()方法会得到字符串‘[object Object]’。

  {% asset_img 2-4.jpg %}

  {% asset_img 2-5.jpg %}



说明：隐式转换参考（及图片）自：传智播客官方博客 文章：https://blog.csdn.net/itcast_cn/article/details/82887895，感谢。