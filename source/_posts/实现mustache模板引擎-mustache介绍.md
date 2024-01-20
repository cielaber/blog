---
title: 简单实现mustache模板引擎-mustache介绍
tags: mustache
categories: Vue源码解析
img: /image/31.jpg
summary: Vue源码解析之mustanche模板引擎-mustache介绍
abbrlink: '8602'
date: 2021-02-05 11:13:50
---

# 简单实现mustache模板引擎之mustache介绍

最近开始学习vue源码，会慢慢记录一些笔记。首先开始学的是mustache模板引擎，因为vue进行数据绑定使用的是mustache语法。

### 什么是模板引擎？ 

模板引擎（这里特指用于Web开发的模板引擎）是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的HTML文档。 

简单来说，在前端领域它是将数据变为视图的最优解决方案。

在模板引擎之前，也有许多的其他解决方案，内容挺长，我直接放其他博主的文章，有兴趣可以自行学习：https://blog.csdn.net/weixin_43690348/article/details/113103697

### mustache实现原理

一句概括就是：将模板字符串转换成tokens（一个js嵌套数组）后，将tokens结合数据再转换成dom树。

如下图：

{% asset_img 1.png %}

将模板字符串编译成tokens如下：

{% asset_img 2.png %}

再将tokens转为dom字符串，通过innerHTML赋给dom节点。

下面两篇文章分别详细介绍两个主要功能：

[简单实现mustache模板引擎之将模板字符串编译成tokens](https://www.eternitywith.xyz/posts/af35.html)

[简单实现mustache模板引擎之将tokens变为dom](https://www.eternitywith.xyz/posts/51d7.html)

