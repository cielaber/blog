---
title: Vue-入门
tags: Vue
categories: 前端框架
img: /image/19.jpg
summary: 第一章：Vue入门
abbrlink: 84c5
date: 2020-09-25 09:49:43
---

# 前端框架学习笔记

## 第一章 Vue入门

### 节点赋值（复习）

- 非表单元素

  js：innerHTML

  jQuery：html()

- 表单元素

  js：value()

  jquery：val()

- 媒体元素

  js：src

  jQuery：attr(‘src’,‘1.jpg’)



### Vue介绍

官网：https://cn.vuejs.org

Vue是一款渐进式JavaScript框架，采用自底向上增量开发的设计，只关注视图层，易于上手。

Vue核心：数据驱动+组件系统

优点：

- 模块友好化
- 易用、灵活、高校
- SPA(single page application)单页面应用，用户体验好

缺点：

- Vue不兼容ie8及以下版本
- 首屏加载慢
- 不利于SEO优化

安装：

- CDN引入

  ```html
  <!-- 开发环境版本，包含了有帮助的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

  <!-- 生产环境版本，优化了尺寸和速度 -->
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  ```
  
- NPM下载

  ```shell
  npm install vue
  ```

- cli（脚手架）

  详见Vue组件章节。



### 使用Vue

```js
new Vue({
	el:"#div",
    data:{
		name:'zs',
        age:18,
        src:'1.jpg'
    },
    methods:{
		fn(){
            //...
        }
    }
})
```

#### el：挂载点

- 不能挂载到body、html上，要挂载到正常的元素上。
- 一个vue实例只能挂载到一个节点上，所以一般使用id选择器。
- 如果有好几个元素都满足el的选择，vue实例挂载到满足条件的第一个节点上。

#### data：属性

存放vue对象的属性（变量），需要先声明后使用，且只有在vue对象内的变量才在该vue实例中有用。

#### methods：方法

存放vue对象的方法。



### 绑定数据（非表单元素）

#### 模板语法

要在HTML元素中使用vue，可以使用双大括号，双大括号内必须使用js语法，但是每一个双大括号内只能有一句js，可以是变量、三目运算符、方法等，但是不能有if、for等，且双大括号内不能有{}。

```html
<div id='div'> {{name+':'+age}} </div>
<!-- zs:18 -->
```

优点：方便、简单

缺点：不能解析标签；首屏会出现闪屏问题（vue加载有延迟，造成双大括号暂时无法解析。）

#### v-html

```html
<div id='div' v-html="'hello,'+name"></div>
<!-- hello,zs -->
```

优点：可以解析标签（一般用在首屏和详情页）

缺点：不方便

#### v-text

```html
<div id='div' v-text="'hello,'+name"></div>
<!-- hello,zs -->
```

缺点：不方便，不能解析标签。



### MVVM模式（表单元素）

v-model

M-model模型、V-view视图、VM-viewModel视图模型

模型通过 视图模型 控制视图，视图通过 视图模型 修改模型。

```html
<!-- div中的name会随着input中的输入值改变而改变 -->
<input v-model='name'>
<div>{{name}}</div>
```



### 属性绑定（媒体元素）

v-bind

可以绑定已有或者自定义属性。

```html
<div id='div'>
    <img v-bind:src='src'>
</div>

<!-- 可以简写成 -->
<div id='div'>
    <img :src='src'>
</div>
```

注意点：当HTML标签的属性绑定了vue之后的变量之后，该属性后面引号内的值就是js语法，这里会出现数据类型的问题。

```html
<!-- 这里的age是字符串类型的10 -->
<div id='div' age='10'></div>

<!-- 这里的age是数值类型的10 -->
<div id='div' :age='10'></div>

<!-- 这里的age是字符串类型的10 -->
<div id='div' :age="'10'"></div>
```



### 条件渲染

v-if：条件渲染，如果条件为true则节点显示，条件为false则节点消失（节点不加载/惰性加载）。

v-show：条件渲染，如果条件为true则节点显示，条件为false则节点消失（新增属性display：none）。

使用：频繁切换时使用v-show，不频繁切换使用v-if。

```html
<div v-if='1'>显示</div>
<div v-if='0'>消失</div>

<div v-show='true'>显示</div>
<div v-show='false'>消失</div>
```

v-else可以与v-if搭配使用，但是相邻的两个之间不能有任何元素间隔。

```html
<div v-if='true'>显示</div>
<div v-else>消失</div>
```



### 循环渲染

v-for

```html
<ul id='ul'>
  <li v-for="(item,index) in comment">{{index}}：{{item}}</li> 
</ul>
```

```js
new Vue({
    el:'ul',
    data:{
        comment:['非常好','一般般','很棒，下次还来买！','赞']
    }
})
```

与key关键字搭配使用，实现Vue的v-for更新DOM时实现就地更新，只渲染被改变的DOM，提高更新性能。key后面跟上数据的唯一标识，并且用v-bind指令绑定该属性。

```html
<li v-for="(item,index) in data" :key='item.id'></li>
```



### 样式

#### 动态行间样式

```html
<!--语法-->
<div :style='json'></div>
```

```html
<div id="app">
    <div :style="{background:bg1}"> </div>
    <div :style="bg2">NNNN</div>
</div>
```

```js
new Vue({
    el:"#app",
    data:{
        bg1:'red',
        bg2:{
            background:'blue',
            borderRadius:'5px'
        }
    }
})
```

#### 动态类名

- 变量：v-bind:class=‘变量’

  ```html
  <div class='box' :class="className"></div>
  ```

- 三元运算符：:class=‘[逻辑]’，虽然不用[]也可以实现，但是Vue官方建议用[]包裹运算逻辑。

  ```html
  <!--通过控制isRed变量的值来控制选择是red类名还是blue类名，这种情况只能控制两种类-->
  <div :class="[isRed?'red':'blue']"></div>
  ```

- 多类名使用：:class=“{class1:true,class2:false,…}”，类名后面可以跟逻辑运算，该方法可以便捷实现隔行变色。

  ```html
  <div v-for="(item,index) in arr" :key="item.id" :class="'red':index%3==0,'blue':index%3==1,'yellow':index%3==2"></div>
  ```

  

