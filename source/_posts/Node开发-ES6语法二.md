---
title: Node开发-ES6语法二
tags: ES6
categories: Node开发
cover: false
img: /image/12.jpg
summary: 第二章：Set、Map、Symbol、Iterator、异步
abbrlink: a979
date: 2020-09-12 14:35:46
---

# Node开发学习笔记



##  第二章 ES6语法二

### Set

Set对象允许存储任何类型的唯一值，无论是原始值或者是对象引用。

#### Set和Map中的特殊值

Set对象存储的值总是唯一的，所以需要判断两个值是否恒等。有几个特殊值需要特殊对待：

- +0与-0在存储判断唯一性的时候是恒等的，所以不会同时存在。
- undefined与undefined是恒等的，所以不会同时存在。
- NaN与NaN是不恒等的，但是在Set中只能存在一个，不会同时存在。

#### Set类型转换

- ##### Array转Set

  ```js
  var set = new Set([1,'a',{},[2,3]]);
  console.log(set);//Set { 1, 'a', {}, [ 2, 3 ] }
  ```

- ##### Set转Array

  ```js
  var set = new Set([1,'a',{},[2,3]]);
  var arr = [...set];
  console.log(arr);//[ 1, 'a', {}, [ 2, 3 ] ]
  ```

- ##### String转Set

  ```js
  var set = new Set('hello');
  console.log(set);//Set { 'h', 'e', 'l', 'o' }
  ```

#### Set实例的属性

- Set.prototype.constructor：构造函数。默认是Set函数。
- Set.prototype.size：返回Set实例的总成员数。

```js
var set = new Set([1,'a',2,'b']);
console.log(set.constructor);//[Function: Set]
console.log(set.size);//4
```

#### Set实例的方法

Set实例的方法分为两大类：操作方法（操作数据）和遍历方法（遍历成员）

##### 操作方法：

- Set.prototype.add(value)：添加某个值，返回原Set本身。
- Set.prototype.delete(value)：删除某个元素，返回一个布尔值，表示是否删除成功。
- Set.prototype.has(value)：返回一个布尔值，表示该值是否为Set的成员。
- Set.prototype.clear()：清除所有成员，没有返回值。

```js
var set = new Set([1,2,'a','b']);
set.add('c');
console.log(set);//Set { 1, 2, 'a', 'b', 'c' }
set.delete(2);
console.log(set);//Set { 1, 'a', 'b', 'c' }
console.log(set.has('a'));//true
set.clear();
console.log(set);//Set {}
```

##### 遍历方法：

- Set.prototype.keys()：返回键名的遍历器。
- Set.prototype.values()：返回值的遍历器。
- Set.prototype.entries()：返回键值对的遍历器，同时包括键名和键值。
- Set.prototype.forEach()：使用回调函数遍历每个成员。

由于Set结构没有键名只有键值（或者说键名键值是同一个值），所以keys方法和values方法的行为完全一致。

```js
let set = new Set(['a','b','c']);
console.log(set.keys());//[Set Iterator] { 'a', 'b', 'c' }
console.log(set.values());//[Set Iterator] { 'a', 'b', 'c' }
console.log(set.entries());//[Set Entries] { [ 'a', 'a' ], [ 'b', 'b' ], [ 'c', 'c' ] }

for(let item of set.keys()){
	console.log(item);
}
//a b c
for(let item of set.values()){
	console.log(item);
}
//a b c
for(let item of set.entries()){
	console.log(item);
}
//[ 'a', 'a' ]
//[ 'b', 'b' ]
//[ 'c', 'c' ]
```

Set结构的实例默认可以遍历，它的默认遍历器生成函数就是它的values()方法。这意味着可以省略values方法，**直接用for…of循环遍历Set**。

```js
let set = new Set(['a','b','c']);
for(let item of set){
	console.log(item);
}
//a b c
```

Set与数组一样，也可以使用forEach方法进行遍历，但与数组不同的是，Set没有索引值，所以它的第一第二两个参数的值相同。

```js
let set = new Set([4,3,2,1]);
set.forEach((value,key) => console.log(value,key));
//4 4
//3 3
//2 2
//1 1
```

#### Set对象作用

- ##### 数组去重

  ```js
  var set = new Set([1,2,3,4,4,5]);
  var arr = [...set];
  console.log(arr)//[1,2,3,4,5]
  
  ```

- ##### 并集

  ```js
  var a = new Set([1,2,3]);
  var b = new Set([4,5,6]);
  var nuion = [...a,...b];
  console.log(union);
  
  ```

- ##### 交集

  ```js
  var a = new Set([1,2,3,4]);
  var b = new Set([3,4,5,6]);
  
  var intersect = new Set([...a].filter(x => b.has(x)));//Set { 3, 4 }
  
  ```

- ##### 差集

  ```js
  var a = new Set([1,2,3]);
  var b = new Set([4,3,2]);
  
  var difference = new Set([...a].filter(x => !b.has(x)));//Set { 1 }
  
  ```

  

### Map

JavaScript对象本质上是键值对的集合（Hash结构），但是传统上只能用作字符串当作键，限制太大。

为了解决这个问题，ES6提供了Map数据结构，类似于对象，也是键值对的集合，但是“”键

的范围不限于字符串，各种类型的值都可以当作键，是一种更完善的Hash结构实现。

Map中的键也有特殊值，与Set一样，-0与+0、undefined、NaN。

#### Map和Object的区别

- 一个Object的键只能时字符串或者Symbol，但是一个Map的键可以是任意值。
- Map中的键值是有序的（FIFO原则），而添加到对象中的键则不是。
- Map的键值对个数可以从size属性获取，而Object的键值对个数只能手动计算。
- Object都有自己的原型，原型链上的键名有可能和对象上的设置的键名产生冲突。

#### Map属性和方法

- size属性

  该属性返回Map结构的成员总数。

  ```js
  const map = new Map();
  map.set('name' , 'zs');
  map.set('age' , 18);
  console.log(map.size)//2
  
  ```

- Map.prototype.set(key,value)

  set()方法设置/修改键值对成员并返回整个Map结构。

  ```js
  const map = new Map();
  map.set('name' , 'zs');
  map.set('age' , 18);
  console.log(map);//Map { 'name' => 'zs', 'age' => 18 }
  
  ```

  set()返回的是当前Map对象，因此可以采用链式写法。

- Map.prototype.get(key)

  get()方法读取key对应的键值，如果找不到key就返回undefined。

  ```js
  const map = new Map();
  map.set('name','zs');
  console.log(map.get('name'));//zs
  
  ```

- Map.prototype.has(key)

  has()方法返回一个布尔值，表示某个键是否在当前Map对象中。

  ```js
  const map = new Map();
  map.set('name','zs');
  console.log(map.has('name'));//true
  
  ```

- Map.prototype.delete(key)

  delete()方法删除某个键，返回true，如果删除失败，返回false。

  ```js
  const map = new Map();
  map.set('name','zs');
  console.log(map);//Map { 'name' => 'zs' }
  map.delete('name');
  console.log(map);//Map {}
  
  ```

- Map.prototype.clear()

  clear()方法清除所有成员，没有返回值。

  ```js
  const map = new Map();
  map.set('name','zs');
  console.log(map.size);//1
  map.clear();
  console.log(map.size);//0
  
  ```

#### Map遍历方法

- Map.prototype.key()：返回键名的遍历器
- Map.prototype.values()：返回键值的遍历器
- Map.prototype.entries()：返回所有成员的遍历器
- Map.prototype.forEach()：遍历Map的所有成员

```js
const map = new Map([
    ['name','zs'],
    ['age',18]
]);

for(let key of map.keys()){
	console.log(key);
}
//name age

for(let value of map.values()){
	console.log(value);
}
//zs 18

for(let item of map.entries()){
	console.log(item);
}
//['name','zs']
//['age',18]

```

Map的forEach()方法与数组和Set的forEach()方法类似，实现遍历如下：

```js
const map = new Map([
    ['name','zs'],
    ['age',18]
]);
map.forEach(function(value,key,map){
	console.log(value,key);
})
//zs name
//18 age

```

#### Map与其他数据结构的相互转换

##### Map转为数组

Map转为数组最方便的方法就是使用拓展运算符（…）

```js
const map = new Map().set('name','zs').set('age',18);
var arr = [...map];
console.log(arr);//[ [ 'name', 'zs' ], [ 'age', 18 ] ]

```

##### 数组转为Map

将数组以参数传入Map构造函数，就可以将数组转为Map。注意：这里的数组也必须是类似键值对形式，否则键值为undefined。

```js
const map = new Map([['name','zs'],['age',18],['abc']])
console.log(map);//Map { 'name' => 'zs', 'age' => 18, 'abc' => undefined }

```

##### Map转为对象

如果所有Map的键都是字符串，它可以无损地转为对象。

```js
const map = new Map().set('name','zs').set('age',18);

function mapToObj(map){
	let obj = new Object();
    for(let [key,value] of map){
		obj[key] = value;
    }
    return obj;
}

console.log(mapToObj(map));//{ name: 'zs', age: 18 }

```

如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。

##### 对象转为Map

对象转为Map可以通过Object.entries()。

```js
let obj = {'a':1,'b':2};
let map = new Map(Object.entries(obj));
console.log(map);//Map { 'a' => 1, 'b' => 2 }

```

也可以通过函数转换：

```js
const obj = {'name':'zs','age':18};
function objToMap(obj){
	let map = new Map();
    for(let key of Object.keys(obj)){
		map.set(key,obj[key]);
    }
    return map;
}

console.log(objToMap(obj));//Map { 'name' => 'zs', 'age' => 18 }

```

##### Map转为JSON

Map转为JSON要区分两种情况，一种情况是Map的键名都是字符串，这时可以选择转为对象JSON。

```js
function mapToObj(map){
	let obj = new Object();
    for(let [key,value] of map){
		obj[key] = value;
    }
    return obj;
}

function mapToStrJson(map){
	return JSON.stringify(mapToObj(map));
}

let map = new Map().set('name','zs').set({'age':18},['abc']);
console.log(mapToStrJson(map));//[["name","zs"],[{"age":18},["abc"]]]

```

另一种情况是，Map的键名有非字符串，这时可以选择转为数组JSON。

```js
function mapToArrayJson(map){
	return JSON.stringify([...map]);
}
let map = new Map().set('name','zs').set('age',18);
console.log(mapToArrayJson(map));//[["name","zs"],["age",18]]

```

##### JSON转为Map

这也分两种情况，键名都是字符串。

```js
function objToMap(obj){
	let map = new Map();
    for(let key of Object.keys(obj)){
		map.set(key,obj[key]);
    }
    return map;
}

function jsonToStrMap(json){
	return objToMap(JSON.parse(json));
}
console.log(jsonToStrMap('{"name":"zs","age":18}'));
//Map { 'name' => 'zs', 'age' => 18 }

```

另一种情况，整个JSON就是一个数组，且每个数组成员本身又是一个有两个成员的数组，这时，它可以一一对应地转为Map，这往往是Map转为数组JSON的逆操作。

```js
function jsonToMap(json){
	return new Map(JSON.parse(json));
}
console.log(jsonToMap('[["abc",123],[{"name":"zs"},["beijing"]]]'));
//Map { 'abc' => 123, { name: 'zs' } => [ 'beijing' ] }

```



### Symbol

ES6引入了一种新的原始数据类型Symbol，表示独一无二的值，最大的用法是用来定义属性的唯一属性名。

#### 基本用法

Symbol函数栈不能使用new命令，因为Symbol是原始数据类型，不是对象，可以接受一个字符串作为参数，为新创建的Symbol提供描述，用来显示在控制台或者作为字符串的时候使用，便于区分。

Symbol函数的参数只是表示对当前Symbol值的描述，因此相同的参数Symbol函数返回值是不相等的。

```js
let s = Symbol('name');
console.log(s);//Symbol(name)

let s1= Symbol('name');
//相同的参数，返回值不相等。
console.log(s1 === s);//false

```

**作为属性名使用，可以保证属性不重名。**

```js
let s = Symbol('name')
let obj = {
     [s] : 'zs'
 };

console.log(obj);//{ [Symbol(name)]: 'zs' }

```

Symbol作为对象属性名时不能用.运算符，要用方括号，因为**.运算符后面是字符串**，所以取到的是字符串s属性，而不是Symbol值s属性。

因为点运算符后面总是字符串，所以当以点运算符的方式将Symbol值作为对象的属性名时，以[]使用该属性需要加上引号，如下：

```js
let s = Symbol();
let obj = {};
obj.s = 'abc';
console.log(obj);//{ s: abc}
console.log(obj[s]);//undefined
console.log(obj['s']);//abc

```

注意：Symbol值作为属性名时，该属性是公有属性不是私有属性，可以在类的外部访问，但是不会出现在for…in，for…of的循环中，也不会被Object.keys()、Object.getOwnPropertyNames()返回。如果读取到一个对象的Symbol属性，可以通过Object.getOwnPropertySymbols()和Reflect.ownKeys()取到。

> - Object.keys()方法返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致。
> - Object.getOwnPropertyNames()方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的数组。
> - Object.getOwnPropertySymbols()方法返回一个给定对象自身的所有Symbol属性的数组。
> - Reflect.ownKeys()静态方法返回一个由目标对象自身的属性键组成的数组，返回值等同于：Object.getOwnPropertyNames(target).concat(Object.getOwnPropertySymbols(target))

**Symbol类型还可以用于定义一组常量，保证这组常量的值都是不相等的。**因此，可以保证switch语句按照预想的方式执行。

#### Symbol.for()

用Symbol.for()创建一个Symbol数据时，首先会在全局搜索被登记的Symbol中是否有该字符串参数作为名称的Symbol值，如果有即返回该Symbol值，没有则新建并返回一个以该字符串为名称的Symbol值，并登记在全局环境中供搜索。而Symbol()方法生成的值不会登记在全局环境中。

```js
let s1 = Symbol('sym');
let s2 = Symbol('sym');
let s3 = Symbol.for('sym');
let s4 = Symbol.for('sym');

console.log(s1 === s2);//false
console.log(s1 === s3);//false
console.log(s3 === s4);//true

```

#### Symbol.keyFor()

Symbol.keyFor()返回一个已登记的Symbol类型值得key，用来检测该字符串参数作为名称得Symbol值是否已经被等级。

```js
let s1 = Symbol('sym');
let s2 = Symbol.for('sym');
console.log(Symbol.keyFor(s1));//undefined
console.log(Symbol.keyFor(s2));//sym

```



### Promise

Promise是一种异步解决方案，解决了回调地狱的问题，比传统的回调函数有更好的语义化。一开始由社区提出，es6中将其写进标准语法，原生提供了Promise对象。

#### Promise对象有以下两个特点：

- 对象的状态不受外界影响。

  Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfiled（已成功）、rejected（已失败）。只有异步操作的结果可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

- 状态变化后无法改变，任何时候都可以得到这个结果。Promise对象状态的改变只有两种：从pending变为fulfilled或rejected。状态变为fulfilled或rejected之后就不会改变，会一直保持这个结果，称为resolved。与事件不同，事件错过了之后再去监听得不到结果，但是Promise而言，改变发生之后就一直存在，对Promise对象添加回调函数也能立刻得到这个结果。

有了Promise对象就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数，此外，Promise对象提供统一接口，使得控制异步操作更加容易。

#### Promise的缺点：

- 无法取消Promise，一旦新建它就会立即执行，无法中途取消。
- 如果不设置回调函数，Promise内部抛出错误，不会反应到外部（不会影响整个程序进行），可以理解为Promise会吃掉（其运行期间的）异常（不能吃编译期异常）。
- 当处于pending状态时，无法得知目前进展到哪一阶段。

#### Promise状态变更的控制

- new Promise的时候，状态为pending。
- 当执行resolve（），状态由pending变为fulfilled。
- 当执行reject（），状态由pending变为rejected。

#### Promise基本用法

```js
var p = new Promise(function(resolve,reject){
    if(/*异步操作成功*/){
       resolve(data);
	}else{
      reject(error);
	}
});

```

Promise构造函数接收一个函数作为参数，该函数的两个参数分别是resolve、reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。

resolve函数的作用是将Promise对象状态从pending变为resolved，在异步操作成功时调用，并将异步操作的结果作为参数传出去，传给then的第一个参数函数的的形参。

reject函数的作用是将Promise对象的状态从pending变为rejected，在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去，传给then第二个参数函数的形参或者catch的参数函数的形参。

#### then()方法

Promise.prototype.then()，then方法是定义在原型对象Promise.prototype上的，它的作用是为Promise实例添加状态改变时的回调函数。then()方法返回的是一个新的Promise实例（不是原来那个Promise实例）。因此可以采用链式写法，即then()方法后面再调用另一个then方法。

Promise实例生成之后，可以用then方法指定resolved状态和rejected状态的回调函数。第一个回调函数时Promise对象的状态变为resolved时调用，第二个回调函数时Promise对象的状态变为rejected时调用。两个函数只有一个会被调用，其中，第二个函数是可选的，不一定要提供。

```js
promise.then(function(value){},function(error){});

```

#### catch()方法

Promise.prototype.catch()方法是.then(null,reject)或.then(undefined,rejected)的别名，用于指定发生错误时的回调函数。reject()方法的作用，等同于try{throw new Error()}抛出异常。

Promise对象错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。即错误总是会被下一个catch语句捕获。

一般来说，不要在then()方法里面定义reject状态的回调函数（即then的第二个参数），总是使用catch方法捕获错误。

与传统的try/catch代码块不同的是，如果没有使用catch（）方法指定错误处理的回调函数，Promise对象抛出的错误不会传递到外层代码，不会影响到Promise外部的代码，简单来说是“Promise会吃掉错误”。

一般总是建议，Promise对象后面要跟catch()方法，这样可以处理Promise内部发生的错误，catch()方法返回的还是一个Promise对象，因此后面还可以接着调用then()方法（或catch方法）。如果没有报错，则会跳过catch()方法。

#### finally()方法

Promise.prototype.finally()方法用于指定Promise对象最后状态如何，都会执行的操作，该方法是ES2018引入的标准。finally()方法其实是then()方法的特例，即不管resolved和rejected都调用finally中的方法。

#### 多个异步的协调问题

除了传统的回调地狱之外，Promise提供了以下方法。

##### Promise.all()

用于将多个Promise实例包装成一个新的Promise实例。

```js
const p = Promise.all([p1,p2,p3]);

```

该方法接收一个数组作为参数，数组的值都是Promise实例，如果不是，就会先调用Promise.resolve方法，将参数转为Promise实例，再进一步处理，另外，Promise.all()方法的参数可以不是数组，但是必须有Iterator接口，且返回的每个成员都是Promise实例，

p的状态由p1、p2、p3决定，分成两种情况。

- 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
- 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值会传递给p的回调函数。

##### Promise.race()

promise.race()方法同样是将多个Promise实例，包装成一个新的Promise实例。

```js
const p = Promise.race([p1,p2,p3]);

```

与Promise.all()一样，接收一个数组作为参数，如果数组值不是Promise实例，就会调用Promise.resolve()方法，将参数转为Promise实例。

只要p1、p2、p3之中有一个实例的状态改变，p的状态就跟着改变，那个率先改变状态的Promise实例的返回值，就传递给p的回调函数。

除了以上两种方法之外还有Promsie.allSettled和Promise.any方法，但这两种方法兼容性不优。

#### Promise总结

Promise用来解决异步回调问题，由于js是单线程的，很多异步方法都是依靠回调方法实现的，这种做法在逻辑上比较复杂的回调嵌套中会相当复杂，也叫回调地狱。Promise用来将这种繁琐的做法简化，让程序更具备可读性，可维护性。

Promise内部由三种状态：pending、fulfilled、rejected，分别表示程序正在执行但未得到结果即异步操作没有执行完毕、程序执行完毕且成功、程序执行完毕但失败。这里的成功和失败都是逻辑意义上的，并非是要报错。

其实，Promise和回调函数一样，都是要解决数据的传递和消息发送问题，Promise中的then一般对应成功后的数据处理，catch一般对应失败后的数据处理。



### Iterator

Iterator是一种统一的接口机制，来处理所有不同的表示“集合”的数据结构，如Array、Object、Map、Set等。任何数据结构只要部署了Iterator接口，就可以完成遍历操作。

#### Iterator的作用：

- 为数据结构提供一个统一的、简便的访问接口。
- 使得数据结构的成员能按某种次序排列。
- ES6创建了一种新的遍历命令for…of循环，Iterator接口主要供for…of消费。（意味着for…of是Iterator接口的上层表现形式，我们在使用for…of的遍历集合的时候，底层运行逻辑为遍历部署了iteraor接口的集合。）

#### Iterator遍历过程：

- 创建一个指针对象，指向当前数据结构的起始位置，也就是说，遍历器对象本质上就是一个指针对象。
- 第一次调用指针对象的next()方法，可以将指针指向数据结构的第一个成员。
- 第二次调用指针对象的next()方法，指针就指向数据结构的第二个成员。
- 不断调用next()方法，直到它指向数据结构的结束位置。

每次调用next()方法，都会返回数据结构的当前成员的信息。具体来说就是返回一个包含value和done两个属性的对象。其中value属性是当前成员的值，done属性是一个布尔值，表示遍历是否结束。

####   默认Iterator接口

ES6规定，默认的Iterator接口部署在数据结构的Symbol.iterator属性，或者说，一个属性只要具有Symbol.iterator属性，就认为是“可遍历的”。Symbol.iterator属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。至于属性名Symbol.iterator，它是一个表达式，返回Symbol对象的iterator属性，这是一个与定义好的，类型为Symbol的特殊值。

凡是原生具备Iterator接口的数据结构，不用任何处理就可以被for…of循环遍历。原生具备Iterator接口的数据结构包括Array、Map、Set、String、TypedArray、函数的arguments对象、NodeList对象。



### for…of循环

一个数据结构只要部署了Symbol.iterator属性，就可以用for…of循环遍历它的成员。

解构赋值时，会默认调用Symbol.iterator方法。

拓展运算符（…）也会默认的iterator接口。

yield* 后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。



### Generator

Generator函数是es6提供的一种异步编程解决方案，通过yield关键字把函数的执行流挂起，为改变执行流程提供了可能，语法行为与传统函数完全不同。

Generator函数可以理解成一个状态机，封装了多个内部状态。另外，它还是一个遍历器对象生成函数，执行Generator函数会返回一个遍历器对象，可以依次遍历Gnerator函数内部的每个状态。

#### 语法

- 在function后面，函数名之前有个*，用来表示函数是Generator函数；
- 函数内部有yelid表达式，用来定义函数内部的状态。

#### 执行机制

调用Generator函数和调用普通函数一样，在函数名后面加上（）即可，但是Generator函数不会像普通函数一样立即执行，而返回一个指向内部状态对象的指针，所以要调用遍历器对象Iterator的next()方法，指针就会从函数头部或者上一次停下来的地方开始执行。

```js
function * fn(){
    let p1 = yield 1000;
    console.log(p1);//222
    let p2 =  yield 2000;
    console.log(p2);//333
    yield 3000;
}
var result = fn();
console.log(result.next(111));//Object { value: 1000, done: false }
console.log(result.next(222));//Object { value: 2000, done: false }
console.log(result.next(333));//Object { value: 3000, done: false }
console.log(result.next(444));//Object { value: undefined, done: true }

```

调用Generator函数，返回一个遍历器对象，代表Generator函数的内部指针。以后每次调用遍历器对象的next方法，就会返回一个存着有value和done两个属性的对象。value表示当前的内部状态值，是yield表达式后面的表达式的值；done属性是一个布尔值，表示遍历是否结束。

#### yield表达式

由于Generator函数返回的遍历器对象，只有调用next()方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数，yield表达式就是暂停标志。

运行逻辑如下：

- 遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。
- 下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。
- 如果没有遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。
- 如果没有return语句，则返回的对象的value属性值为undefined。

#### next方法的参数

yield表达式本身没有返回值，或者说她的返回值是undefined。next()方法可以带一个参数，该参数就是上一个yield的表达式的返回值。



### async

async函数其实就是Generator函数的语法糖，它使得异步操作更加方便。

async函数就是将Generator函数的*替换成async，将yield替换成await。

async函数返回一个Promise对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

#### async函数对Generator函数的改进

- 内置执行器

  Generator函数的执行必须以考执行器（next()），而async函数自带执行器，与普通函数一样，调用即执行。

- 更好的语义

  async和await比起*和yield，语义更清楚。

- 更广的适用性

  co模块约定，yield命令后面只能是Thunk函数或者Promise对象，而async和await命令后面，可以是Promise对象和原始类型的值（数值、字符串

  布尔值，但这时会自动转成立即resolved的Promise对象）。

- 返回值是Promsie

  async函数的返回值是Promise，而Generator函数的返回值是Iterator对象，这样更方便使用then方法指定下一步的操作。

进一步说，async函数完全可以看作多个异步操作，包装成的一个Promise对象，而await命令就是内部then命令的语法糖。

#### 语法

- async函数返回一个Promise对象。

- async函数内部return语句返回的值，会成为then方法回调函数的参数。

  ```js
  async function fun(){
  	return 'hello world';
  }
  fun().then(value => console.log(value));//'hello world'
  
  ```

- async函数内部抛出错误，会导致返回的Promsie对象变为reject状态。抛出的错误对象会被catch方法回调函数收到。

  ```js
  async function fun(){
  	throw new Error('error!');
  }
  fun().then(
  	value => console.log(value),
      err => console.log(err)
  );//error!
  
  ```

#### await命令

正常情况下，await命令后面是一个promise对象，返回该对象的结果。如果不是Promise对象，就直接返回对应的值。

如果await命令后面是一个thenable对象（即定义了then方法的对象），那么await会将其等同于Promise对象。比如Sleep对象的实例。

await命令后面的promise对象如果变为reject状态，则reject参数会被catch方法的回调函数接收到。

```js
async function fun(){
	await Promise.reject("error!");
}
fun().then(value => console.log(value)).catch(err => console.log(err));//error!

```

任何一个await语句后面的Promise对象变为reject状态，那么async函数都会中断执行。

**所以，最好把await命令放在try…catch语句中，这样不管这个异步操作是否成功，第二个await都会执行。如果有多个await命令，可以统一放在try…catch结构中。**

**多个await命令后面的异步操作如果不存在继发关系，最好让他们同时触发，这样可以缩短程序的执行时间。**比如：

```js
let p1 = await p1Fun();
let p2 = await p2Fun();
//p1和p2互不依赖，可以让他们同时触发，可以写成如下：
let [p1,p2] = await Promise.all([p1Fun(),p2Fun()]);
//或者
let p1Promise = p1Fun();
let p2Promise = p2Fun();
let p1 = await p1Promise;
let p2 = await p2Promise;

```

如果确实希望多个请求并发执行，可以使用Promise.all方法，如上面第二种方法。

await命令只能在async函数之中，如果用在普通函数，就会报错。

async函数会保留运行栈。

```js
//由于b()是异步任务，b()运行结束时，a()可能早就结束了，b()所在的上下文环境已经消失了。如果b()，c()报错，错误栈将不包括a()。
const a = () => {
    b().then(() => c());
}

//如果是async函数，b()运行的时候，a()只是暂停执行，上下文环境都保存这，b()，c()报错，错误栈会包括a()。
const a = async() => {
	await b();
    c();
}

```



ES6语法学习资料大多来自如下，致谢：

- 阮一峰《ECMAScript6入门教程》https://es6.ruanyifeng.com/
- 菜鸟教程https://www.runoob.com/w3cnote/es6-tutorial.html