---
title: hexo搭建博客教程
tags: hexo
categories: 博客教程
img: /image/50.jpg
top: true
summary: 个人搭建博客的总结收录
abbrlink: 1cf0
date: 2020-10-30 14:08:28
---

# hexo搭建博客教程(matery主题)

### **写在前面**

这篇教程是我搭建个人博客的路程经历和经验总结，matery主题挺多人用，网上也有各种详细教程，没时间我就不出详细教程了，这里给出其他大佬的教程链接，还有我自己总结的经验。本教程中搭建完博客之后的其他优化可以根据自己的需求选择配置即可，没有顺序。

如果你有前端基础，在个性化定制博客时可能有些效果不满意，可以自己在控制台修改调试，然后在相应的文件中修改。仔细观察框架目录和文件内容，基本都能知道对应的css和js在哪，比如我自己的博客中就修改了挺多样式（不得不说`!important`用起来真方便）。如果修改自己不熟悉的文件还是建议先备份一下，防止意外。

如果在某些配置中出错，百度找不到解决方案的话，可以去主题Github的issues中，有些主题的问题会在里面有讨论，或许会给你帮助。

### hexo博客基础搭建

#### 视频教程

我是看b站羊哥（ [CodeSheep](https://space.bilibili.com/384068749) ）的教程入坑的，视频地址：https://www.bilibili.com/video/BV1Yb411a7ty

这篇虽然他是用苹果系统操作的，但是windows系统操作起来几乎是一样，新人小白可以注意看弹幕，不同的地方弹幕都有提示。

#### hexo官网

根据视频操作完了之后，个人博客其实就搭建完成了，后面可以根据个人喜好在hexo官网选择博客主题，hexo官网也有一些教程，如果能看懂也可以跟着官网教程进行配置。hexo官网：https://hexo.io/

安装好主题之后就再根据主题官网进行主题的配置，如果主题官网看不懂可以直接百度去搜主题配置教程。这里建议小白选择比较大众的主题，不然之后配置出错了在网上都找不到解决方法，很多人用的主题，功能相对也多，教程也多。我选择的主题是matery。

### Matery主题配置

matery主题教程较多，主题[官网](https://github.com/blinkfox/hexo-theme-matery)对于一些基本配置已经介绍地挺详细了。

该主题的作者是[闪烁之狐](http://blinkfox.com/)，里面也有主题配置教程。

我参考的主要是 [Yafine](https://yafine-blog.cn)和[张小菜苔](https://zhangxiaocai.cn/ )两位大佬的。

两位大佬的教程很全面，我就不重复罗列，如果上面找不到想要的配置，下面的教程也可以参考。

- 超逸の博客 ：https://blog.csdn.net/weixin_42429718/article/details/105723193

- 这么多年的技术：https://chen-shang.github.io/2019/08/15/ji-zhu-zong-jie/hexo/hexo-theme-matery-zhu-ti-you-hua/

- cungudafa：https://cungudafa.blog.csdn.net/article/details/106278206

### 其他配置

这里收录一些网上讲解不太详细的教程。

#### 添加看板娘

Live2D Widget：https://github.com/stevenjoezhang/live2d-widget

这个看板娘应该是我能找到的最灵性的看板娘了。

最简单的使用方法（matery主题），在主题配置目录的`hexo-theme-matery\layout\layout.ejs`底部</body>之前加如下代码即可：

```js
<script src="https://cdn.jsdelivr.net/gh/stevenjoezhang/live2d-widget@latest/autoload.js"></script>
```

如果你懂前端的话，可以修改看板娘位置，可以在主题目录的`hexo-theme-matery\layout\_partial\head.ejs`中用`!important`强制修改位置，代码我就不解释了，我的写法是：

```css
<style>
#waifu{
    left: auto !important;
    right: 140px !important;
}
</style>
```

如果你是大佬，也可以直接按照Live2D Widget的官方教程按自己的想法改。

#### 代码块方括号不转义问题

这个在matery主题的issues中有讨论，之前大部分的解决办法是hexo降级，但是我降级之后也没解决这个问题，于是我在issues中看到xmuli给的方法：升级到Hexo 5.1.1。升级教程他也给了。

该issues地址：https://github.com/blinkfox/hexo-theme-matery/issues/503

#### 切换页面音乐播放器重新加载问题

很多教程都是用pjax解决，但我看了下感觉挺复杂的，有空再去研究，如果你会用pjax就可以使用它解决，不会的话可以用下面的方法凑合一下。

我的解决方案是将主页跳转文章页的跳转方式改为`_blank`(在新标签页打开)。具体改动方法是：在主题目录的hexo-theme-matery\layout\index.ejs文件中，找到三个a标签(大概在60、95、113行)，给他们各添加一个`target="_blank"`属性。

```html
<a href="<%- url_for(post.path) %>" target="_blank">
    
<a href="<%- url_for(category.path) %>" class="post-category" target="_blank">
    
<a href="<%- url_for(tag.path) %>" target="_blank">
```

文章推荐卡片同理，在`hexo-theme-matery\layout\_widget\recommend.ejs`中修改，头部导航在`hexo-theme-matery\layout\_partial\navigation.ejs`中修改，logo跳转在`hexo-theme-matery\layout\_partial\header.ejs`中修改。

这种方法的缺点是在主页进文章详情页/标签/分类页会打开新的页面，只要主页不关，音乐就一直在。

### 将博客部署到个人服务器

前提是要有自己的服务器和域名，并且要在工信部备案。

网上有挺多教程，但是有的教程没有将git更换版本，导致我一直配置不成功。

下面两个教程是我试过没出错的，这里教程使用的服务器是centos，我的服务器是腾讯云centos7.6。

- 不使用宝塔面板：https://blog.csdn.net/weixin_45682081/article/details/105278898

- 使用宝塔面板：https://zhuanlan.zhihu.com/p/128649492

cdn加速这里就不给教程，上面的大佬有各种教程。我用的是腾讯云免费送的cdn流量包，像腾讯云/阿里云它们官网都有各自的教程，仔细看文档就可以。





如果还有其他补充，我会在该教程的基础上更新。