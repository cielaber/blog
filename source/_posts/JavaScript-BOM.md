---
title: JavaScript-BOM
tags: [JavaScript,BOM]
categories: JavaScript
cover: false
img: /image/8.jpg
summary: 第十章：js中BOM模型
abbrlink: 20ad
date: 2020-08-23 21:21:10
---

# JavaScript学习笔记



## 第十章 BOM

### 1、BOM-window对象

BOM全称Browser Object Model-浏览器对象模型，提供了很多对象，用于访问浏览器的功能，没有规范，由各个浏览器厂商制定标准，兼容性差。

BOM的核心是window：

- 所有浏览器都支持window对象，它表示浏览器窗口。
- 所有js的全局对象、函数及变量均自动成为window对象的成员。
- 全局变量是window对象的属性。
- 全局函数是window对象的方法。
- HTML DOM的document也是window对象的属性之一。

#### 1-1、window的子对象主要有：

- JavaScript document对象
- JavaScript frames对象
- JavaScript history对象
- JavaScript location对象
- JavaScript navigator对象
- JavaScript screen对象

#### 1-2、window尺寸

有三种方法能够确定浏览器窗口的尺寸（不包括工具栏和滚动条）。

- 对于IE9及以上、及其他标准浏览器
  - window.innerHeight -浏览器窗口内部高度
  - window.innerWidth -浏览器窗口的内部宽度
- 对于IE8及以下
  - document.documentElement.clientHeigth
  - document.documentElement.clientWidth
- 或者，用以下方法获取body的高度，当页面没有内容时，以下方法获取的高度为0，但上面两种方法能获取到页面高度。
  - document.body.clientHeight
  - document.body.clientWidth

document.documentElement.clientHeight与document.body.clientHeight都是获取页面可视度高度。但是在不同的html声明中获取的高度不一样，一定要注意。

如果加了`<!DOCTYPE html>`声明使用document.documentElement.clientHeight获取正常，但是使用document.body.clientHeight获取就有问题。

如果缺少`<!DOCTYPE html>`声明使用document.documentElement.clientHeight获取有问题，但是使用document.body.clientHeight获取正常。

因为`<!DOCTYPE html>`是最新的html5标准，所以建议使用document.documentElement.clientHeight获取。

实用的js解决方案为：

```js
var width = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth
var height = widow.innerHeight || document.documentElement.clientHeight || document.body.clientHeiht
```

### 2、window的open与close方法

#### 2-1、open()

该方法用于打开一个新的浏览器窗口。

语法：window.open(url，name，specs，repalce)

- url：打开指定url的页面，如果省略，打开新的空白窗口。
- name：指定target属性或窗口的名称，值如下：
  - _blank –新窗口打开，默认。
  - _self –当前窗口打开
  - _parent --url加载到父框架
  - _top --url替换任何可加载的框架集
  - name --窗口名称
- specs：一个逗号分隔的项目列表，如窗口大小、是否显示菜单栏、是否显示滚动条、是否显示地址字段等。
- replace：新窗口是否取代浏览器记录中的位置。
  - true --url替换浏览历史中的当前条目
  - false --url在浏览器中创建新的条目

```html
<button>打开百度</button>
```

```js
var btn = document.querySelector('button');
btn.onclick = function(){
    window.open('http://www.baidu.com');
}
```

#### 2-2、close()

该方法用于关闭浏览器窗口，该方法没有参数。

语法：window.close()

```js
var btn = document.querySelector('button');
btn.onclick = function(){
    window.close();
}
```

### 3、location对象

location是最有用的BOM对象之一，它用于获得当前页面的url地址，并把浏览器定向到新的页面。事实上，location是一个很特别的对象，因为它既是window对象的属性，也是document对象的属性。

#### 3-1、location对象属性

- hash --返回url的锚部分（#后的部分），没有则返回空字符串。

- host --返回url的主机名和端口

- hostname --返回不带端口的url主机名

- href --返回完整的url，location对象的toString()方法也返回这个值，也可以用来跳转页面。

  ```js
  location.href = 'http://www.baidu.com';//跳转网页
  console.log(location.href);//获取url
  ```

- pathname --返回url路径名

- port --返回url服务器使用的端口号

- protocol --返回一个url协议

- search --返回一个url的查询字符串部分（？之后的部分）

#### 3-2、location对象方法

- location.assign(url)：载入一个新的文档
- location.reload()：重新载入当前文档
- location.repalce(newurl)：用新的文档代替当前文档

```html
<button>打开百度</button>
```

```js
var btn = document.querySelector('button');
btn.onclick = function(){
    window.location = 'http://www.baidu.com';
}
```

### 4、history对象

history保存了用户的上网记录，在有页面跳转的情况下，可以实现前后跳转。

go()方法的参数可以是数字，数字代表的是要访问的url在history的url列表中的相对位置（-1后退一个，-2后退两个，1向前一个，以此类推。）。参数也可以是一个字符串，字符串必须是局部或者完整的url，该方法会去匹配字符串的第一个url。

- 向前跳转一个页面

  ```js
  history.forward();//方法1
  history.go(1);//方法2
  ```

- 向后跳转一个页面

  ```js
  history.back();//方法1
  history.go(-1);//方法2
  
  ```

### 5、navigator对象

该对象包含浏览器的相关信息，一般用在响应式布局中。

```js
if ((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
    window.location.href = "../H5/index.html"; //手机index文档所在目录
}else{
    window.location.href = "../PC/index.html"; // PCindex文档所在目录
}

```

### 6、body位置属性

#### 6-1、offset系列

- 获取元素的占位宽高
  - 元素.offsetWidth：width+padding+border
  - 元素.offsetHeight：height+padding+border
- 获取元素在页面的位置
  - 元素.offsetTop：当前元素的顶部，到定位父元素的距离（不包含父元素边框和父元素的外边距），没有定位父元素，则到body的距离。
  - 元素.offsetLeft：当前元素的左部，到定位父元素的距离（不包含父元素边框和父元素的外边距），没有定位父元素，则到body的距离。

#### 6-2、client属性

- 屏幕的可视宽高
  - document.documentElement.clientWidth
  - document.documentElement.clientHeight
- 元素可视宽高
  - 元素.clientWidth(width+padding)
  - 元素.clientHeight(height+padding)
- 元素边框
  - 元素.clientTop(元素本身上边框高度)
  - 元素.clientLeft(元素本身左边框高度)

#### 6-3、scroll系列

Google认为scroll是属于body的，其他浏览器认为scroll是html的，所以存在兼容问题。

- 滚动事件

  window.onscroll：滚动条滚动的时候调用

- 屏幕的滚动距离，页面被卷去的高度

  - document.documentElement.scrollTop
  - document.body.scrollTop

- 元素的滚动

  - 元素.scrollTop：被卷去的高
  - 元素.scrollLeft：被卷去的宽
  - 元素.scrollWidth：获取元素实际内容宽（元素宽度-滚动条宽度）
  - 元素.scrollHeight：获取元素实际内容高（包括被滚动条卷走的高度）

### 7、图片懒加载

图片懒加载是指先只加载可视窗口区域的图片，当用户向下拖动滚动条时再加载后面显示出来的图片。

- 这样减少了加载时的线程数量，使可视区域内的图片也能快速加载，优化了用户体验。
- 减少了同一时间向服务器发出的请求数量，服务器压力剧减。

方法：在写网页<img>标签时先不将图片的路径放入src属性，而是自定义一个其他属性data_src，将图片路径放入到这个自定义的属性中，在加载页面时，这个图片一开始是无法加载的，当浏览器的可视区域移动到此图片上来时，将data_src中的路径赋值给src属性，图片开始加载出来。

```html
 <img src="" data_src='./images/background-image.jpg' alt="">
 <img src="" data_src='./images/background-image.jpg' alt="">
 ...

```

```js
var img = document.getElementsByTagName('img');
var tag = 0;//记录图片被加载的数量，每次遍历从没有加载的图片开始加载
//当打开界面时先加载当前可视区的图片
lazyLoad();
//滚轮事件绑定懒加载函数
window.onscroll = lazyLoad;
function lazyLoad(){
    for(var i = tag;i < img.length;i++){
        //获取可视窗口高度
        var seeHeight = document.documentElement.clientHeight;
        //获取被滚动条卷走的高度
        var scrollHeight = document.documentElement.scrollTop || document.body.scrollTop;
        //比较图片距离文档顶部的距离是否小于可是高度与被卷高度和，如果小于说明图片进入了可视区域
        if(img[i].offsetTop < seeHeight + scrollHeight){
            //判断图片路径是否为空，若为空则给src赋值
            if(img[i].getAttribute("src") == ''){
                //将图片路径赋给src
                img[i].src = img[i].getAttribute("data_src");
                //被加载的图片数量+1
                tag += 1;e
            }
        }
    }
}
```

### 8、resize事件

当屏幕发生变化时不间断的调用。

```js
window.onresize = function(){
    var clientWidth = document.documentElement.clientWidth;
    console.log(clientWidth);
}
```

