---
title: 简单实现mustache模板引擎-实现tokens
tags: mustache
categories: Vue源码解析
img: /image/32.jpg
summary: Vue源码解析之mustache模板引擎-将模板字符串编译成tokens
abbrlink: af35
date: 2021-02-05 11:15:02
---

# 简单实现mustache模板引擎之将模板字符串编译成tokens

这里使用了webpack进行项目开发，所以在写源码之前先说明如何建项目。

### 准备工作

新建项目

```bash
npm init
```

安装依赖包

```bash
npm i -D webpack@4 webpack-cli@3 webpack-dev-server@3
```

书写webpack配置文件

在项目目录下新建webpack.config.js文件，配置内容如下，可自行修改：

```js
const path = require("path")

module.exports ={
    //模式：开发模式
    mode:"development",
    //入口文件
    entry:"./src/index.js",
    //出口 打包到什么文件
    output:{
        filename:"bundle.js"
    },
    //配置一下webpack-dev-server
    devServer:{
       //静态文件根目录
       contentBase:path.join(__dirname,"www"),
       //是否压缩
       compress:false,
       //端口号
       port:8080,
       //虚拟打包的路径，bundle.js文件没有真正地生成
       publicPath:"/xuni/", 
    }
}
```

上面的入口文件为src/index.js，静态文件为www，所以在项目根目录中新建src和www目录，并在src中新建index.js文件，在www中新建index.html文件，并在index.html中引入上面设置的虚拟打包文件。

```html
<script src="/xuni/bundle.js"></script>
```

### 目录

{% asset_img 1.png %}

Scanner.js为实现对模板字符串中的字符进行逐个扫描；

parseTemplateToTokens.js实现将模板字符串变为数组，但这时候还不是真正的tokens；

nestTokens.js将parseTemplateToTokens.js中生成的数组变为真正的tokens。

模板字符串生成的dom字符串，在渲染成dom树时，空字符串会影响布局，handleSpace.js对空字符串进行了处理，它有没有并不影响其他的功能。

其他文件在下一章中进行介绍，与该章功能无关。

### 实现Scanner字符扫描

通过实别双大括号来实别数据，所以需要对字符进行扫描，这里专门写一个类来处理。

```js
/*
    扫描器类
*/
export default class Scanner{
    constructor(templateStr){
        //将模板字符串放在实例上
        this.templateStr = templateStr;
        //指针
        this.pos =0 ;
        //尾巴（未扫描的内容），开始时后就是模板字符串原文
        this.tail = templateStr;
    }

    //跳过指定内容
    scan(tag){
        //尾巴的第一个字符就是tag
        if(this.tail.indexOf(tag)==0){
            //tag有多长，比如{{长度是2，就让指针后移多少位
            this.pos+=tag.length;
            //改变尾巴为从当前指针这个字符开始，到最后的全部字符
            this.tail = this.templateStr.substring(this.pos);
        }
    }

    //让指针进行扫描，直到遇见指定内容结束，并且能够返回结束之前扫描的内容
    scanUtil(stopTag){
        //记录一下执行该方法时候的pos值
        const pos_back = this.pos;
        //当尾巴没有到头且尾巴的开头不是stopTag的时候，说明还没有扫描到stopTag
        while(!this.eos() && this.tail.indexOf(stopTag) !=0){
            this.pos++;
            //改变尾巴未从当前指针这个字符开始，到最后的全部字符
            this.tail = this.templateStr.substring(this.pos);
        }

        //返回已经被扫描过的内容
        return this.templateStr.substring(pos_back,this.pos);
    }

    //指针是否到头，返回布尔值 end of string
    eos(){
        return this.pos >=this.templateStr.length;
    }
}
```

### 将模板字符串变为临时数组tempTokens

也就是parseTemplateToTokens.js的内容，这里需要用到上面的扫描器类对模板字符串进行扫描，然后根据字符不同把数据变量和普通字符区分开。

```js
import Scanner from "./Scanner.js"
import nestTokens from "./nestTokens.js"
import handleSpace from "./handleSpace.js"
/*
    将模板字符串变为tempTokens数组
*/
export default function parseTemplateToTokens(templateStr) {
    var tempTokens = [];
    //创建扫描器
    var scanner = new Scanner(templateStr);
    var words;
    //让扫描器工作
    while (!scanner.eos()) { //没有到末尾
        //收集开始{{之前的文字
        words = scanner.scanUtil("{{");
        if (words != '') {
            //处理空字符串
            var _words = handleSpace(words);
            //存起来
            tempTokens.push(["text", _words]);
        }
        //跳过{{
        scanner.scan("{{");
        //开始收集{{}}中间的内容
        words = scanner.scanUtil("}}");
        if (words != '') {
            //此时words就是{{}}中间的内容
            if (words[0] == "#") { //数据的开始标签
                //跳过#，从下标为1的项开始存
                tempTokens.push(["#", words.substring(1)]);
            } else if (words[0] == "/") { //数据的结束标签
                tempTokens.push(["/", words.substring(1)]);
            } else { //普通内容
                tempTokens.push(["name", words]);
            }
        }
        //跳过}}
        scanner.scan("}}");
    }
    //调用nestTokens()将tempTokens变为最终tokens
    var nestedTokens = nestTokens(tempTokens);
    return nestedTokens;
}
```

### handleSpace.js处理空字符串

```js
/*
    该方法用来处理空格，为了避免空格对dom树渲染造成布局影响。
    标签外的空字符串需要去除，标签<>内的空字符不需要去除。
*/
export default function handleSpace(words){
    //isNull标记是否是标签标签内部
    let isNull = false;
    //结果字符串
    var _words = '';
    //循环遍历判断
    for(let i = 0;i<words.length;i++){
        //判断是否在标签里
        if(words[i] == "<"){
            isNull = true;
        }else if(words[i] == '>'){
            isNull = false;
        }

        //如果这项不是空格，拼接上
        if(!/\s/.test(words[i])){
            _words += words[i];
        }else{//是空格
            //且在标签内部，则拼接该空格
            if(isNull){
                _words +=" "; 
            }
        }
    }
    return _words;
}
```

如果是但这里生成的tempTokens并不是最后的tokens，只是对双大括号进行了实别并将模板字符串中不同的内容区分开然后存入数组中。然后在最后调用了下面要讲到的nestTokens函数将tempTokens转为最终的tokens。

{% asset_img 2.png %}

这个数组中的每一项小数组都是一个token，这个概念下面要用。

### 生成真正的嵌套的tokens

nestTokens.js通过循环遍历上面生成的tempTokens每一个token，判断处理后，生成最终的tokens。

```js
/*
    该函数的功能是折叠tokens,将#和/之间的tokens整合起来，作为下标为2的项
*/
export default function nestTokens(tokens){
    //结果数组
    var nestedTokens = [];
    //这里采用栈结构，存放内嵌的的tokens
    var sections = [];
    //收集器，初始指向nestedTokens数组
    //收集器的指向会变化，当遇见#的时候，收集器会指向这个token的下标为2的新数组
    var collector = nestedTokens;

    for(let i = 0;i<tokens.length;i++){
        let token = tokens[i];

        switch(token[0]){
            case "#":
                //收集器中放入token
                collector.push(token);
                //入栈
                sections.push(token);
                //收集器换人，给token添加下标为2的项，并让收集器指向它
                collector = token[2] = [];
                break;
            case "/":
                //出栈
                sections.pop();
                //改变收集器为栈队尾（队尾是栈顶）那项的下标为2的数组
                //如果栈为空则指回结果数组
                collector = sections.length > 0 ? sections[sections.length - 1][2] : nestedTokens;
                break;
            default:
                collector.push(token);
                break;
        }
    }
    return nestedTokens;
}
```

这一段是比较精彩的地方，看起来就是把上面的数组变成下面的嵌套式数组，但是实现原理很精妙。

{% asset_img 3.png %}

逻辑如下：

1、遍历tempTokens，首先有一个collector指向结果tokens，遇到普通token就追加进入collector，遇到‘#’token也把该token加到结果collector，此时collector依旧是结果数组。

2、由于有先进后出的逻辑，这里设置了sections为一个栈。遇到‘#’token，该token入栈。然后collector开始指向该token的第三项（下标为2），且初始为一个空数组。

3、然后又会遇到普通token，追加进collector，这时候collector已经指向了之前遇到的‘#’token的第三项空数组了。

4、之后就会遇到需要内嵌的‘#’token，又入栈，循环第二步和三步。

5、直到进入最里层的内嵌数组后，collector指向最里层token的第三项空数组。然后把该token后的普通token都放入collector中，直到遇到‘/’token。

6、遇到‘/’token后，出栈（栈中存储的是从外到里的所有内嵌的token），每出一个，collector就重新指向栈顶的token的第三项数组。继续遍历，如果后面还有普通token，普通token继续放入collector中。

7、直到sections栈为空，代表内嵌数组处理完毕，collector最后指向结果tokens。

如果对栈结构不熟悉，可以看之前的文章：[JavaScript中数据在内存中的存储方式](https://www.eternitywith.xyz/posts/242b.html)

### 入口文件index.js

```js
import parseTemplateToTokens from "./parseTemplateToTokens.js"

//全局提供myTemplate
window.myTemplate = {
    //渲染方法
    render(templateStr,data){
        //调用parseTemplateToTokens函数，让模板字符串能够变为tokens数组
        var tokens = parseTemplateToTokens(templateStr);
        console.log(tokens);
    }
}
```

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>手写mustache源码</title>
</head>
<body>
    <script src="/xuni/bundle.js"></script>
    <script>
        //模板字符串
        var templateStr = `
        <div>
            <ol>
                {{#students}}
                <li>
                    学生{{name}}的爱好是
                    <ol>
                        {{#hobbies}}
                        <li>{{.}}</li>
                        {{/hobbies}}
                    </ol>
                </li>
                {{/students}}
            </ol>
        </div>
        `;
        var data = {};

        //调用render
        myTemplate.render(templateStr,data)
    </script>
</body>
</html>
```

