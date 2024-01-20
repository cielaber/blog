---
title: React入门
tags: React
categories: 前端框架
img: /image/22.jpg
summary: 第十四章：React入门
abbrlink: e289
date: 2020-10-19 23:35:05
---

# 前端框架学习笔记

## 第十四章 React入门

### React介绍

#### 介绍

React是Facebook内部的一个JavaScript类库。

React用于创建Web用户交互界面。

React不是一个完整的MVC框架，只负责MVC中的V(View)视图层，甚至React不认可MVC开发模式。

React的设计思想极其独特，属于革命性创新，性能出众，代码逻辑简单。

React引入了虚拟DOM(Virtual DOM)的机制。

React引入了组件化的思想，一切皆组件。

React使用Facebook专门为其开发的一套语法糖--jsx。

#### 特点

- 虚拟DOM

- 组件化

- JSX语法

#### 优点

- 组件化开发
- 引入虚拟DOM，性能好，响应速度快。
- JSX语法
- 单向数据绑定
- 跨浏览器兼容
- 完善的生态圈和活跃的社区

#### 缺点

- 不是完整的MVC框架
- React本身不适合大型项目开发，但是如果要开发大型项目，可以借助react-router-dom、redux实现。



### 脚手架安装

```shell
#安装脚手架
cnpm i create-react-app -g

#创建项目
creeate-react-app reactdemo

#进入项目
cd reactdemo

#启动项目
npm start
```



### 目录

```markdown
-public	服务器目录

​	favicon.ico 	页面图标

​	index.html	页面主入口

​	manifest.json	页面配置文件

-src 	项目源码

​	index.js 	入口文件

​	App.js 	根组件

​	App.test.js	根组件测试

​	index.css	全局css样式

​	App.css	根组件样式

​	serviceWorker.js	离线访问服务

index.js与App.js是最重要的文件，其他文件均可根据需求可有可无。
```



#### index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
// import './index.css';
import App from './App';
// import * as serviceWorker from './serviceWorker';

ReactDOM.render(
  // React.StrictMode为严格模式
  //<React.StrictMode>
    <App />,
  //</React.StrictMode>,
  document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
// serviceWorker.unregister();
```

#### App.js

```js
import React from 'react';
// import logo from './logo.svg';
import './App.css';

var name = '张三'

function App() {
  //return 里返回一段嵌套js的html代码
  return (
  <div className="App">{name}</div>
  );
}

export default App;
```



### 语法

React中组件通过function App(){}中return返回，采用的是jsx语法。

()中书写html代码，{}中书写js代码，两者可以相互嵌套。

#### 注释

```jsx
{/*一段注释*/}
```

由于()中是html语法，所以不能直接使用`//`注释。也不能使用`{//}`注释。



### 数据绑定

#### 非表单元素

```jsx
var name ="百度"

<div>name:{name}</div>
```

#### 媒体元素/属性绑定

```jsx
var imgUrl = ""

<a href={imgUrl} title={name}></a>
<img src={imgUrl} alt="" />
```



### 条件渲染

采用三元运算符进行条件渲染

```jsx
var isShow = true;

{isShow ? <div>开启</div> : <div>结束</div>}
```



### 列表渲染/循环渲染

```jsx
var list = [
  {name:"张三",age:18},
  {name:"李四",age:20},
  {name:"王五",age:17}
]

<ol>
    {list.map((item)=>{
        return (<li key={item.name}>{item.name + ':'+ item.age}</li>)
    })}
</ol>
```



### 动态样式

React中的样式类名用className属性而不是class属性。

#### 动态类名

```jsx
<div className={/*通过js逻辑选择类名*/}></div>
```

#### 动态行间样式

```jsx
<div style={{background:"orange",color:"#fff"}}></div>
```

这里出现了双大括号，外面的大括号表示js语法，里面的括号表示一个json格式。

这里不能使用以前的写法直接在style属性写样式，比如以下写法是错误的：

```jsx
<div style="background:'orange',color:'#fff'"></div>
```



### 事件

#### 箭头函数绑定事件

```jsx
<button onClick={()=>fun()}>
```

#### bind绑定

```jsx
<button onClick={this.fun.bind(this)}>
```

通过bind绑定，bind第一个参数是this。

#### 事件传参

箭头函数绑定事件传参直接传

```jsx
<button onClick={()=>fun(a,b)}>
```

bind绑定事件第一个参数是this，想传递的参数从第二个开始。

```jsx
<button onClick={this.fun.bind(this,a,b)}>
```

#### 事件对象event

箭头函数绑定事件的event，通过箭头函数的参数传递event，而fun()中接收的位置可以自定义，如下第二个参数是event。

```jsx
<button onClick={(e)=>fun(a,e,b)}>
```

bind绑定事件的event最后一个参数就是event，但是省略不写，接收的时候是最后一个参数接收。

```jsx
<button onClick={this.fun.bind(this,a,b)}>
    
function fun(a,b,e){}
```

#### 阻止默认事件

jsx接近原生js，不像Vue有修饰符，React中阻止默认事件需要通过原生js的事件对象实现。

```jsx
<button onClick={(e)=>fun(e)}>
    
function fun(e){
	e.preventDefault()
}
```

注意：React中不能用return false来阻止默认事件。

#### 阻止事件冒泡

也是采用原生js的事件对象实现。

```jsx
<button onClick={(e)=>fun(e)}>
    
function fun(e){
	e.stopPropagation()
}
```

#### 捕获事件

在事件名后加上Capture就是捕获事件。

```jsx
<button onClickCapture={()=>fun()}>
```

#### 弹窗案例

```jsx
import React ,{Component} from 'react'
import '../App.css';
class ClassComponent extends Component{

state={
    hideTag :false,
}
hide(){
  this.setState({
    hideTag : false
  });
}
stopPro(e){
    e.stopPropagation()
}
show(){
    this.setState({
        hideTag:true
    })

}
hide2(e){
    //实现vue中的.self
    // if(e.target.className ==="mask"){
    //     this.hide();
    // }

    //短路实现
    e.target.className === 'mask' && this.hide()
}

rightClick(e){
    e.preventDefault()
}

//有生命周期
render(){
    return (
        // 阻止传播实现弹窗消失
        //    <div>
        //        <button onClick={this.show.bind(this)}>mask</button>
        //        {this.state.hideTag ? ( 
        //        <div className="mask" onClick={this.hide.bind(this)}>
        //         <div className="content" onClick={this.stopPro.bind(this)}>
        //             <p>你确定要删除吗？</p>
        //             <div className="btn">
        //                 <button onClick={()=>this.hide()} >取消</button>
        //                 <button onClick={(e)=>this.hide(e)} >确定</button>
        //             </div>
        //         </div>
        //   </div>): null}
        //    </div>

        //判断是否点击自身实现弹窗消失
        <div onContextMenu={(e)=>this.rightClick(e)}>
            <button onClick={this.show.bind(this)}>mask</button>
            {this.state.hideTag ? ( 
                <div className="mask" onClick={this.hide2.bind(this)}>
                    <div className="content" >
                        <p>你确定要删除吗？</p>
                        <div className="btn">
                            <button onClick={()=>this.hide()} >取消</button>
                            <button onClick={(e)=>this.hide(e)} >确定</button>
                        </div>
                    </div>
                </div>): null}
        </div>
    )
}
}

export default ClassComponent
```



### 组件

与Vue一样，React中每个组件，但不同的是React中所有组件都是局部组件，要用需要引入，没有全局组件。

#### 命名

- 组件名字首字母必须大写
- 且与vue不同的是，React中组件名字中间出现大写字母是可以的，不需要改成‘-小’写的形式。

#### 定义组件

##### 类定义

```jsx
import React ,{Component} from 'react'
class ClassComponent extends Component{
    name="张三";
//可以自定函数绑定事件
changeName(name){
    this.name = name;
}
//有生命周期
render(){
    return (
        <div>
        <span>这是一个类定义组件</span>
        </div>
    )
}
}

export default ClassComponent
```

#### 函数定义

函数定义组件相对来说更简单，但是无法绑定事件，也没有生命周期。

```jsx
import React from "react"
function FunComponent(){
	return (
        <div>
            <span>这是一个函数定义组件</span>
        </div>
    )
}

export default FunComponent
```

#### 引入使用

```jsx
import ClassComponent from "./components/ClassComponent"
import FunComponent from "./components/FunComponent"

(<div>
        <ClassComponent></ClassComponent>
        <FunComponent></FunComponent>
</div>)
```

#### 函数定义组件与类定义组件的区别

- 类定义组件有生命周期，而函数定义的组件没有。
- 类定义组件接收父组件传递的值的this.props，函数定义的组件接收父组件传递的值是props。
- 类定义组件有state，函数定义的组件没有，state中数据改变会引起页面的重新渲染。



### 组件通信

#### 父组件向子组件传值

父组件通过自定义属性给子组件传值，函数定义的子组件通过函数参数接收，类定义的组件通过this.props接收。

```jsx
{/*父组件传值给函数定义的子组件*/}
<FunComponent data={this.state.data} change={(e)=>this.hang(e)}></FunComponent>

{/*父组件传值给类定义的子组件*/}
<ClassCom data={this.state.data} change={(e)=>this.hang(e)}></ClassCom>
```

```jsx
{/*类子组件接收父组件传的值*/}
<p>{this.props.data}</p>

{/*函数子组件接收父组件传的值*/}
function FunComponent(props){
	return (
    <p>{props.data}</p> 
    )
}
```

#### 子组件向父组件传值

父组件通过给子组件绑定自定义属性，值是一个函数，子组件通过this.props或者props触发。

```jsx
{/*函数定义的子组件 函数接收一个参数props*/}
<button onClick={()=>props.change(childData)}>传值</button>

{/*类定义的子组件*/}
<button onClick={()=>this.props.change(this.state.childDta)}>传值</button>
```

```jsx
<FunComponent data={this.state.data} change={(e)=>this.hang(e)}></FunComponent>

<ClassCom data={this.state.data} change={(e)=>this.hang(e)}></ClassCom>
```



