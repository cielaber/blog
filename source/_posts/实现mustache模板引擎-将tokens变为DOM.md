---
title: 简单实现mustache模板引擎-将tokens变为dom
tags: mustache
categories: Vue源码解析
img: /image/33.jpg
summary: Vue源码解析之mustache模板引擎-将tokens变为dom
abbrlink: 51d7
date: 2021-02-06 22:57:37
---

# 简单实现mustache模板引擎之将tokens变为dom

上一篇文章讲述了将模板字符串变为tokens，这里记录将tokens变为dom，本篇结束也就完成了mustache的基本功能，也能够理解mustache的核心原理。

### 完整目录

{% asset_img 1.png %}

renderTemplate.js该函数将tokens变为dom，因为存在嵌套tokens，需要进行递归处理，为了方便将该部分提取到parseArray.js中。

由于数据中存在对象嵌套，所以会出现{{a.b.c}}这种连续点操作符的情况，会出现问题，lookup.js就是解决该问题。

### renderTemplate.js

让tokens变为dom字符串，通过遍历tokens判断每个token的类型，不同类型做不同的处理，最终会拼接成一个字符串。

```js
import lookup from "./lookup.js"
import parseArray from "./parseArray.js"
/*
    该函数让tokens变为dom字符串
*/
export default function renderTemplate(tokens,data){
    //结果字符串
    var resultStr = "";
    //遍历tokens
    for(let i= 0;i<tokens.length;i++){
        let token = tokens[i];
        //判断类型
        if(token[0]== "text"){
            resultStr += token[1];
        }else if(token[0] == 'name'){
            // resultStr +=data[token[1]];
            //这里不能直接用以上方式拼接，因为如果出现连续点（如a.b.c）的形式，是无法通过[]得出结果的，所以这里使用了单独的lookup函数进行处理
            resultStr +=lookup(data,token[1]);
        }else if(token[0] == "#"){
            resultStr += parseArray(token,data);
        }
    }
    return resultStr;
}
```

### parseArray.js

由于存在数据嵌套，该函数处理嵌套token，并且递归调用上面的renderTemplate.js。

```js
import lookup from "./lookup.js"
import renderTemplate from "./renderTemplate";

/*
    该函数处理数组，结合renderTemplate实现递归
    这里的参数是token(tokens中的一条)，而不是tokens！
*/
export default function parseArray(token,data){
    //在整体数组中找到需要使用的部分
    var v = lookup(data,token[1]);
    //结果字符串
    var resultString = "";
    //根据数据长度决定遍历次数,因为每一条数据都需要被生成dom
    for(let i = 0;i<v.length;i++){
        //一维数组中点符号代表每个数据本身，为了适应一维和多维数据，需要将数据进行处理成为一个新对象传入renderTemplate中
        resultString +=renderTemplate(token[2],{
            ...v[i],
            '.':v[i]
        });
    };
    return resultString;
};
```

### lookup.js

解决数据中连续点操作符的问题，该函数处理逻辑是，如果有点操作符则将其拆分，一步一步往里查找，直到找到最终值。

```js
/*
    在dataObj对象中，寻找用连续点符号的keyName属性
    例如：
    var dataObj = {a:{b:{c:10}}}
    那么lookup(dataObj,"a.b.c")为100
*/

export default function lookup(dataObj, keyName) {
    //寻找keyName中有没有点符号,或者不是点符号本身
    if(keyName.indexOf(".") != -1 && keyName != '.'){
        //如果有点符号则将其以点符号进行拆分
        var keys = keyName.split(".");
        //设置临时遍历，用于存储向里查找的每一层
        var temp = dataObj;
        //层层往里寻找
        for(let i = 0;i<keys.length;i++){
            temp  = temp[keys[i]];
        }
        return temp;
    }

    //没有点符号，直接返回属性值
    return dataObj[keyName];
}
```

### index.js

```js
import parseTemplateToTokens from "./parseTemplateToTokens.js"
import renderTemplate from "./renderTemplate.js"

//全局提供myTemplate
window.myTemplate = {
    //渲染方法
    render(templateStr,data){
        //调用parseTemplateToTokens函数，让模板字符串能够变为tokens数组
        var tokens = parseTemplateToTokens(templateStr);
        //调用renderTemplate让tokens变为dom字符串
        var domStr = renderTemplate(tokens,data);

        return domStr;
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
    <div class="con"></div>
    
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
        var data = {
            students:[
                {'name':'张三','hobbies':["音乐","舞蹈","逍遥法外"]},
                {'name':'老王','hobbies':["太极","广场舞"]},
                {'name':'小明','hobbies':["篮球","游泳","摄影"]}
            ]
        };

        //调用render
        var domStr = myTemplate.render(templateStr,data);
        //渲染dom树
        var con = document.getElementsByClassName("con")[0];
        con.innerHTML = domStr;
    </script>
</body>
</html>
```

结果如图：

{% asset_img 2.png %}



总结：该模板引擎并不是mustache官方源码，只是模仿源码，实现了最基本的功能，能够帮助深入理解vue的底层原理。