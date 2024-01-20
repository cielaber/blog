---
title: 原生js+jQuery实现瀑布流布局
tags:
  - JavaScript
  - jQuery
categories: 组件库
img: /image/28.jpg
summary: 通过原生js计算及jQuery辅助实现瀑布流布局
abbrlink: 5d51
date: 2021-01-22 20:20:33
---

# 前端常见效果实现

## 原生js+jQuery实现瀑布流布局

在制作相册的时候发现使用瀑布流更合适，因此研究了一番，本篇记录通过原生js计算及使用jQuery辅助实现瀑布流布局。

参考自：[3种方式实现瀑布流布局](https://blog.csdn.net/weixin_44135121/article/details/98629830)

该方法为响应式，无需固定子项宽度，子项宽度可根据窗口宽度而改变。

核心思路：通过父元素宽度及列数动态计算子项宽高，第一行排列完后每一列就有了高度，之后每增加一项就找出最小高度列，将子项放到该列中，并将该列高度增加，之后每加一个子项则重复上面的操作。

核心代码如下，为了方便使用将它封装成了一个函数，参数分别是：每个子项具有的公共类名、父元素宽度、列数、每列间距、上下间距。

注意：

- 该方法使用了jQuery，需提前引入。
- 该方法使用了定位，动态生成子项的left和top值，需给父元素相对定位，子项设置绝对定位。

```js
/*
        item:子项类名，注意要加.,如“.item”
        fatherWidth:父元素宽度
        columns:列数
        spacingRight:每列间距
        spacingTop:上下间距
    */
function waterfall(item,fatherWidth,columns,spacingRight,spacingTop){
    //确定每个子项的宽度
    var itemWidth = parseInt(fatherWidth/columns);
    //设置item的宽度
    $(item).width(itemWidth)
    //存储每一行的最小子项的高度
    var arr = [];
    $(item).each(function (i){
        // console.log(i);
        //每个子项的高度
        var height = $(this).height();
        if(i<columns){
            //第一行
            $(this).css({
                top:0,
                left:(itemWidth)*i +spacingRight*i,
            });
            //将第一行每个子项的行高存储到arr中
            arr.push(height);
        }else{//第二行及以后
            //获取现在状态下（不包含第i个的状态）最小的列的高度
            var minHeight = arr[0];//保存最小高度
            var index = 0;//保存索引
            for(var j = 0;j<arr.length;j++){
                if(minHeight >arr[j]){
                    minHeight = arr[j];
                    index =j;
                }
            }
            //设置第i个子项的位置
            $(this).css({
                top:minHeight+spacingTop,//已知的最小列高度+上下间距
                left:$(item).eq(index).css("left")//最小列最后一个子项的left值
            })
            //修改第i个子项所在列的高度
            arr[index] = minHeight +height+spacingTop;
        }
    })
}
```

如果需要做到响应式，则需要在页面尺寸改变时动态获取视口大小。

```js
//获取视口宽函数
function getClient(){
    return {
        width:window.innerWeight || document.documentElement.clientWidth || document.body.clientWidth,
        height:window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight
    }
};

//设置参数
var fatherWidth = getClient().width;
var columns = 3;
var spacingRight = 20;
var spacingTop = 10;
var item = ".item"

//初始化
window.onload = function(){
	waterfall(item,fatherWidth,columns,spacingRight,spacingTop);
}

//页面尺寸改变时触发
window.onresize = function(){
    //如果父元素宽度随窗口变化，每次变化重新获取父元素宽度
    fatherWidth = getClient().width;
    waterfall(item,fatherWidth,columns,spacingRight,spacingTop)
}
```

