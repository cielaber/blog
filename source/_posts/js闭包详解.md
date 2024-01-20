---
title: 一文详解js执行上下文、作用域链、闭包、this之间的关系
tags: JavaScript
categories: JavaScript
summary: 通过执行上个下文、作用域、深刻理解闭包和this
img: /image/27.jpg
abbrlink: d94f
date: 2020-11-23 09:52:55
---

# JavaScript学习笔记

## 一文详解js执行上下文、作用域链、闭包、this之间的关系

闭包是js中的一大难点，想要理解闭包，只知道它的特性是不够的，只有知道了它的执行原理才能融汇贯通运用它。

这篇文章就通过js执行上下文、作用域链这几个js的重难点知识来理解闭包，以及js中的另一个难点this。

本篇篇幅较长，但是讲解详细，如果有基础可以根据目录跳着看。

### 1、执行上下文

js中的执行上下文是一个比较抽象的概念，js中变量或函数的执行上下文决定了它们可以访问哪些数据及行为。

**变量对象**：每个上下文都有一个关联的变量对象，它保存了这个上下文中定义的所有变量和函数。如果上下文是函数，则其活动对象就是变量对象。

**全局上下文**：全局上下文是最外层的上下文（根据ECMAScript实现的宿主环境，全局上下文可能会不一样。），在浏览器中通常的全局上下文是window对象，所有var定义的变量和函数都会成为window对象的属性和方法（let和const声明的变量和函数不会）。

函数在调用的时候会创建自己的执行上下文，当代码执行留进入函数的时候，函数的上下文被推到一个上下文栈上，在函数执行完毕之后，上下文栈会弹出该函数上下文。

### 2、作用域链

上下文中的代码在执行的时候，会创建变量的作用域链，作用域链决定了各级上下文中的代码在访问变量和函数时的顺序。

函数的变量对象最初只有arguments(全局上下文中没有这个变量)，当当前上下文中每声明一个变量或这函数时，就会把该变量或加入到变量对象中。

#### 函数定义

每声明一个函数，就会生成一个该函数的作用域链，该作用域链预装载全局变量对象，并存于函数的[[Scope]]（域）中。

{% asset_img 闭包-1.png %}

#### 函数调用

调用该函数时，就会生成自己的执行上下文，并复制[[Scope]]生成作用域链。然后生成活动对象，把该活动对象放入到该函数作用域链的最前端。

{% asset_img 闭包-2.png %}

每创建一个内嵌函数，内嵌函数就会把父作用域链中的活动对象放入到自己的作用域链中，并把自己活动对象加入到它作用域链的最前端。

{% asset_img 闭包-3.png %}

函数内部使用变量时，能从自己作用域链的最前端依次往后查找，直到找到全局变量对象。

而全局上下文对象始终在每个作用域的最后，也就是说，所有函数都能访问全局作用域中的变量。

#### 函数执行完毕

对于上下文而言，自己上下文中的所有代码都执行完毕后，该上下文包括定义在它上面的所有变量和函数都会被销毁。

注意：在函数嵌套中，虽然外部函数的arguments对象在外部函数的活动对象中，并且存在在了内部函数的作用域内，但由于每个函数都有自己的arguments对象和this，内部函数也有自己的arguments和this，所以**内部函数并不能直接访问外部函数的的arguments和this**。

如果内部函数需要访问外部函数的arguments和this，就需要用另外的变量存储它（arguments或this），该变量就能通过作用域链被内部函数访问到，这也就是下面要讲的闭包的最经典案例：函数嵌套。

### 3、闭包

闭包是指引用了另一个函数作用域中变量的函数。

上面讲了，当函数某个函数执行完毕，该函数的上下文会被销毁。但是如果有另一个函数是闭包，它引用了该函数中的变量，由于js的垃圾回收机制，被执行完毕的函数被销毁时，会保留它的活动对象，使闭包中的函数依旧能使用其中的变量。

#### 闭包使变量长期存在

```JS
const countFun = function(){
    let count = 0;
    count++;
	console.log(count)
    return count;
}
countFun()//1
countFun()//1
```

这样的常规函数，每次调用执行完之后，都会被销毁，所以再一次调用，count变量重新赋值为0，每次调用结果都是1。如果想每次调用，能保存上一次的count记录，然后count在原来的基础上+1，我们可以用闭包来实现。

```js
const countFun = (function(){
    let count = 0;
    return (function(){
        count++;
        console.log(count)
        return count;
    })
})()
countFun()//1
countFun()//2
```

首先解释一下函数的书写：首先声明外部匿名函数，并返回了一个内部匿名函数，然后执行了外部匿名函数，相当于把外部匿名函数的执行结果（也就是内部匿名函数）返回给了countFun。

为什么说这里用到了闭包？

因为外部匿名函数是个自执行函数（即声明时调用），这时候它已经执行完了，按理外部匿名函数上下文被销毁，它的活动对象也会被销毁，但是由于内部函数通过作用域链用了外部函数的count变量（内部函数成了一个闭包），所以外部函数的活动对象不会被销毁，count一直被保存着。每次调用countFun()就相当于执行了内部函数，count自增。

注意不要搞混了，count依旧是在外部函数的活动变量，并没有跑到内部函数中，因为它是外部函数的上下文中创建了，它只是没有被销毁，通过作用域链被内部函数一直使用着。

#### 闭包避免全局变量污染

上面这种方法也避免了全局变量的污染，由于自执行函数，创建后被执行，对于全局变量对象来说，count是被销毁了的，因此可以重新用var定义count变量。

```js
const countFun = (function () {
    var count = 0;
    return (function () {
        count++;
        console.log(count)
        return count;
    })
})()
var count = 55;
console.log(count)//55
countFun() //1
countFun() //2
```

#### 闭包模拟私有属性

```js
function MyObject(v){
	var value = v;
    return{
		changeValue:function(){
			value++;
            return value;
        }
    }
}
var obj = new MyObject(10);
console.log(obj.changeValue())//11
console.log(obj.value)//undefined
```

上述代码中value就相当于对象obj的私有属性，因为前面作用域链讲了，函数形成的私有作用域，外部不能直接访问该作用域的变量。通过函数返回一个对象，模拟了实例化对象的方法和属性，而value只能通过changeValue方法进行访问。

这里对比一下通过构造函数创建对象。

```js
function MyObject(v) {
    var value = v;
    this.changeValue = function () {
        value++;
        return value;
    }
}
var obj = new MyObject(10);
console.log(obj.changeValue()) //11
console.log(obj.value) //undefined
```

这里的value也相当于obj的私有属性，外部不能访问，只能通过changValue访问。

那为何构造函数里通过this添加的方法和属性能被外部访问？这里就要谈到new的作用了。

创建一个实例，使用new操作符之后，会进行以下操作：

- 在内存中创建一个新对象。
- 新对象内的[[prototype]]特性被赋值为构造函数的prototype属性。
- 构造函数中的this指向了新对象。
- 执行构造函数内的代码，即创建属性和方法。
- 如果构造函数返回一个非空对象，则返回该非空对象，否则返回新创建的对象。

这就能解释为何this添加的属性在外部能被直接访问了。

#### 闭包的优点

- 变量长期保存，不被销毁。
- 避免全局变量的污染。
- 模拟私有属性。

#### 闭包的缺点

- 内存常驻，增加内存使用量。
- 使用不当容易造成内存泄漏。

### 4、this

主要有以下几种形式：

- this在函数中是函数调用时所在的执行上下文。如果是在全局作用域中调用，则在非严格模式下this是指widow，严格模式下则是指undefined。
- 在对象中，this指实例的对象本身。
- 在方法（对象中的函数）中，this指方法所属对象本身。
- 在事件中，this指触发事件的对象。

由于函数中有时候比较复杂，this指向并不明确，我们通过案例来理解一下：

```js
var name = "window"

let object = {
    name:'object',
    getName(){
        return function(){
            return this.name;
        }
    }
}
console.log(object.getName()())//window
```

上面说了方法中的this指向对象本身，这里却指向了window，是因为这里方法返回了一个匿名函数，调用的时候`object.getName()`实际上是匿名函数本身，后面再加一个()是匿名函数被调用，匿名函数被调用时所在的执行上下文就是全局window，函数的this指向就是全局上下文window。而且上面作用域链的时候也讲过，内部函数无法直接访问外部函数的this，两者的原理是一样的。

那我们需要获得对象的name怎么办？

这就需要我们上面讲到的闭包了。虽然被返回的匿名函数this指向不是object，但是getName()方法的中的this指向是object对象，所以可以通过新建一个变量把保存getName()中的this，返回的匿名函数使用该变量，匿名函数就成了一个闭包。代码如下：

```js
var name = "window"

let object = {
    name:'object',
    getName(){
        let that = this;
        return function(){
            return that.name;
        }
    }
}
console.log(object.getName()())//object
```

