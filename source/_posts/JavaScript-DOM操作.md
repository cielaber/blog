---
title: JavaScript-DOM操作
tags: [JavaScript,DOM]
categories: JavaScript
img: /image/7.jpg
cover: false
summary: 第九章：js中DOM操作
abbrlink: a33b
date: 2020-08-23 21:15:58
---

# JavaScript学习笔记



## 第九章 DOM操作

### 1、DOM

Document Object Model，文档对象模型，是W3C组织推荐的处理可拓展性语言的标准编程接口。在网页上，组织页面（文档）的对象被组织在一个树形结构中，用来表示文档中对象的标准模型称为DOM，如标签、文本、属性等。

### 2、节点

#### 2-1、节点介绍

在加载HTML页面时，web浏览器生成一个树型结构，用来表示页面内部结构，称为DOM树，DOM将这种树型结构解释为由节点组成。

- 整个文档时一个文档节点

  DOM中的根节点，是文档内其他节点的访问入口，提供了操作其他节点的方法。

- 每个HTML元素是元素节点

- HTML元素内的文本是文本节点

- 每个HTML属性是属性节点

- 注释是注释节点

{% asset_img 9-1.jpg %}

#### 2-2、节点基本属性

- nodeType

  表示节点的类型，只读，节点类型由在Node类型中定义的数值常量来表示，比较重要的节点类型如下：

  - 元素--1
  - 属性--2
  - 文本--3
  - 注释--8
  - 文档--9

- nodeName

  节点的名称，只读。

  - 元素节点的nodeName与标签名相同，且都均为大写。
  - 属性节点的nodeName与属性名相同。
  - 文本节点的nodeName始终是#text。
  - 文档节点的nodeName始终是#document。

- nodeValue

  该属性规定节点的值。

  - 元素节点的nodeValue是undefined或null。

  - 文本节点的nodeValue是文本本身。

  - 属性节点的nodeValue是属性值。

    - 获取属性节点：DOM元素.getAttribute(“属性名称”)，该方式可以获取自定义属性，可以直接用class获取而不是className。

    - 设置属性节点：DOM元素.setAttribute(“属性名称”,“属性值”)，就是直接修改标签属性。
    - 移除属性节点：DOM.removeAttribute(‘属性名称’)，直接删除标签属性。

### 3、获取节点

#### 3-1、获取子节点

- 父节点.children

  只返回子元素（标签）节点集合，该属性为非标准属性。

  IE8及以下浏览器支持该属性，但返回节点集合中除了元素节点还包含注释节点。

- 父节点.childNodes

  返回子节点集合，包括标签、文本、属性节点。

  对于元素间的空格，IE8及以下版本不会返回文本节点。

#### 3-2、获取父节点

- 元素.parentNode

  指向文档树中的父节点，只获取一个直接父元素。

- 元素.offsetParent

  指向离当前元素最近的有定位属性的父级元素，如果没有定位的父级，则指向body。

#### 3-3、其他节点

- ##### 获取父元素的首节点

  - 父元素.firstChild

    可以获取所有类型节点，且只能拿一个。

    IE9及以上和标准浏览器会获取到文本、折行，IE8及以下获取到元素节点。

  - 父元素.firstElementChild

    只获取元素节点，IE8及以下不支持。

- ##### 获取父元素的尾节点

  - 父元素.lastChild

    可以获取所有类型节点，且只能拿一个。

    IE9及以上和标准浏览器会获取到文本、折行，IE8及以下获取到元素节点。

  - 父元素.lastElementChild

    只获取元素节点，IE8及以下不支持。

- ##### 获取元素的下一个兄弟节点

  - 元素.nextSibling

    可以获取所有类型节点，且只能拿一个。

    IE9及以上和标准浏览器会获取到文本、折行，IE8及以下获取到元素节点。

  - 元素.nextElementSibling

    只能获取元素节点，IE8及以下不支持。

- ##### 获取元素的上一个兄弟节点

  - 元素.previousSibling

    可以获取所有类型节点，且只能拿一个。

    IE9及以上和标准浏览器会获取到文本、折行，IE8及以下获取到元素节点。

  - 元素.previousSibling

    只能获取元素节点，IE8及以下不支持。

以上四种方法均存在兼容问题，解决方法如下：

```js
var first = box.firstElementChild || box.firstChild
var last = box.lastElementChild || box.lastChild
var next = box.nextElementSibling || box.nextSibling
var previous = box.previousElementSibling || box.previousSiblng
```

### 4、操作节点

#### 4-1、创建节点

- document.createElement(“标签名”)

  该方法接收一个字符串标签名，返回一个标签对象的引用。

- document.createTextNode(“文本”)

  该方法创建一段文本，返回文本对象的引用。

```js
//创建标签节点
var p = document.createElement("p");
//创建文本节点
var text = document.createTextNode('创建的文本节点');
//将文本添加到标签内
p.appendChild(text);
//将标签添加到页面中
document.body.appendChild(p);
//但是更多情况下，是直接用标签的innerHTML属性来添加内容，一步可以代替上面的2、3两步
p.innerHTML = 'innerHTML方法添加的内容';
```

#### 4-2、追加节点

- 父元素.appendChild(子节点)

  用于向父元素的末尾添加一个节点。

```js
var ul = document.getElementsByTagName('ul')[0];
var li = document.createElement('li');
li.innerHTML = '追加的标签';
ul.appendChild(li);//li添加到ul的尾部
```

#### 4-3、插入节点

- 父元素.insertBefore(要插入的节点，参考的节点)

  该方法接收两个参数，要插入的节点和作为参照的节点，插入节点后，被插入的节点会变成参照节点的前一个兄弟节点，同时被方法返回。没有子元素时以文本节点为参考。

```js
var ul = document.getElementsByTagName('ul')[0];
var aLi = document.getElementsByTagName('li');
var li = document.createElement('li');
li.innerHTML = '插入的标签';
ul.insertBefore(li,aLi[1]);//插入到第二个li的前面，成为了新的第二个li，原第二个标签成为了第三个
```

#### 4-4、删除节点

- 父元素.removeChild(被删除的元素)

  该方法接收一个参数，要删除的节点。

```js
var ul = document.getElementsByTagName('ul')[0];
var aLi = document.getElementsByTagName('li');
ul.removeChild(aLi[1]);//第二个li被删除
```

#### 4-5、替换节点

- 父元素.replaceChild(新的节点，被替换的节点)

  该方法接收两个参数，要插入的节点和被替换的节点。

```js
var ul = document.getElementsByTagName('ul')[0];
var aLi = document.getElementsByTagName('li');
var li = document.createElement('li');
li.innerHTML = '新的标签';
ul.replaceChild(li,aLi[1]);//第二个li被新的标签替换掉
```

如果是用已经存在的节点去替换，已存在的节点会直接从原有位置上消失，在被替换节点的位置覆盖被替换的节点。

#### 4-6、复制节点

- 被复制的节点.cloneNode(布尔值)

  该方法拷贝节点并返回节点副本，接收一个布尔值参数，即是否需要克隆所有后代，默认值是false，只复制标签，不复制内容或子代；true表示复制标签中所有子代。

```js
var ul = document.getElementsByTagName('ul')[0];
var newUl= ul.cloneNode(true);
document.body.appendChild(newUl);//复制一个ul标签，包含其所有原有的内容
```

**如果使用这种方式复制节点，需要使用getElementsByTagName()动态获取元素，不然可能存在获取不到被复制的节点的情况。**

> 注意：以上操作节点，除了创建和复制节点，其他方法均通过父元素操作，使用时分清所在位置。如删除节点中的父元素应该是被删除元素的父元素，而删除事件的操作元素可能是被删除元素的子元素。

### 5、DOM操作表格

#### 5-1、获取表格元素

DOM提供了可以简便快速获取表格元素的属性，先获取到表格table对象，再通过table获取里面的元素。

```html
<table>
    <thead>
        <th>编号</th>
        <th>姓名</th>
        <th>性别</th>
        <th>年龄</th>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>张三</td>
            <td>man</td>
            <td>18</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>sum</td>
        </tr>
    </tfoot>
</table>
```

```js
var table = document.getElementsByTagName('table')[0];
//获取thead，返回thead
console.log(table.tHead);//<thead>...</thead>
//获取tbody，返回一个集合tbody集合
console.log(table.tBodies);//HTMLCollection[tbody]
//获取tfood，返回tfoot
console.log(table.tFoot);//<tfoot>...</tfoot>
//获取所有行，返回一个行集合，包括thead和tfoot中的行
console.log(table.rows);//HTMLCollection(3)[tr,tr,tr]
//获取指定的行，此处获取第一个tbody中的行
console.log(table.tBodies[0].rows);//HTMLCoolection[tr]
//获取单元格，只能通过固定行获取，返回一个集合
console.log(table.rows[0].cells);//HTMLCollection(4)[th][th][th][th]
```

