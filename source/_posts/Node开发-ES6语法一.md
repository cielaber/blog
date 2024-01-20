---
title: Node开发-ES6语法一
tags: ES6
categories: Node开发
cover: false
img: /image/11.jpg
summary: 第一章：let、const、解构赋值、语法拓展
abbrlink: cc78
date: 2020-09-12 14:28:24
---

# Node开发学习笔记



##  第一章 ES6语法 一

### ES5回顾

#### 严格模式

定义：严格模式是指在严格的条件下运行js代码，在ECMAScript5引入，通过在脚本或函数的头部添加“use strict”来声明。    

好处：

- 消除语法的不合理、不严谨之处，保证代码的运行安全。

- 提高编译器效率，增加运行速度。

- 为未来新版本的js做铺垫。  

限制：

- 不允许使用未声明的变量。

- 不允许对变量或函数使用delete操作符。

- 不允许重命名变量。

- 不允许使用八进制。

- 抛弃with语句。

- 不可对只读对象赋值，不可对不可配置对象使用delete操作符。

- 禁止this关键字指向全局对象。

- 不可再if内部声明函数。

- 不允许使用转义字符。

- 不允许对一个使用getter方法读取的属性进行赋值。

- 不允许删除一个不允许删除的属性。

- 变量不能使用“eval、arguments”字符串。

- 在作用域eval()创建的变量不能被调用。

#### JSON对象

JSON是一种按照JavaScript对象语法的数据格式，虽然是基于JavaScript语法，但它独立于JavaScript，这也是为什么许多程序环境能够读取和生成JSON。JSON是一个序列化的对象或者数组，它的本质是字符串。

JSON对象有两个方法，并且JSON通常用于服务端交换数据，分别为JSON.parse()（将JSON转为js可用数据格式）和JSON.stringify()（将js数据格式转为JSON）。

#### Object拓展

ES5给Object拓展了一些静态方法，常用的有：

- Object.create(prototype[,descriptors])

  作用：以指定对象为原型创建新的对象，为新的对象指定新的属性，并对属性进行描述。新增的属性不会加到原型上。

  - value：指定值
  - writable：标识当前属性是否可修改，默认为false。
  - configurable：标识当前属性值是否可以被删除，默认为false。
  - enumenerable：标识当前属性是否能用for…in枚举，默认为false。

  ```js
  var person1 = {name:'zs',age:18};
  var person2 = Object.create(person1,{
  	sex:{
  		value:'man',
          writable:true,
          configurable:true,
          enumerable:true
      }
  })
  person2.name = 'lisi';
  console.log(person2);//{ sex: 'man', name: 'lisi' }
  ```

- Object.defineProperties(object,descriptors)

  作用：为指定对象定义拓展多个属性。

  - get：来获取当前属性值触发的回调函数。
  - set：修改当前属性值触发的回调函数，参数为修改后的值。

  ```js
  var person = {
  	firstName : 'Marry',
      lastName : 'LuCi'
  }
  
  Object.defineProperties(person,{
  	fullName:{
          get:function(){
  			return this.firstName +' '+this.lastName
          },
          set:function(value){//在外部设置此属性的时候，会调用一次set方法
              var names = value.split(' ')
              this.firstName = names[0]
              this.lastName = names[1]
          }
      }
  })
  console.log(person.fullName);//Marry LuCi
  person.fullName = 'L Jams';
  console.log(person);//{ firstName: 'L', lastName: 'Jams' }
  ```

#### Array数组拓展

数组的拓展主要体现在一些原型方法上，需要合理使用，后续框架中也会经常使用。

- Array.prototype.forEach(function(item,index){})

  遍历数组

  ```js
  var arr = [5,4,3,2,1];
  arr.forEach(function(item,index){
      console.log(item,index);
  });
  //5 0
  //4 1
  //3 2
  //2 3
  //1 4
  ```

- Array.prototype.map(function(item,index){})

  遍历数组，返回一个新数组，返回加工之后的值。

  ```js
  var arr = [1,2,3,4]
  var arr1 = arr.map(function(item,index){
  	return item+10;
  })
  console.log(arr1);//[ 11, 12, 13, 14 ]
  ```

- Array.prototype.filter(function(item,index){})

  遍历过滤出一个新的子数组，返回条件为true的值。

  ```js
  var arr = [-2,-1,0,1,2]
  var arr1 = arr.filter(function(item,index){
  	return item > 0;
  })
  console.log(arr1);//[ 1, 2 ]
  ```

#### Function拓展

- Function.prototype.bind(obj)

- Function.prototype.call(obj)

- Function.prototype.apply(obj)

  改变this指向，将函数内this绑定为obj，并将函数返回。

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

  

### ES6简介

ECMAScript6.0（j简称ES6）是JavaScript语言的下一代标准，在2015年6月正式发布。它的目标是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。



### let命令

新的变量声明方式，与var相似。

- 不能变量提升

- 形成一个**暂时性死区**

  在同一个作用域内，用let声明某一个变量，那么在此声明之前的区域，称之为该变量的暂时性死区。根本原因是，let和const必须，先声明，后使用，不存在变量提升。

  ES6明确规定，如果在区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域，凡是在声明之前就使用这些变量，就会报错。

  ```js
  let temp = 1;
  if(true){
  	temp = 2;
      let temp;
  }
  //该代码在temp = 2;处会报错。
  //因为在{}内，temp = 2下面还有一个let temp声明，temp在{}内形成了暂时性死区，所以上面的temp = 2无效，但如果没有这个声明，temp在{}是可以被访问的，因为{}内与外层还是父子作用域的关系，这个要分清楚。
  ```

- 基于暂时性死区的性质，同一个块级作用域内不能重复声明，且同一个作用域内let和var不能声明同一个变量。

for循环中很适合用let

```js
for(var i = 0;i<5;i++){
    setTimeOut(function (){
        console.log(i);
    })
}
//55555
for(let i = 0;i<5;i++){
	setTimeOut(function(){
		console.log(i);
    })
}
//01234
```

var 声明的变量是一个全局变量，在全局范围内有效，全局只存在一个变量，每次循环都会覆盖。由于for循环瞬间完成的特性，定时器是在循环完成之后执行所以i都是5。

let声明的i，只在当前轮循环有效，每次都是新的变量，所以定时器里的i每次都是不同的变量。（JavaScript引擎内部会记录每次变量的值。）



### 块级作用域

块级作用域中的块，就是语句块，表现形式为{}，大括号包裹的内容为语句块。

- 只对let声明的变量才会有块级作用域。

- if-else会形成块级作用域。

- for循环小括号()内跟循环体{}是一种父子作用域的关系。

- 如果使用let与const，每次迭代将会创建一个新的存储空间，这可以保证作用域在迭代的内部。

- 块级作用域允许任意嵌套，外层作用域无法读取内层作用域的变量，内外层作用域可以声明同名变量。

  

### const命令

- 常量声明关键字：const
- 不能变量提升
- 不能重复声明/赋值（如果是对象，可以修改对象中属性的值。）
- 具有暂时性死区特点
- 块级作用域
- 常量特有的特点
  - 只能赋值一次，不能重复赋值。
  - 声明时候立马赋值。

补充：const如何做到在变量声明之后不允许改变的？其实const保证的不是变量的值不变，而是保证了变量指向的内存地址所保存的数据不允许改动。由于简单数据类型和复杂数据类型的保存值的方式不一样，简单数据类型的值保存在变量指向的内存地址，因此const声明简单数据类型变量等同于常量。而复杂数据类型，变量指向的内存地址其实是保存了一个指向实际数据的指针，所以const只能保存指针是固定的，至于指针指向的数据结构变不变就无法控制了，所以使用const声明复杂数据类型时要慎重。



### 解构赋值

解构赋值是对赋值运算符的拓展，针对数组或对象进行模式匹配。用于多个变量从同一个复杂数据源获取值。

#### 数组解构赋值

参考数组的模式一一赋值。在数组的解构中，解构目标若为可遍历对象，皆可进行解构赋值。

```js
let [a,b] = [1,2,3];//a = 1,b = 2
//可省略
let [a,,b] = [1,2,3];//a = 1,b = 3
let [,a,b] = [1,2,3];//a = 2,b = 3
//不完全解构
let [a,b] = [1];//a = 1,b = undefined
//可嵌套
let [a,[[b],c]] = [1,[[2],3]];//a = 1,b = 2,c = 3
```

匹配失败时，计算就为默认值，默认值可以是具体值或表达式。匹配成功就是匹配成功的值，匹配失败才会是默认值。

```js
let [a = 1,b = 2] = [0]
console.log(a,b)//a = 0,b = 2
```

#### 对象解构赋值

数组的按顺序排列，变量的值由位置决定，而对象的属性没有次序，必须与属性名相同才能取到值。

```js
let {a:a1, b:b1} = { a : 1,b : 2}//a1 = 1,b1 = 2
//顺序无关
let {b:b1 ,a:a1} = {a : 1,b : 2}//a1 = 1,b1 = 2
//属性值需相同
let {c:a1,d:b1 } = {a :1,b : 2 }//undefined,undefined
//可以有默认值
let {a:a1,b:b1,c:c1='beijing'} = {a:1,b:2}//a1 = 1,b1 = 2,c1 = 'beijing'
```

当变量和属性名一致时，可以简写（省略属性名和：），一般也是用简写。对象解构赋值的内部机制是先找到同名属性，然后再赋值给对应的变量，真正被赋值的是变量，而不是属性名。

```js
let {a, b} = { a : 1,b : 2}//a = 1,b = 2
```

所以有时候需要分清到底哪些是模式（属性名）哪些是变量，例如

```js
let obj = {
    s1:[
        'zs',
        {age:18}
    ]
}

let {s1:[name,{age}]} = obj;
console.log(name,age)//name = 'zs',age = 18
//这里的s1是模式而不是变量，不会被赋值
```

```js
let obj = {
    s1:[
        'zs',
        {age:18}
    ]
}

let {s1,s1:[name,{age}]} = obj;
console.log(s1,name,age)//s1 = [ 'zs', { age: 18 } ],name = 'zs',age = 18
//这里的s1就是与属性名同名的变量
```

```js
let person = {
    man:{
        student:{
            name:'zs',
            age:18
        }
    }
}

let {man,man:{student},man:{student:{name,age}}} = person;
console.log(man,student,name,age);
//man = { student: { name: 'zs', age: 18 } }
//student = { name: 'zs', age: 18 } 
//name = zs,age = 18
```

解构赋值可以解析到子对象从父对象继承来的属性，如下：

```js
let obj1 = {name : 'zs'};
let obj2 = {age : 18};
Object.setPrototypeOf(obj1,obj2);

let {age} = obj1;
console.log(age);//18
//Object.setPrototypeOf()方法设置一个指定的对象的原型（一般不建议使用）
//obj1的原型对象是obj2，age属性不是obj1自身的属性，而是继承自obj2的属性，解构赋值可以取到这个值。
```

注意：如果要将一个已经声明的变量用于解构赋值，需要非常小心。

```js
let name;
{ name } = { name : 'zs' }
```

以上写法不合理，因为JavaScript引擎会将{ name }解释成代码块，从而发生语法错误，可以写成如下：

```js
let name;
({ name } = {name : 'zs'});
//圆括号虽然在此处有助于解析，但是也有许多情况滥用圆括号导致歧义，ES6的规则是，只要有可能导致歧义，就不得使用圆括号。
```

解构赋值允许等号左边的模式不放任何变量名，虽然没有意义但是语法合法。

```js
({} = ['abc'])
```

由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。

```js
let arr = [1,2,3];
let { 0 : a,[arr.length-1] : b} = arr;
console(a,b)//1,3
```

对数组进行对象解构，键名分别是0，1，2，方括号的写法属于“属性名表达式”，深究另行查找。

#### 字符串的解构赋值

字符串可以进行解构赋值，这时字符串被转换成一个类似数组的对象，类似数组的对象都有一个length属性，也可以进行解构赋值。

```js
let [a,b,c,d,e] = 'hello';
let {length : len} = 'hello'
console.log(a,b,c,d,e,len);
//a = 'h'
//b = 'e'
//c = 'l'
//d = 'l'
//e = 'o'
//len = 5
```

#### 数值和布尔值的解构赋值

数值和布尔值进行解构赋值时，会先转成对象，数值和布尔值包装成对象都有toString属性。

```js
let {toString : a} = 123;
let {toString : b} = true;
console.log(a === Number.prototype.toString)//true
console.log(b === Boolean.prototype.toString)//true
```



### 字符串的扩展

#### 模板字符串

模板字符串是增强版的字符串，用反引号(``)标识，除了作为普通字符串，还可以用来定义多行字符串，通过${}在字符串中加入变量和表达式。中间可以使用转义字符，模板字符串中的换行和空格都是会被保留的。

```js
let a = 123;
let str = `he${a}llo\nworld`;
console.log(str);
//he123llo
//world
```

#### 标签模板

标签模板是函数调用的一种特殊形式，调用的参数是模板字符串。

```js
console.log`hi`;//[ 'hi' ]
console.log(['hi']);//[ 'hi' ]
```

如果模板字符串里面有变量，就不是简单的调用了，而是会将模板字符串先处理成多个参数再调用函数。

```js
var str = 'b';
function fun(){
	console.log(arguments);
};
fun`a${str}c`;//[Arguments] { '0': [ 'a', 'c' ], '1': 'b' }
//等同于
fun(['a','c'],'b');//[Arguments] { '0': [ 'a', 'c' ], '1': 'b' }
```



### 函数拓展

#### 参数默认值

- 在es6之前，不能直接为函数参数指定默认值，es6中允许为函数的参数传递默认值，在未传递参数或者参数未undefined时，才会使用默认参数，需要注意，null值是被认为有效的值传递。

  ```js
  //es6之前
  function fun(x){
      x = x || 'hello';
  	console.log(x);
  }
  
  //es6
  function fun(x = 'hello'){
  	console.log(x);
  }
  ```

- 函数使用默认参数时，该函数不允许有与默认参数同名的参数。

- 函数参数默认值存在暂时性死区，在函数参数默认值表达式中，还未初始化的参数值无法作为其他参数的默认值。

函数默认值与解构赋值结合使用

```js
function fun1({x,y = 5}){
	console.log(x,y);
}
fun1({})//undefined 5
fun1()//TypeError   没有实参会报错

function fun2({x,y = 5} = {}){
	console.log(x,y);
}
fun2()//undefined 5
```

函数参数设置默认值之后，具有以下特点：

- **函数的length属性**

  函数的length属性返回该函数预期传入的参数个数，不包括设置默认值的参数，某个参数指定默认值后，预期传入的参数就不包括这个参数了。

- **默认值参数形成单独作用域。**

  函数参数设置了默认值之后，函数进行声明初始化时，参数会形成一个单独的作用域，等到初始化结束，这个作用域就会消失。这种语法在不设置参数默认值时，是不会出现的。

  ```js
  let x = 1;
  function fun(y = x){
  	let x = 2;
      console.log(y);
  }
  f();//1
  //参数y的默认值等于变量x，函数调用时，参数形成一个单独的作用域，在这个作用域里面，变量x本身没有定义，所以指向外层的全局变量x，函数调用时，函数体内部的局部变量x不影响默认值变量。
  ```

参数默认值应用：

利用参数默认值，可以指定某一个参数不得省略，如果省略就抛出一个错误。

```js
function throwError(){
	throw new Error('Missing parameter');
}

function fun(mustBeProvided = throwError()){
	...;
}

fun()//Error:Minssing parameter
//当参数没有赋值时，默认参数是一个函数调用，该函数被执行抛出错误。
```

#### 不定参数/rest参数

不定参数用来表示不确定参数，由…加上一个具名参数标识符组成，**只能放在参数组的最后，且只有一个**。

rest参数在一定程度上代替了arguments，他们的区别是，**argumens对象不是数组，而是一个类似数组的对象**，所以它要使用数组的方法，必须使用Array.prototype.silce.call将其转为数组。rest参数是一个真正的数组，数组特有的方法他都可以使用。

```js
function add(...item){
    var sum = 0;
	for(var i=0;i<item.length;i++){
		sum +=item[i];
    }
    return sum;
}
console.log(add(1,2,3,4,5))//15
```

**箭头函数中没有arguments。**

**函数的length属性不包含rest参数。**

#### 箭头函数

基本语法：参数 => 函数体

```js
var f = v => v;
//等价于
var f = function(v){
	return v;
}
```

- 当箭头函数没有参数或者有多个参数的时候，要用（）括起来。

- 当箭头函数有多行语句，函数体要用{}包裹起来表示代码块，当只有一行语句，并且需要返回值时，可以省略{}，结果会自动返回。

  ```js
  var f = (a,b) => a+b;
  f(1,2);//3
  ```

- 当箭头函数要返回对象的时候，为了区分代码块，要用（）将对象包裹起来，否则会报错。

- 箭头函数中没有this、super、arguments和new.target绑定。

- 箭头函数不改变this指向，箭头函数中的this与父级this指向一样。



### 数组拓展

#### 拓展运算符

在数组中…是拓展运算符，它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。

```js
function add(x,y,z){
    return x+y+z;
};

const number = [1,2,3];
console.log(add(...number));//6
```

拓展运算符与正常的函数参数结合使用，位置不固定。拓展运算符后面还可以放置表达式，如果拓展运算符后面是一个空数组，则不产生任何效果。只有函数调用时，拓展运算符才能放在圆括号中，否则会报错。

#### 拓展运算符的应用

- ##### 复制数组

  ```js
  const arr1 = [1,2,3,4];
  //方法一
  const arr2 = [...arr1];
  console.log(arr2);//[1,2,3,4]
  //方法二
  const [...arr2] = a1;
  console.log(arr2);//[1,2,3,4]
  ```

  …拓展运算符复制一维数组属于深拷贝，但是它只实现一层拷贝，不能实现多层深拷贝。如果是多维数组，里面的嵌套数组依然是指针指向。下面的合并数组也是如此。

- ##### 合并数组

  拓展运算符提供了数组合并的新写法。

  ```js
  const arr1 = [1,2,3];
  const arr2 = [4,5,6];
  
  arr3.concat(arr1,arr2);
  console.log(arr3);//[1,2,3,4,5,6]
  
  arr4 = [...arr1,...arr2];
  console.log(arr4);//[1,2,3,4,5,6]
  ```

- ##### 与解构赋值结合

  拓展运算符与解构赋值结合起来，用于生成数组。

  ```js
  const [first,...rest] = [1,2,3,4,5];
  console.log(first,rest);//1  [2,3,4,5]
  ```

  如果将拓展运算符用于数组赋值，则只能放在参数的最后一位，否则会报错。

- ##### 字符串

  拓展运算符还可以将字符串转为真正的数组。

  ```js
  let str = [...'hello'];
  console.log(str);//[ 'h', 'e', 'l', 'l', 'o' ]
  ```

  上面的写法有一个重要的好处，能够正确识别四个字节的Unicode字符。

  ```js
  console.log('a\uD83D\uDE80b'.length)//4
  console.log([...'a\uD83D\uDE80b'].length)//3
  ```

  凡是涉及到操作四个字节的Unicode字符的函数，都有这个问题，因此最好都用拓展运算符改写。

- ##### 实现了Iterator接口的数据类型转为真正数组

  所有实现了Iterator接口的数据类型都可以采用拓展运算符将其转为真正的数组，比如map数据。

  ```js
  let map = new Map([
      [1,'one'],
      [2,'two'],
      [3,'three'],
  ])
  
  let arr = [...map.keys()];
  console.log(arr);//[1,2,3]
  ```




ES6语法学习资料大多来自如下，致谢：

- 阮一峰《ECMAScript6入门教程》https://es6.ruanyifeng.com/
- 菜鸟教程https://www.runoob.com/w3cnote/es6-tutorial.html