---
title: Node开发-ES6语法三
tags: ES6
categories: Node开发
cover: false
img: /image/13.jpg
summary: 第三章：Class
abbrlink: cab8
date: 2020-09-12 14:38:26
---

# Node开发学习笔记

##  第三章 ES6语法三

### Class

JavaScript中生成实例对象的传统方法是通过构造函数和原型对象混合方法，但是这种写法跟传统的面向对象语言（如c++,java）差异很大。ES6提供了更接近传统面向对象编程的语法，引入了Class类的概念。但是它不过是一个语法糖，它的绝大部分功能ES5都可以做到，class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

注意点：类不可重复声明、类定义不会被提升、类中方法不需要function关键字、类中方法见不能加分号。

```js
//传统写法
function Person(name,age){
	this.name = name;
    this.age = age;
}

Person.prototype.eat(){
	console.log('I am eating')
}

var p = new Person('zs',18)
```

```js
//class写法
class Person{
	constructor(name,age){
		this.name = name;
        this.age = name;
    }
    eat(){
		console.log('I am eating');
    }
}

var p = new Person('zs',18)
console.log(typeof Person);//function
console.log(Person === Person.prototype.constructor);//true
```

表明：**类的数据类型就是函数，类本身就指向构造函数。**

实际上，类的所有方法都定义在类的prototype属性上面。

```js
class Person{
	constructor(){};
    eat(){};
}
//等同于
Person.prototype = {
    constructor(){},
    eat(){}
}
```

与ES5原型对象创建不同的是，class内部定义的方法是不可枚举的，ES5中原型对象的方法可枚举。

```js
class Person{
    eat(){};
};
console.log(Object.keys(Person.prototype));//[]

var Person = function(){};
Person.prototype.eat = function(){};
console.log(Person.prototype);//Person { eat: [Function] }
```

#### 属性

- prototype

  ES6中依旧存在prototype属性，虽然可以直接在类中定义方法，但是本质上还是定义在prototype上的。

  ```js
  class Person{};
  Person.prototype.eat= function(){
  		console.log('I am eating!');
  }
  var p = new Person();
  p.eat();//I am eating!
  ```

- 静态属性

  class本身的属性，用state关键字声明。**静态属性、静态方法与类的普通属性、普通方法是两套机制，即使是同名属性和方法也不冲突，普通this指向普通属性，静态属性的this指向对应的静态属性。**

- name

  返回类名

  ```js
  class Person {};
  console.log(Person.name);//Person
  ```

#### constructor

该方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类中必须有constructor方法，如果没有，一个空的constructor方法会被默认添加。

#### 继承

class通过extends和super关键字实现类的继承。

需要注意的是，在子类的构造函数中，只有调用super关键字后才能使用this，因为子类实例的构建基于父类实例，而只有super方法才能调用父类实例，**所以this关键字必须在super之后**。

```js
class Person{
    constructor(name,age,address){
        this.name = name,
        this.age = age,
        this.address = address
    }
    run(){
        console.log('I can run')
    }
    sing(){
        console.log('I can sing')
    }
}

class Student extends Person{
    constructor(name, age,address,sId){
        super (name, age, address);
        this.sId = sId;
    }
    run(){//方法可以重写
        console.log('I can not run');
    }
    eat(){
        console.log('I am eating');
    }
}

var s= new Student('zs',18,'jx',1001);
s.sing();//I can sing
s.run();//I can not run
s.eat();//I am eating
console.log(s.name,s.sId);//zs  1001
```



ES6语法学习资料大多来自如下，致谢：

- 阮一峰《ECMAScript6入门教程》https://es6.ruanyifeng.com/
- 菜鸟教程https://www.runoob.com/w3cnote/es6-tutorial.html



