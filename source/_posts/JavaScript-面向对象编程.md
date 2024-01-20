---
title: JavaScript-面向对象编程
tags: JavaScript
categories: JavaScript
summary: js面向对象编程、原型、原型链
img: /image/23.jpg
abbrlink: c9c1
date: 2020-11-20 16:05:44
---

# JavaScript学习笔记

## 第十三章 面向对象编程

### 1、面向对象概念

#### 1-1、对象基本概念

ECMAScript有两种开发模式：

- 函数式：面向过程的开发模式，每个步骤都需要自己完成，注重的是实现过程。
- 面向对象： 将对象作为程序的基本单元，将程序分解为数据和操作的集合。 

面向对象基本概念：

- 类：类是对象的类型模板，将具有同一类属性和方法的对象抽象出来称为一个类。
- 实例：实例时根据类创建的对象，是类的个体表现。

但是ECMAScript中没有类的概念，因此它的对象也基于类的语言中的对象有所不同。

#### 1-2、对象的组成及特点

对象组成：

- 属性：对象的特征描述，静态，名词。
- 方法：对象的行为，动态，动作功能。

对象的基本特征：

- 封装
- 继承
- 多态

### 2、面向对象的创建

#### 2-1、字面量创建

```js
var obj = {
    name:'张三',
    age:18,
 	run:function (){
		console.log(this.name);
    }
};
obj.run();//张三
```

#### 2-2、new实例创建

```js
var box = new Object();
box.name = '张三';
box.age = 18;
box.run = function(){
	console.log(this)
};
box.run();//box对象本身
```

该方法创建对象有个缺点，想创建类似的对象就会产生大量的代码。

#### 2-3、工厂模式创建对象

工厂模式创建对象，其实就是函数封装，将创建对象、添加属性和方法的操作封装在函数中，最后返回这个创建出来的对象，在需要创建对象的时候调用这个函数，就能够得到一个创建好的对象。所有的加工都在函数中，而且可以批量生产，因此叫生产模式。

```js
function createObject(name,age){
	var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.run = function(){
		console.log(this);
    }
    return obj;
};
var man1 = createObject('张三',18);
var man2 = createObject('李四',20);
man1.run();//man1对象本身
man2.run();//man2对象本身
```

工厂模式解决了重复实例化的问题，但是还有一个问题，那就是识别问题，因为根本无法搞清楚他们到底是哪个对象的实例，如下：

```js
function createObject(name, age) {
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.run = function () {
        console.log(this);
    }
    return obj;
};
var man1 = createObject('张三', 18);
console.log(typeof man1);//Object
console.log(man1 instanceof createObject);//false
console.log(man1 instanceof Object);//true
```

instanceof的作用 是测试它左边的对象是否是它右边的类的实例，返回 boolean 的数据类型。 以上案例中，man1应该是由createObject实例创建，但是它却返回false，而承认man1是Object的实例。

#### 2-4、构造函数创建对象

构造函数创建对象，是使用较多的创建对象的方式，它也是将所有的操作都封装在函数中，但是与工厂模式的创建方式有很大不同，而且可以明确区分对象的种类。

构造函数创建对象的特点：

- 构造函数首字母大写（为了区分普通函数，不是必须是约定）
- 构造函数方法没有显示的创建对象（new Object）
- 直接将属性和方法赋值给this对象
- 没有return语句，不需要返回对象
- 通过构造函数创建对象，必须使用new运算符（直接调用跟普通函数一样）

```js
function Person(name){
    this.name = name;//不用new Object 直接通过this添加属性和方法
    this.eat = function(){
        console.log('I am eating');
    }
};
var man1 = new Person('张三');//该方法实例化一个对象必须使用new
console.log(man1 instanceof Person);//true
```

以上这种调用构造函数创建对象，实际上会经历以下四个步骤：

- 创建一个新对象；
- 将构造函数的作用域赋给新对象（this就指向了这个新对象）；
- 执行构造函数中的代码（为这个新对象添加属性）；
- 返回新对象（构造函数会自动return this）。

new的作用：

- 在内存中开辟一块内存
- 让this指向这个内存
- 把对象的属性和方法放到该内存里面
- 把对象的属性和方法返回

构造函数中的this指向实例化的对象

```js
var that;
function Person(name) {
    that = this;
    this.name = name;
    this.eat = function () {
        console.log('I am eating');
    }
};
var man1 = new Person('张三');
console.log(that === man1);//true
console.log(that);//Person {name: "张三", eat: ƒ}
```

 如果你在调用构造器函数时忘记了在前面加new前缀，那么this将不会绑定到一个新对象上，而是一个全局对象。

```js
var that；
function Person(name) {
    that = this;
    this.name = name; 
    this.eat = function () {
        console.log('I am eating');
    }
};
var man2 = Person('李四');
console.log(man2);//undefined
console.log(that);//Window {parent: Window, opener: null, top: Window, length: 0, frames: Window, …}
```

同一个方法，对象不同，存储的位置也不同，如果有多个对象则需要分配存储多次，内存消耗巨大。

#### 2-5、原型创造对象

- prototype

  每一个构造函数都会有一个显式属性：peototype属性，该属性指向了prototype对象（原型对象），原型对象上存储了这类构造函数对象共有的属性和方法的集合。当修改了原型对象上的属性和方法之后，被实例化的对象调用这些属性和方法都是被修改之后的，因为原型对象中的内容是共享的。

- ``__proto__``

  通过构造函数实例化一个对象，被实例化的对象有一个隐式属性``__proto__``（对象原型），通过构造函数创建的对象之所以能访问属性和方法，就是因为有隐式属性``__proto__``指向了原型对象prototype，``__ptoto__``对象原型是一个非标准属性，是内部的一个指向，在实际开发中不使用。

- constructor

  原型对象有一个constructor属性，它指向构造函数，用来标识对象类型。我们可以用它来做对象的类型判断。实例化对象通过``__proto__``指向对象原型，对象原型又通过constructor属性指向构造函数，所以可以判断实例化对象的类型。（实例化对象的隐式属性``__proto__``也有constructor属性，它可以直接访问构造函数获取私有属性。当构造函数没有共享的属性和方法的时候，对象就直接通过``__proto__.constructor``指向构造函数。）

{% asset_img 13-1.png %}

无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，这个属性指向函数的原型对象。默认情况下，所有原型对象都会自动获得一个constructor属性，该属性包含一个指向prototype属性所在函数的指针。

```js
function Student(){};//定义一个构造函数
Student.prototype.name = '张三';
Student.prototype.eat = function(){
	console.log('I am eating');
}
var s1 = new Student();
var s2 = new Student();
console.log(s1.eat === s2.eat);//true 通过原型对象创建的方法和属性都是共享的
```

原型对象创建对象解决了内存浪费的问题，但是属性和方法都是共有的，不能传参，无法传入不同对象的私有属性，因此采用构造函数+原型对象混合模式创建对象。

#### 2-6、构造函数+原型对象混合模式创建对象

构造函数中存放私有属性和方法，通过参数改变，原型对象中存放共享属性和方法，该方法也是ECMAScript中使用最广泛、认同度最高的一种写法。

```js
function Student(name,age){
	this.name = name;
    this.age = age;
};
Student.prototype.address = China;
Student.prototype.eat = function(){
    console.log('I am eating');
};
var s1 = new Student('张三',18);
var s2 = new Student('李四',20);
console.log(s1.name + s1.address, s2.name + s2.address);//张三China 李四China

```

当原型对象上以对象的形式存放属性和方法的时候，实例化的对象无法指向构造函数，它不知道自己是被哪个构造函数实例化出来的。

```js
//分开存储，可以指向构造函数
function Student(name, age) {
    this.name = name;
    this.age = age;
};
Student.prototype.address = 'China';
Student.prototype.eat = function () {
    console.log('I am eating');
};

var s1 = new Student('张三', 18);
var s2 = new Student('李四', 20);
console.log(s1.__proto__.constructor);
//ƒ Student(name, age) {
//          this.name = name;
//          this.age = age;
//        }

```

```js
//以对象形式存储，无法指向创建自己的构造函数
function Student(name, age) {
    this.name = name;
    this.age = age;
};
Student.prototype = {
    address : 'china',
    eat : function(){
        console.log('I am eating');
    }
}

var s1 = new Student('张三', 18);
console.log(s1.__proto__.constructor);//ƒ Object() { [native code] }

```

这个时候需要我们手动指向。

```js
function Student(name, age) {
    this.name = name;
    this.age = age;
};
Student.prototype = {
    constructor:Student,//自动指向
    address : 'china',
    eat : function(){
        console.log('I am eating');
    }
}

var s1 = new Student('张三', 18);
console.log(s1.__proto__.constructor);
//ƒ Student(name, age) {
//          this.name = name;
//        this.age = age;
//  }

```

#### 2-7、动态混合模式创建对象

有人认为混合模式破坏了封装性，提出了动态混合模式创建(也称动态原型模式)。动态原型模式将所有信息封装在了构造函数中，而通过构造函数中初始化原型（仅第一个对象实例化时初始化原型），这个可以通过判断该方法是否有效而选择是否需要初始化原型。

```js
function Student(name,age){
	this.name = name;
    this.age = age;
    if(typeof (this.eat) !== 'function'){
		Student.prototype.eat = function(){
            console.log('I am eating')
        }
    }
};

var s2 = new Student('李四',20);
s2.eat();//I am eating

```

### 3、原型链

 几乎所有 JavaScript 中的对象都是位于原型链顶端的Object的实例 。当对象一个对象访问属性或方法的时候，会在自身查找有没有，如果没有，会通过``__proto__``到原型对象上查找，如果没有通过prototype原型对象的``__proto__``往上层层查找，最终找到Object的原型对象，如果还没有则返回null。

```js
function Person(name, age) {
    this.name = name;
    this.age = age;
};
Person.prototype = {
    constructor:Person,
    address : 'china',
    eat : function(){
        console.log('I am eating');
    }
}

var student = new Person('张三', 18);

console.log(student.__proto__);
console.log(student.__proto__.__proto__);
console.log(student.__proto__.__proto__.__proto__);

```

{% asset_img 13-2.png %}

{% asset_img 13-3.png %}

数组拓展原型链方法--数组求和

```js
var arr = [1,2,3,4,5];
Array.prototype.sum = function(){//把求和的方法放到数组的原型链上
    var sum = 0;
    for(var i = 0;i<this.length;i++){
        sum += this[i];
    }
    return sum;
}
console.log(arr.sum());//15

```

