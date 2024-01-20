---
title: Vue-表单数据绑定、修饰符和生命周期
tags: Vue
categories: 前端框架
img: /image/20.jpg
summary: 第二章：Vue表单数据绑定、修饰符、生命周期
abbrlink: a0dc
date: 2020-09-25 14:02:06
---

# 前端框架学习笔记

## 第二章 Vue表单数据绑定、修饰符、生命周期

### 表单数据绑定

所有表单元素都可以使用v-model绑定数据。

##### 文本

表单中的文本包括文本框和文本域，都可以通过v-model进行数据的双向绑定。

```html
<input type="text" v-model='value'>
<textarea v-model='value' name="" id="" cols="30" rows="10"></textarea>
```

##### 单选钮

单选钮中v-model绑定的是value，哪一个被选中，被绑定的变量的值就是对应的value值。

```html
<input type="radio" name="" id="" v-model='sex' value='man'>男
<input type="radio" name="" id="" v-model='sex' value='women'>女
<!--男被选中sex值为man，女被选中sex值为women-->
```

##### 复选框

复选框中，v-model绑定的也是value，但是区分单个和多个。

如果只有一个复选框或者绑定的变量hobby为一个字符串（这种情况即使有多个复选框，只要改变一个复选框的状态，其他复选框状态也会跟着被改变。），则hobby的值为true（被选中）或false（未选中）。

如果被绑定的变量hobby为一个数组，则数组的值就是对应的value值，值的排列顺序为复选框被选中的先后顺序。

```html
<input type="checkbox" name="" id="" v-model='hobby' value='0'>音乐
<input type="checkbox" name="" id="" v-model='hobby' value="1">舞蹈
<!--依次选中舞蹈和音乐，hobby的值为['1','0']-->
```

拓展：上述案例中如果需要把hobby的值变为数值类型，则需要**在被绑定的属性前使用v-bind指令，将被绑定的属性后面变为js语法**，如下：

```html
<input type="checkbox" name="" id="" v-model='hobby' :value='0'>音乐
<input type="checkbox" name="" id="" v-model='hobby' :value="1">舞蹈
<!--依次选中舞蹈和音乐，hobby的值为[1,0]-->
```

##### 选择列表

v-model绑定选择列表（列表多选需要加上mutiple属性），和复选框一样，被选择的选项的value组成数组。

#### 表单修饰符

- .lazy-默认情况下通常是输入数据时绑定数据，如果加上了改修饰符，则被绑定的值不会立即改变，而是会等到输入框失去焦点时改变。
- .number-将绑定的数据的数据类型转为number类型，当然，仅限数值类型的字符串。
- .trim-该修饰符能够自动过滤用户输入的首尾空白字符串。

```html
<input type="text" v-model.lazy="name">
```



### 事件

#### 绑定事件

```html
<button v-on:click='fun()'>点击绑定fun</button>
<!-- 可以简写成如下 -->
<button @click='fun()'>点击绑定fun</button>
<!--当函数没有参数时，可以省略括号-->
<button @click='fun'>点击绑定fun</button>
```

#### 事件对象：*`$event`*

```html
<!--显示获取event对象-->
<button @click='fun($event)'>获取event</button>
<!--隐式获取event对象，绑定函数时，圆括号不写，默认参数就是event对象。如果写了圆括号但是没有参数，则e就是undefined。-->
<button @click='fun'>获取event</button>
```

```js
new Vue({
	el:'#app',
    methods:{
		fun(e){
			console.log(e);
        }
    }
})
```

#### 阻止事件默认行为

vue修饰符写法：

```html
<div @contextmenu.prevent='fun'>阻止右键默认行为</div>

<!--还可以搭配按键码使用-->
<div @keydown.38.prevent='fun'>阻止上方向键的按下默认行为</div>
<!--38代表的是上方向键，在输入框中输入文字后按下上方向键焦点会从文字末尾跳到文字最前面，有些情况需要阻止该默认行为。-->

```

也可以在函数中自动阻止：

```js
new Vue({
	el:'app',
    methods:{
        fun(e){
			e.preventDefault();
            //由于vue不支持低版本，不需要考虑低版本兼容。
        }
    }
})

```

原生兼容封装写法：

```js
function stopDefault(e){
	if(e.preventDefault){
		e.preventDefault();
    }else{
		e.returnValue = false;
    }
}

```

#### 阻止事件传播

vue修饰符写法：

```html
<div @click.stop='fun'>阻止点击事件冒泡</div>

```

在函数中手动阻止：

```js
new Vue({
	el:'app',
    methods:{
        fun(e){
			e.stopPropagation();
            //由于vue不支持低版本，不需要考虑低版本兼容。
        }
    }
})

```

原生兼容封装写法：

```js
function stopBubble(e){
	if(e.stopPropagation){
        e.stopPropagation();
    }else{
        e.cancelBubble = true;
    }
}

```

#### 事件修饰符

- .prevent-阻止默认行为
- .stop-阻止事件传播
- .once-控制指定的事件只执行一次
- .self-触发的目标元素是自身才执行
- .capture-捕获
- .native-解决组件绑定不上事件

```html
<!--阻止右击默认事件-->
<div class="box" @contextmenu.prevent="yj"></div>

```

#### 键盘修饰符

键盘修饰符可以是按键名称，也可以时是对应的编码值。

- .left(.37)
- .up(.38)
- .right(.39)
- .down(.40)
- .enter(.13)
- .tab
- .delete(包括delete和backspace键)
- .esc
- .space-特定键盘按键的修饰符。

```html
<input type="text" @keydown.left.prevent="left" @keydown.right="right">

```

#### 鼠标修饰符

- .left-鼠标左键
- .middle-鼠标中间滚轮
- .right-鼠标右键

```html
<button @click.right="right">点击</button>

```

注意和上面的键盘修饰符区分，前面的事件有所不同。



### 生命周期

Vue实例在被创建之前都要有一个初始化过程，有一个完整的生命周期，也就是从创建开始、数据初始化、编译模板、挂载DOM、渲染、更新、卸载等一系列过程，被称为Vue的生命周期。同时在这个过程中也会运行一些叫做声明周期钩子的函数，可以在不同阶段的函数内添加代码。

{% asset_img 2-1.png %}



```js
var vm = new Vue({
    el: "#app",
    data: {
        name: '百念成诗',
        nowTime: new Date(),
        arr: [{
            id: 1,
            name: '甄姬'
        }, {
            id: 2,
            name: "虞姬"
        },
         {
            id: 3,
            name: '阿离'
          }
     	]
    },
    /*-------------------创建期：创建vue实例--------------------*/
    //创建之前
    beforeCreate() {
        console.group('创建之前');
        console.log('el', this.$el);
        console.log("data", this.$data);
        console.log('name', this.name);
        console.groupEnd();
    },
    //创建完成：vue实例数据初始化完成，el还是undefined
    created() {
        console.group("创建完成");
        console.log('el', this.$el);
        console.log("data", this.$data);
        console.log('name', this.name);
        console.groupEnd();
    },
    /*--------------------挂载期---------------------------*/
    //挂载之前：找到了要挂载的节点，但是{{}}、指令等还没有被解析。
    beforeMount() {
        console.group("挂载之前");
        console.log('el', this.$el);
        console.log("data", this.$data);
        console.log('name', this.name);
        console.groupEnd();
    },
    //挂载完成：页面初始化完成，可以进行开启计时器、轮播图、Ajax、操作DOM节点、给window/document添加事件等。
    mounted() {
        console.group("挂载完成");
        console.log('el', this.$el);
        console.log("data", this.$data);
        console.log('name', this.name);
        console.groupEnd();

        this.timer = setInterval(() => {
            this.nowTime = new Date();
        }, 1000)
    },
    /*----------------更新期----------------*/
    //页面更新之前：不是数据变化之前，而是数据以及变了，页面重新渲染之前。
    beforeUpdate() {
        console.group("更新之前");
        console.log('el', this.$el);
        console.log("data", this.$data);
        console.log('name', this.name);
        console.groupEnd();
    },
    //页面更新完成：数据变了，页面再次渲染完成
    update() {
        console.group("更新完成");
        console.log('el', this.$el);
        console.log("data", this.$data);
        console.log('name', this.name);
        console.groupEnd();
    },
    /*-----------------销毁期--------------*/
    //卸载之前：善后工作。清除定时器、轮播图、window/document置空等
    beforeDestroy() {
        console.log("销毁之前");
        console.log(this.name);
        window.onscroll = null;
    },
    //卸载完成
    destroyed() {
        console.log("销毁完成");
        console.log(this.name);
    }
})
//点击按钮销毁vue实例，$destory()方法可以触发beforeDestory函数
document.querySelector('button').onclick = function () {
    vm.$destroy()
}
//点击挂载到指定标签，$mount()方法可以触发beforeMount函数
document.querySelectorAll('button')[1].onclick = function(){
    vm.$mount("#app")
}

```

```html
<div id="app">
    <input type="text" v-model='name'>
    <br>
    <h3>{{name}}</h3>
    <p>当前时间：{{nowTime.toLocaleTimeString()}}</p>
    <ul>
        <li v-for='item in arr' :key='item.id'>{{item.name}}</li>
    </ul>
    <button>销毁vue实例</button>
    <button>挂载vue实例</button>
</div>

```

