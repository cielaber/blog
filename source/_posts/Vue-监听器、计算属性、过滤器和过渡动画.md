---
title: Vue-监听器、计算属性、过滤器和过渡动画
tags: Vue
categories: 前端框架
img: /image/21.jpg
summary: 第三章：Vue监听器-计算属性-过滤器-过渡动画
abbrlink: 26f7
date: 2020-09-25 14:02:59
---

# 前端框架学习笔记

## 第三章 Vue监听器、计算属性、过滤器和过渡动画

### 监听器

Vue提供了watch选择项，这是一个更通用的方法来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有效的。

```js
new Vue({
    watch:{
        //浅监听，参数可省略
        value(newV,oldV){
            //逻辑
        }，
        //深度监听不建议使用，因为会造成页面卡顿，如果要使用的话，建议转换成简单类型使用。
        json:{
        	handler(){
    			//逻辑
			}，
        	deep:true
        }
	}
})

```

注意：优就业课本上说监听数组也需要深度监听，但是亲测，监听数组不需要深度监听，浅监听就可以，不管是一层还是嵌套多层。如下：

```html
<div id="app">
    <input type="text" v-model='arr[0]'>
    <input type="text" v-model="arr1[2][1]">
</div>
```

```js
new Vue({
    el:'#app',
    data:{
        arr:[1,2,3,4,5],
        arr1:[1,2,[3,4]]
    },
    watch:{
        arr(){
            console.log('arr改变了！')
        },
        arr1(){
            console.log("arr1改变了！")
        }
    }
})
//改变输入框的值，均能触发监听器。
```

有博主分析Vue源码得出结论，watch监听判断的是数据否是对象类型，是对象类型则必须使用深度监听，不论是一层才是多层。



### 过滤器

Vue允许自定义过滤器，可用于一些常见的文本格式化。

目的：转换数据

使用： 双括号、v-bind

```html
<!--{{ | }}-->
<div>
    {{变量 | 过滤器名称}}
</div>

<!--v-bind，该方法从2.1.0版本之后开始支持-->
<div v-bind:id="变量 | 过滤器名称"></div>
```

定义：推荐全局定义

```js
//全局定义
Vue.filter("过滤器名称",(过滤对象)=>{
    //逻辑 
    return "你要的结果"
})
```

```js
//局部定义
new Vue({
    filters:{
        //全局定义
		过滤器名称(过滤对象){
            //逻辑 
    		return "你要的结果"
        }
    }
})
```

### 计算属性

Vue提供了计算属性，它不需要在data里面定义，就能返回需要的值，并且能进行大量计算。

```js
new Vue({
    computed:{//计算属性
        a(){
            return 10
        },
        ava(){
            //运算逻辑
            var sum=0
            this.students.forEach(item=>{
                sum+=item.score
            })
            //通过return返回
            return (sum/this.students.length).toFixed(2);
        }
    }
})
```

在Vue中，计算属性可以被视为是data一样的，可以读取和设值，因此在计算属性中可以分成getter和setter，一般情况下是没有setter的，computed预设只有getter，也就是只能读取，不能改变，设值，但是可以通过依赖变量改变值。

```js
var vm = new Vue({
    el: '#app',
    data:{
        a: 3,
        b: 4
    },
    computed: { //计算属性
        c: {
            get() {
                console.log("c被获取了！");
                return Number(this.a) + Number(this.b);
            },
            set(v) {
                //设置的值
                console.log('set：' + v);
                //实际c的值
                console.log(vm.c);
            }
        }
    }
})
```

```html
<div id="app">
    <input type="text" v-model='a' placeholder="a">+
    <input type="text" v-model='b' placeholder="b">=
    <input type="text" v-model="c" placeholder="c">
</div>
```

原始状态

{% asset_img 3-1.png %}

改变a时，get()方法被调用。

{% asset_img 3-2.png %}

改变c绑定的输入框，c的值并不会被改变，只是set()方法被调用了。

{% asset_img 3-2.png %}

注意：

1、v-for和v-if不能作用在同一个标签，虽然有时候有效果，但是Vue官方声明该语法不合法。这时候可以用计算属性computed解决该方面的需求。

参考：https://cn.vuejs.org/v2/guide/list.html#v-for-%E4%B8%8E-v-if-%E4%B8%80%E5%90%8C%E4%BD%BF%E7%94%A8

2、如果数组数据改变，页面不渲染，可以采用以下方法解决。

```js
//1、this.arr.splice(下标,1,新值);
this.arr.splice(index,1,newValue)

//2、vm.$set(arr,下标,新值)
this.$set(this.arr,index,newValue)

//3、Vue.set(arr,下标,新值)
Vue.set(this.arr,index,newValue)
```

3、如果是json数据发生了改变，页面不渲染，可以用以下方法解决。

```js
//1、vm.$set
this.$set(this.json,改变的属性,newValue);
//2、Vue.set
Vue.set(this.json,改变的属性,newValue);
```



### jsonp

ajax不能跨域 get、post

jsonp可以跨域，只能以 get方式请求

jsonp跨域的要求：提供回调函数（一般是callback或cb）和数据接口的规则。

```js
var vm = new Vue({
    el: "#app",
    data: {
        value: "",
        results: ''
    },
    methods: {
        enter() {
            if (this.value != '') {
                //console.log(this.value)
                //创建script标签
                var os = document.createElement('script');
                //设置src属性
                os.src = "https://api.asilu.com/fanyi?callback=cbb&q=" + this.value;
                //将src属性添加到页面中
                document.body.appendChild(os)
            } else {
                this.value = '';
            }
        },
    }
})
//回调函数必须是全局函数
function cbb(d) {
    vm.results = d.data;
    console.log(d);
}
```



### 动画

Vue在插入、更新或者移除DOM时，提供多种不同方式的应用过渡效果，包括以下工具：

- 在CSS过渡和动画中自动应用class
- 可以配合使用第三方CSS动画库，如Animate.css
- 在过渡钩子函数中使用js直接操作DOM
- 配合使用第三方js动画库，如Velocity.js

#### 使用场景

 Vue 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡 。

- 条件渲染（v-if）
- 条件展示（v-show）
- 动态组件
- 路由

当插入或删除包含在 `transition` 组件中的元素时，Vue 将会做以下处理：

- 1-自动嗅探目标元素是否应用了 CSS 过渡或动画，如果是，在恰当的时机添加/删除 CSS 类名。
- 2-如果过渡组件提供了 [JavaScript 钩子函数](https://cn.vuejs.org/v2/guide/transitions.html#JavaScript-钩子)，这些钩子函数将在恰当的时机被调用。
- 3-如果没有找到 JavaScript 钩子并且也没有检测到 CSS 过渡/动画，DOM 操作 (插入/删除) 在下一帧中立即执行。(注意：此指浏览器逐帧动画机制，和 Vue 的 `nextTick` 概念不同)

#### 状态

 在进入/离开的过渡中，会有 6 个 class 切换：

- `v-enter`：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
- `v-enter-active`：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
- `v-enter-to`：2.1.8 版及以上定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 `v-enter` 被移除)，在过渡/动画完成之后移除。
- `v-leave`：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
- `v-leave-active`：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
- `v-leave-to`：2.1.8 版及以上定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 `v-leave` 被删除)，在过渡/动画完成之后移除。

```html
<button v-if='!isshow'>
<transition name='animate'>
    <div class='box' v-if="isshow"></div> 
</transition>
```

对于这些在过渡中切换的类名来说，如果你使用一个没有名字的 ``，则 `v-` 是这些类名的默认前缀。如果你使用了 ``（如上面取名为`‘animate’`），那么 `v-enter` 会替换为`animate-enter`。 

 `v-enter-active` 和 `v-leave-active` 可以控制进入/离开过渡的不同的缓和曲线 。

这里只简要介绍了Vue动画的部分内容，详情参考[官网](https://cn.vuejs.org/v2/guide/transitions.html)。