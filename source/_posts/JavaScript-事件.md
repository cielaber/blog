---
title: JavaScript-事件
tags: JavaScript
categories: JavaScript
cover: false
img: /image/9.jpg
summary: 第十一章：JavaScript事件
abbrlink: b93
date: 2020-08-23 21:25:50
---

# JavaScript学习笔记



## 第十一章 事件

### 1、事件基础

HTML事件是发生在HTML元素上的事情，是可以被JavaScript侦测到的行为。

#### 1-1、事件函数

当事件被触发时调用的函数。

#### 1-2、事件对象

当事件发生时候，浏览器会将事件相关信息保存在内置全局对象window.event中（window可以省略），包括事件类型、操作对象、鼠标位置等。

```js
//由于事件对象是事件触发时由js底层将实参数据保存在window.event对象中，因此需要在功能函数中加上形参来接收事件对象。
document.onclick = function(e){
    //console(e);//标准浏览器
    //console(event);//兼容ie低版本，非标准浏览器没有实参只能通过window.event
    //兼容写法
    var e = e || event;
}
```

事件对象中比较重要的属性：

- type：事件类型

- target：获取事件源

  ```js
  //事件源也存在兼容问题，target在ie低版本中无效，需要使用srcElement进行兼容处理
  document.onclick = function(e){
      var e = e || event;
  	var target = e.target || e.srcElement;
  }
  ```

- clientX、clientY

  鼠标位置坐标，相对于浏览器可视窗口的坐标

- pageX、pageY

  鼠标位置坐标，相对于浏览器文档（即包括被滚动条卷走的部分）

- screenX、screenY

  鼠标位置坐标，相对于屏幕

### 2、事件绑定

- 注册事件

  元素.事件

  ```js
  div.onclick = function(){
      console.log(1);
  }
  ```

  该方式添加事件有一个弊端，如果给一个元素添加多个相同的事件，后面的事件会覆盖前面的事件。

- 注册事件取消

  ```js
  div.onclick = null;
  ```

- 事件监听绑定事件

  标准浏览器：元素.addEventListener(事件类型(不加on)，事件处理函数，是否捕获)。默认false冒泡，true捕获。

  非标准浏览器（ie8及以下）：元素.attachEvent(事件类型(加on)，事件函数)。没有第三个参数，只有冒泡，没有捕获（如果需要捕获需要通过其他方法设置）。

  ```js
  //事件绑定封装
  function addEventL(obj,eName,callback){
  	if(obj.addEventListener){
  		obj.addEventListener(eName,callback,false);
      }else{
  		obj.attachEvent('on' + eName,callback);
      }
  };
  ```

  标准浏览器的事件绑定和非标准浏览器的事件绑定的区别：

  - 非标准浏览器没有捕获，标准浏览器有捕获。
  - 非标准浏览器的事件名称前面有on，标准浏览器没有。
  - 非标准浏览器的this是window，标准浏览器的是触发这个事件的对象。

- 事件监听取消

  标准浏览器和非标准浏览器的绑定方法不一样，取消绑定的方法也不一样。

  - 标准浏览器：元素.removeEventListener(事件类型(不加on)，执行函数，false)
  - 非标准浏览器：元素.detachEvent(事件类型(加on)，执行函数)

  ```js
  //解除绑定兼容处理封装
  function delEventL(obj,eNname,callback){
      if(obj.removeEventListener){
  		obj.removeEventListener(eName,callback,false);
      }else{
          obj.detachEvent('on' + eName,callback);
      }
  };
  ```

### 3、DOM事件流

事件发生时会在元素节点与根节点之间按特定的顺序传播，路径所经过的所有节点都会收到该事件，这个传播过程就是DOM事件流。

#### 3-1、DOM事件流三阶段：

- 事件捕获阶段：事件从最不具体的元素到最具体的元素进行传播，即从DOM的根节点往目标节点传播。
- 确定目标阶段：事件达到目标元素。
- 事件冒泡阶段：事件从最具体的元素到最不具体的元素进行传播，即从目标元素往DOM根节点处传播。

#### 3-2、事件流分为冒泡事件流和捕获事件流

- 冒泡事件流

  当addEventListener()第三个参数为false时，或非标准浏览器的默认事件流，都是冒泡事件流。

  冒泡事件流的元素事件在事件冒泡阶段发生。

- 捕获事件流

  当addEventListener()第三个参数为true时，是捕获事件流。

  捕获事件流的元素事件在事件捕获阶段发生。

```html
<div class="box1">
    div1
    <div class="box2">
        div2
        <div class="box3">
            div3
            <div class="box4">div4</div>
        </div>
    </div>
</div>
```

```js
box[3].addEventListener('click',function(){console.log('box4')},true)
box[2].addEventListener('click',function(){console.log('box3')})
box[1].addEventListener('click',function(){console.log('box2')})
box[0].addEventListener('click',function(){console.log('box1')},true)
//点击div4时，结果如下图。
//分析：根据DOM事件流，点击事件从进行捕获阶段div1到div4，到达目标div4之后，再进行冒泡阶段从div4到div1。
//由于div1和div4支持捕获，而div2和div3支持冒泡（默认false为冒泡），所以div1和div4的点击事件在捕获阶段发生，而div3和div2的点击事件在冒泡阶段发生。
```

{% asset_img 11-1.png %}

注意：事件流到达目标阶段后将不再往子节点传播，即点击div3时，事件流达到div3后往根节点冒泡，达不到div4，所以div4的点击事件不会发生。

{% asset_img 11-2.png %}

#### 3-3、阻止冒泡事件

阻止冒泡事件是指：子元素接收到事件后，阻止子元素再给父元素传播事件。

- 标准浏览器：事件对象.stopPropagration();

- 非标准浏览器：事件.cancelBubble = true;

```js
//封装如下
function stopBubble(e){
	if(e.stopPropagation){
		e.stopPropagation();
    }else{
		e.cancelBubble = true;
    }
};
```

### 4、事件默认行为

事件的默认行为，即赋予了元素特殊的操作，如点击链接会跳转到其他的网页，在浏览器中右击鼠标会弹出菜单，当我们不需要这些默认行为的时候，可以手动阻止。

- 元素.事件添加的事件：return false
- 元素.addEventListener()：e.preventDefault()
- 元素.attachEvent()：e.returnValue = false

```js
//封装如下
function stopDefault(e){
	if(e.preventDefault){
		e.preventDefault();
    }else{
		e.returnValue = false;
    }
};
```

### 5、键盘事件

常用键盘事件如下，当一个键盘被按下的时候，事件发生次序如下：

- onkeydown：键盘按下
- onkeypress：键盘按下（但是该事件不适用于alt、shift，ctrl、esc等键盘。）
- onkeyup：键盘抬起

常用属性如下：

- e.keyCode：返回按键对应键位的键值

  常用键值为：

  - 左上右下：37、38、39、40
  - Enter：13
  - Shift：16
  - Control：17
  - Alt：18

- key：返回具体的键名称（ie8以下不支持）

- shiftKey：判断shift键是否按下，是则返回true否则返回false。

- ctrlKey：判断ctrl键是否按下，是则返回true否则返回false。

- altKey：判断alt键是否按下，是则返回true否则返回false。

- metaKey：判断meta键（window键）是否按下，是则返回true否则返回false，ie所有版本都不支持。

```js
document.onkeydown = function(e){
    var e = e || window.event;
    console.log(e.key);
    console.log(e.keyCode);
    console.log(e.shiftKey);
    console.log(e.altKey);
    console.log(e.ctrlKey);
    console.log(e.metaKey);
}
```

### 6、滚轮事件

鼠标中间的滚轮滚动时，js也能监听到，称为滚轮事件。

#### 6-1、添加滚轮事件

添加滚轮事件中火狐与其他浏览器有所区别：

- 标准和非标准浏览器：元素.onmousewheel = fun;//可以使用传统方式绑定事件或者使用事件监听绑定
- 火狐：元素.addEventListener(‘DOMMouseScroll’,fun,false);//火狐只能使用事件监听方式

```js
//封装如下
function addMousewheel(obj,eName,fun){
	if(obj.addEventListener){
		obj.addEventListener(eName,fun,false);
    }else{
		obj.attachEvent('on' + eName,fun);
    }
};
//标准和非标准
addMousewheel(div,'mousewheel',fun);
//火狐
addMousewheel(div,'DOMMouseScroll',fun);
```

#### 6-2、获取滚轮信息

滚轮可以上下滚动，滚动方向的信息都存储在对象事件的一个属性中，火狐和其他浏览器有不同的写法。

- 标准和非标准：e.wheelDetal	向上滚动120、向下滚动-120（不同浏览器可能值不一样）
- 火狐：e.detail    向上滚动-3，向下滚动3

由于不同浏览器值不一样，并且快速滚动滚轮时值可能加倍，所以不能用确定值来判断滚动方向，一般用是否大于0来判断。且由于火狐于其他浏览器在滚轮不同方向的值正负不一样，所以一般在判断时乘以-1使其方向相同。

```js
//封装如下
function wheelDelta(e){
	if(e.wheelDelta){
        return e.wheelDelta;
    }else{
        return e.detail;
    }
};
```

### 7、事件委托/事件代理

事件委托就是利用事件冒泡，只指定一个事件处理程序，就可以管理某一类的所有事件。

解决的问题：在一类元素中，如果新添加了一个元素，之前添加的事件在该元素上无效，需要重新添加，或者重新获取所有元素。

实现：将事件添加到父元素上，当事件发生时，父元素会找到对应触发事件的子元素去处理，后期添加的子元素依然有该事件。通常结合子元素的nodeName属性对子元素进行判断。

原理：冒泡原理，当里层元素做点击事件的时候，都会冒到外层父元素中，父元素中点击事件的功能都将实现。

```html
<ul>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

```css
li{
    background-color: darkkhaki;
    margin: 10px;
}
```

```js
//案例：通过给ul添加点击事件，当点击子元素li时，相应li的背景色变红。
var ul = document.getElementsByTagName('ul')[0];
ul.onclick = function (e) {
    var e = e || event;
    var target = e.target || e.scrElement
    if (target.nodeName == 'LI') {
        target.style.backgroundColor = "red";
    }
};
ul.innerHTML += "<li></li>";//后期添加的子元素依然有效
```

