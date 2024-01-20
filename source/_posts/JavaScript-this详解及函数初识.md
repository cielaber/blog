---
title: JavaScript-this详解及函数初识
tags: JavaScript
categories: JavaScript
img: /image/3.jpg
cover: false
summary: 第四章：js中this详解及函数初识
abbrlink: f835
date: 2020-08-23 19:01:19
---

# JavaScript学习笔记



## 第四章 this详解及函数初识

### 1、this

- 在方法中，this表示该方法所属的对象本身。（对象中的函数称为方法）

  ```js
  var obj = {
      name:'张三',
      fn:function(){
  		console.log(this);
      }
  }
  obj.fn();
  //结果是：{name:'张三',fn:function()}，说明这里的this是obj对象本身。
  ```

- 如果单独使用，this表示全局对象。

  ```js
  console.log(this);
  //结果是：Window
  ```

- 在函数中，this表示全局对象。

  ```js
  function myFunction() {
      return this;
  }
  console.log(myFunction());
  //结果是：Window
  ```

- 在函数中，在严格模式下，this是未定义的(undefined)。

  ```js
  "use strict";
  function myFunction() {
    return this;
  }
  //结果是：undefined
  ```

  > 补充：严格模式
  >
  > > 定义：严格模式是指在严格的条件下运行js代码，在ECMAScript5引入，通过在脚本或函数的头部添加“use strict”来声明。    
  >
  > > 好处：
  > >
  > > - 消除语法的不合理、不严谨之处，保证代码的运行安全。
  > > - 提高编译器效率，增加运行速度。
  > > - 为未来新版本的js做铺垫。  
  >
  > > 限制：
  > >
  > > - 不允许使用未声明的变量。
  > > - 不允许对变量或函数使用delete操作符。
  > > - 不允许重命名变量。
  > > - 不允许使用八进制。
  > > - 抛弃with语句。
  > > - 不可对只读对象赋值，不可对不可配置对象使用delete操作符。
  > > - 禁止this关键字指向全局对象。
  > > - 不可再if内部声明函数。
  > > - 不允许使用转义字符。
  > > - 不允许对一个使用getter方法读取的属性进行赋值。
  > > - 不允许删除一个不允许删除的属性。
  > > - 变量不能使用“eval、arguments”字符串。
  > > - 在作用域eval()创建的变量不能被调用。

- 在事件中，this指向了接收事件的HTML元素，即触发事件的对象。

- 在call()、apply()、bind()三个函数对象的方法中，允许切换函数执行的上下文环境，即this绑定的对象，所以他们可以将this指向任何地方。

  ```js
  var name = '张三',
      age = 17;
  var obj = {
      name: '李四',
      objAge: this.age,
      objFun: function (fm, t) {
          console.log(this.name + '年龄' + this.age, '来自' + fm + "去往" + t);
      }
  }
  var db = {
      name: '王五',
      age: 99
  }
  
  obj.objFun.call(db, '成都', '上海');
  obj.objFun.apply(db, ['成都', '上海']);
  obj.objFun.bind(db, '成都', '上海')();
  obj.objFun.bind(db, ['成都', '上海'])();
  
  // 王五年龄99 来自成都去往上海
  // 王五年龄99 来自成都去往上海
  // 王五年龄99 来自成都去往上海
  // 王五年龄99 来自成都, 上海去往undefined
  
  /*从上面四个结果不难看出:
  
  bind 返回的是一个新的函数，方法后面多了个 () ，必须调用它才会被执行。
  
  call 、bind 、 apply 这三个函数的第一个参数都是 this 的指向对象，第二个参数差别就来了：
  
  call 的参数是直接放进去的，第二第三第 n 个参数全都用逗号分隔，直接放到后面 obj.myFun.call(db,'成都', ... ,'string' )。
  
  apply 的所有参数都必须放在一个数组里面传进去 obj.myFun.apply(db,['成都', ..., 'string' ])。
  
  bind 除了返回是函数以外，它的参数和 call 一样。
  
  当然，三者的参数不限定是 string 类型，允许是各种类型，包括函数 、 object 等等！*/
  ```

### 2、自定义属性

自定义属性是指给标签添加已有属性以外的属性，例如div标签的id、class这些属性都是已有的，如果再添加一个tag属性，就是我们自定义的。

```html
<div class='box' tag=true>box</div>
```

```js
var div = document.getElementsByTagName('div')[0];
//无法直接获取在标签内直接定义的自定义属性
console.log(div.tag);//undefined
div.tag = true;
//可以获取在js中定义的自定义属性
console.log(div.tag);//true
```

### 3、自定义索引（下标）

索引一般与下标一一对应，但是下标并不是标签本身的属性，只是标签所在集合的属性，所以要通过自定义下标来保存标签的实际下标，一般通过for循环。

```html
<div></div>
<div></div>
<div></div>
```

```js
var div = document.getElementsByTagName('div');
for(var i = 0;i < div.length;i++){
    div[i].index = i;
}
console.log(div[1].index);//1
```

### 4、函数

函数是由事件驱动的或者当它被调用时执行的可重复使用的代码块。先声明，后调用。

#### 4-1、函数声明

- function直接声明

  ```js
  function fn(){
  	...;
  };
  ```
  
- 表达式声明/匿名函数赋值给变量

  ```js
  var fn = function(){
  	...;
  };
  ```
  
- 有名函数赋值给变量

  ```js
  var fn = function fun(){
  	...;
  };
  ```
  
- Function构造器构造函数（不推荐）

  ```js
  var fn = new Function('a','b',"alert(a+b)");
  fn(a,b);
  ```
  

 **`Function` 构造函数**创建一个新的 `Function` **对象**。直接调用此构造函数可用动态创建函数，但会遇到和 [`eval`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/eval) 类似的的安全问题和(相对较小的)性能问题。然而，与 `eval` 不同的是，`Function` 创建的函数只能在全局作用域中运行。

- 匿名函数

  ```js
  ;(function(){console.log(1);})()
  //匿名函数直接使用会报错，因为js规定函数声明function后面必需有函数名或者匿名函数使用表达式赋给变量。
  //解决方法是将匿名函数变成为自执行函数，将其用括号括起来，后面加()代表函数调用，其前面加上;或~防止被前面没加;的代码影响。
  ```

#### 4-2、函数调用

函数名()

#### 4-3、函数的参数

- 形参

  函数声明时定义的参数，实际就是一个未赋值的变量。

- 实参

  函数调用时传入的实际参数，多个参数时用逗号隔开。

js函数中的参数类型可以是js数据类型中的任何一个，但是null和undefined作为参数没有任何意义而且有时还会报错，因此基本不会将这两个作为参数。

#### 4-4、arguments

当参数不一定的时候，可以选择不设置形参，而是在函数内部通过获取arguments得到实参，然后再进行操作。

```js
function sum(){
    var sum = 0;
    for(var i = 0;i < arguments.length;i++){
		sum += arguments[i];
    }
    console.log(sum);
}
sum(1,2,3,4);//10
```

arguments可以改变实参的值。

```js
function fn(a){
	arguments[0] = 3;
    console.log(a);
};
fn(1);//3
```

如果形参名与函数内局部变量名相同，后面的会覆盖前面的。

```js
var function fn(b){
	console.log(b,arguments[0]);
    var b = 3;
    console.log(b,arguments[0]);
};
fn(1);
//1 1
//3 3
```

### 5、函数中的问题

- 同名的函数，后面的函数会覆盖前面的函数。
- 实参给形参赋值时从左往右，实参可以比形参多也可以比形参少，如果实参比形参少，未赋值的参数为undefined。