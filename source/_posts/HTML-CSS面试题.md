---
title: HTML+CSS面试题
tags:
  - HTML
  - CSS
categories: 前端面试题
img: /image/5.jpg
top: true
summary: HTML+CSS面试题
abbrlink: e27a
date: 2020-08-23 19:10:44
---

# 优就业大前端第一阶段（HTML+CSS）面试题



## 第一章 HTML入门

### 1、简述<!doctype>的作用？

>`<!DOCTYPE html>` 决定浏览器渲染方式。`<!DOCTYPE html>`表示用HTML5的doctype声明文件包含HTML5标记。
>
>`<!DOCTYPE>`:告知浏览器当前的HTML或XML文档是哪一个版本。Doctype是一条声明，而不是一个标签，也可以把它叫做“文档类型声明”或者简称为“DTD”。

### 2、常见的浏览器及其内核？

>谷歌(chrome)浏览器 -- webkit --Blink

>苹果(Safari) --webkit

>IE浏览器 --trident

>欧鹏浏览器(Opera) --Presto --webkit --Blink

>火狐浏览器(Firefox) --Gecko

### 3、b和strong，i和em标签的区别？

>b、i属于修饰类标签属于物理标签，没有做到结构与样式分离。strong、em属于内容类标签属于逻辑标签，做到了结构与样式分离。

>strong和em都表示强调，strong比em语气更强烈。

>在搜索引擎优化strong和em比b和i重要的多。

### 4、谈谈对语义化的理解？

>概念：
>
>>用合理的HTML标签及其特有的属性去格式化文档内容。如标题用h1-h6、段落用p，合理地设置图片的alt属性。

>好处：
>
>> - 在没有css的情况下，页面也能呈现出很好的内容结构。  
>> - 使代码更具可读性，便于开发和后期维护。  
>> - 有利于用户体验。如title、label标签、alt属性的灵活运用。  
>> - 有利于SEO。网页和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息。爬虫依赖于标签来确定上下文和各个字段关键字的权重。



## 第二章 CSS入门

### 1、css引入方式有哪些？

>行内样式、内部样式表（内嵌式）、外部样式表（外链式）

### 2、css基本选择器有哪些？

>通配符选择器、标签选择器、类选择器、id选择器

### 3、如何合并表格单元格？

>跨行合并rowspan  

>跨列合并colspan 

>补充：边框合并border-collapse:collapse;

### 4、caption、thead、tbody、tfoot有什么用？

>这些标签能增强语义化，让表格结构更加清晰，对布局不会产生影响。



## 第三章 盒模型

### 1、常见的表单元素有哪些？

>input标签(text、password、button、radio、checkbox、submit、reset、file、image)，select标签、textarea标签、button标签

### 2、请简述一下盒模型的组成？

>content、padding、border、margin

### 3、css复合选择器有哪些？

>后代选择器、子代选择器、并集选择器、交集选择器、伪类链接选择器



## 第四章 浮动

### 1、块级标签、行内块标签、行内标签的区别？

>块级标签:div、p、h1-h6、ul、ol、li、dl、dt、dd、hr、form  
>
>> - 独占一行，上下排列。 
>> - 默认宽度占满父级，默认高度是本身内容的高度。
>> - 可以设置宽高及所有盒模型属性。

>行内标签:label、span、a、b、strong、i、em、sub、sup、del
>
>> - 默认并排，宽高由内容撑开。  
>> - 行内元素只能容纳文本或其他内联元素(行内元素)，不能镶嵌块级元素。
>> - 行标签之间有间隙、不能设置上下内外边距，可以设置左右内外边距。

>行内块标签:input、img、select、textarea、button
>
>> - 默认并排，可以设置宽高。
>> - 中间有间距，可以设置盒模型的所有属性。

### 2、浮动产生的问题？清除浮动的方案？

>产生的问题（浮动是为了实现文字环绕图片而产生）
>
>>子标签浮动后脱离文档流，导致父标签高度塌陷，会影响后续正常布局。

>清楚浮动的方法
>
>> - 给浮动的父标签固定高度（不够灵活）。
>>
>> - 给父标签加overflow属性，overflow为visible以外的其他值时（即把父标签设置成BFC）可以帮助实现。
>>
>> - 给浮动标签的最后加一个空块标签，标签本身不浮动，且添加样式clear:both;(代码冗余，不建议)
>>
>> - (推荐方法)给浮动标签的父标签添加.clearfix（不会在结构上产生代码冗余，可多次重复使用）。
>>
>> ```css
>> .clearfix::after{
>>    content:"";
>>    clear:both;
>>    display:block;
>> }
>> .clearfix{
>>    *zoom:1; //为了兼容ie7及以下。
>> }
>> ```



### 3、伪元素如何创建？

>在元素的开头（第一个子元素之前）添加
>
>```css
>.box::before{
>content:'';
>...
>}
>```

>在元素的末尾（第一个子元素之后）添加
>
>```css
>.box::after{
>content:'';
>...
>}
>```


## 第五章 定位

### 1、如何实现盒子的水平垂直居中？

>文本/行内块水平居中：
>
>>父标签text-align:center;

>文本/行内块垂直居中：
>
>> - line-height:height;
>> - 父标签padding-top=padding-bottom  

>块元素水平居中：
>
>```css
>margin: 0 auto;  /*需要固定宽度*/
>```

>块元素垂直居中：
>
>> - 调节margin、padding

>> - 绝对定位（父元素尺寸变化，子元素尺寸固定）
>>
>> ```css
>>  .parent{
>>    position:relative;
>>  }
>>  .child{
>>    width:200px;
>>    height:200px;
>>    position:absolute;
>>    left:50%；
>>    top：50%；
>>    /* 再移动子元素宽度和高度的一半 */
>>    margin-left:-100px;
>>    margin-top:-100px;
>>  }
>> ```

>> - 绝对定位（父元素和子元素的尺寸都是可变的）
>>
>> ```css
>>  .parent{
>>    position:relative;
>>  }
>>  .child{
>>    position:absolute;
>>    top:0;
>>    bottom:0;
>>    margin:auto;
>>    /*如果还想实现水平居,可以加上:*/
>>    right:0;
>>    left:0;
>>  }
>> ```

>> - 采用Flex弹性布局 （该方法适用于行内、行内块、块级标签） 
>>   父元素设置display:flex;   
>>   align-items: center;（子元素对齐方式）
>>   水平居中还需要加上：justify-content:center;

>> - 采用表格布局（该方法适用于行内、行内块、块级标签,但不常用）  
>>   设置父标签display:table;  
>>   子标签display:table-cell;vertical:middle;  
>>   如下：
>>
>> ```css
>> <div class="wrapper">
>>    <p>我要垂直居中啊a我要垂直居中啊a我要垂直居中啊a我要垂直居中啊a</p>
>> </div>
>> 
>> .wrapper{
>>   display:table;
>> }
>> .wrapper p{
>>  display:table-cell;/*该属性使该元素成为单元格，宽高无效，margin无效，宽度由内容撑开*/
>>  vertical-align:middle;
>>  /*如果还想实现水平居中,加上:*/
>>  text-align:center;
>> }
>> ```



## 第六章 布局技巧

### 1、图片下方空白间隙如何解决？

>水平空白
>
>> - 图片浮动(不建议)  
>> - 父元素设置font-size:0;

>垂直间隙
>
>> - 图片浮动(不建议)  
>> - 父元素设置font-size:0或line-height:0;  
>> - 图片设置vertical-align:middle; //任意不为baseline的值都可以，因为基线对齐会使基线以下的部分空出来。
>> - 图片设置display:block;

### 2、请简述等高布局、圣杯布局、双飞翼布局的实现原理。

>等高布局
>
>> - 伪等高
>>
>> > - 边框模拟  
>> >   原理：因为元素边框和元素高度始终是相同高度，用元素的边框颜色来伪装左右两个兄弟元素的背景色。然后将左右两个透明背景的元素使用absolute覆盖在中间元素的左右边框上，实现视觉上的等高效果。注意:左右两侧元素的内容高度不能大于中间元素内容高度，否则无法撑开容器高度。
>> > - 内外边距相消  
>> >   因为背景是在padding区域显示的，设置一个大数值的padding-bottom，再设置相同数值的负的margin-bottom，使背景色铺满元素区域，又符合元素的盒模型的计算公式（实际宽高=内容+内边距+边框+外边距），实现视觉上的等高效果。注意：父元素要设置overflow:hidden。如果页面中使用a锚点跳转时，将会隐藏部分文字信息。如果页面中的背景图片定位到底部，将会看不到背景图片。

>>- 真等高
>>
>>> - 背景嵌套  
>>>   利用内容撑开父元素的特点，给每一列添加一个背景容器，并且相互嵌套，等高元素在最里层的背景容器中且同级。背景容器使用相对定位往外移，相应的等高元素也利用margin移到对应的容器里，只要有一个等高元素的高度发生变化，就会撑开套在最里层的背景容器，其他套在外层的背景容器高度也随之变化。（讲义p59页）
>>> - 绝对定位  
>>>   所有子元素设置绝对定位，各元素通过绝对定位移到父元素的左中右位置，子元素由于脱离了文档流不占位，无法撑开父元素高度，需要设置父元素的固定高度，并且所有子元素的top:0;bottom:0;(这两个设置才是使子元素高度与父元素同高的关键。)，就可以使得所有子元素的高度都和父元素的高度相同，实现等高效果。
>>> - 表格布局  
>>>   table元素中的table-cell元素默认是等高的，将父元素设置成display:table;等高子元素设置成display:table-cell;即可。
>>> - Flex弹性盒布局  
>>>   flex弹性盒中的项目如果未设置高度或高度为auto，则交叉轴方向默认占满容器的高度，再配合justify-content属性调整项目位置，即可实现等高效果。
>>> - Grid网格布局  
>>>   grid网格布局在不设置行数（默认一行）时，设置grid-auto-flow:column;(先列后行)，每列项目默认占满一列的高度即父容器的高度，即可实现等高布局。

>圣杯布局、双飞翼布局
>
>>共同点:  
>>中间栏放最前面，左右标签固定宽度，中间标签宽度百分百，三个标签左浮动，左边元素margin:-100%移到第一行最左边，右边元素利用marigin:-自身宽度  移到第一行最右边。
>>圣杯解决方案：三个标签的父容器左右分别设置等于左右元素宽度的padding，再利用相对定位移动左右元素。

>>双飞翼解决方案：给中间栏加一个内层inner，并给inner添加等于左右两栏宽度的外边距（其实内边距也可以）。（不能直接给中间栏设置内外边距，因为中间栏设置了width:100%,直接给中间栏加内外边距会导致界面宽度加大。）



## 第七章 CSS技巧

### 1、简述css精灵图原理，及优缺点？

>原理:  
>css sprites，通常被解释为"CSS图像拼合"或"CSS贴图定位"，就是把网页中一些背景图片整合到一张图片文件中，再利用background-image、background-repeat、background-position等属性的组合进行背景定位，background-position用数字能精确地定位出来背景图片的位置，一般适用于小图标，不适合大背景。 

> 优点：  
>
> > - 减少网页的http请求，从而大大提高网页的性能；
> > - 减少图片命名上的困扰；
> > - 更换风格方便。

>缺点：  
>
>> - 必须要限定容器的大小符合背景图元素，需要计算；
>> - 需要测量背景图所在的位置。

### 2、简述BFC规则及解决的问题？

>规则：  
>
>> - 内部的标签会在垂直方向上一个接一个地放置。
>> - 垂直方向上的距离由margin决定，属于同一个BFC的两个相邻标签的margin会发生重叠。
>> - 每个标签的左外边距与包含块的左边相接触，即使浮动标签也是如此。
>> - BFC的区域不会与float的标签域重叠。
>> - 计算BFC的高度时，浮动子标签也参与计算。
>> - BFC就是页面上的一个隔离的独立容器，容器里的子标签不会影响到外面标签，反之亦然。

>解决的问题:
>
>> - 外边距塌陷（只有垂直方向会发生塌陷，水平方向不会。）  
>>   由于BFC是一个独立的容器里面的子标签不会影响外面的标签，外面的标签也不会影响BFC内的标签。所以兄弟盒子之间垂直外边距塌陷可以给其中一个盒子套一个父盒子，把父盒子设为BFC即可。父子垂直外边距塌陷，可以把父元素设置成BFC，父级成为了一个独立区域，子标签的边距就无法塌陷到外面。为了不影响布局一般用overflow:hidden;解决外边距塌陷的问题。
>> - 两栏或三栏布局（这里的情况是将左边盒子或左右两边的盒子写在前面，且必须将自适应的盒子写在后面，与圣杯、双飞翼布局不同。）
>>
>> > - 将左边固定宽度的盒子左浮动，右边盒子设置overflow:hidden;即可将右边盒子设置成BFC，不会覆盖左边元素，并且右边盒子不设宽度就可以实现自适应两栏布局。
>> > - 将左右两边固定宽度的盒子左右浮动，中间盒子不设置宽度且设置overflow:hidden;即可实现三栏自适应布局。 （中间栏写在最后的情况下，还可以中间栏不设宽度，且设置左右外边距等于左右两栏的宽度，由于左右两栏浮动不占位，中间栏可以与之同行，也实现了自适应三栏布局。） 

### 3、文本溢出显示省略号如何实现？

>单行文本溢出
>
>```css
>p{
>width:200px;
>overflow:hidden;
>text-overflow:ellipsis;/*显示省略号*/
>white-space:nowrap;/*文本不换行*/
>}
>```

>多行文本溢出
>
>> - 利用wenkit的css拓展属性（只有webkit内核才有用）
>>
>> ```css
>> p{
>>    width:200px;
>>    overflow:hidden;
>>    text-overflow:ellipsis;
>>    display:-webkit-box;/*将对象设置成弹性伸缩盒子模型显示*/
>>    -webkit-line-clamp:2;/*用来限制在一个块元素显示的文本的行数，这是一个不规范属性，它没有出现在css规范草案中。*/
>>    -webkit-box-orient:vertical;/*设置伸缩盒子对象的子元素的排列方式*/
>> }
>> 
>> ```

>>- 利用伪元素模拟溢出显示省略号的效果（兼容性比较好）  
>>  实现步骤:将height设置为line-height的整数倍，防止超出的文字露出。给伪元素p::after添加渐变背景，避免文字之显示一半。
>>
>>```css
>>p{
>> position:relative;
>> width:200px;
>> line-height:30px;
>> height:60px;
>> overflow:hidden;
>>}
>>p::after{
>> position:absolute;
>> right:0;
>> bottom:0;
>> content:'...';
>> display:block;
>> padding-left:48px;
>> background:-webkit-linear-gradient(left,transparent 0,transparent 60%,#fff 60%);
>> background:-ms-linear-gradient(left,transparent 0,transparent 60%,#fff 60%);
>> background:-o-linear-gradient(left,transparent 0,transparent 60%,#fff 60%);
>> background:linear-gradient(left,transparent 0,transparent 60%,#fff 60%);
>>}
>>
>>```


## 第八章 项目切图及规范

### 1、为什么要初始化css样式？哪些样式需要初始化？

>因为浏览器兼容问题，不同浏览器对某些标签默认的解析是不同的，如果没有对css初始化，往往会出现不同浏览器之间的页面显示差异，为了避免这种差异需要使用样式重置。  
>ul、li、dv等自带的边距，a链接的自带样式，列表自带的列表样式，em、i自带的斜体样式等。

### 2、display:none和visibility:hidden的区别？

>display:none是将元素的显示设为无，即在网页中看不见也不占位。visibility:hidden则是将元素隐藏不可见，但是元素本身宽高都存在并且占位。

### 3、你能想出几种方法让元素在页面中消失？

>- display:none;
>- visibility:hidden;
>- position:absolute或fixed,用z-index覆盖，数值越小，越在后面，默认为0；
>- overflow:hidden;将要隐藏的元素移除父元素的范围。
>- opacity:0;将元素设置成透明。
>- 将font-size,line-height,width,height设置为0;
>- transform:translate(-100%,-100%);
>- position: absolute;top: -9999px;left: -9999px;将元素移出可视区。

### 4、标签应该如何合理嵌套？

>- 行内标签里面不能放块级标签（a里面可以放块标签）。
>- 块级标签里面可以放块级标签、行内标签、行内块标签（特殊：p、h1-h6里面不能再放块级）。
>- ul、ol和li事固定嵌套，ul、ol的直接子元素必须是li。dl和dt、dd是固定嵌套，tr和td、th是固定嵌套。
>- p标签不允许嵌套p标签，a标签不允许嵌套a标签和其他交互性元素如button。
>- 尽可能地控制元素嵌套层级，不合理地嵌套会影响页面性能。

### 5、简述网页中常见图片格式及特点？

| 格式 |                优点                |                 缺点                 |           使用场景           |
| :--: | :--------------------------------: | :----------------------------------: | :--------------------------: |
| jpg  |          色彩丰富，文件小          | 有损压缩，反复保存图片质量会明显下降 | 色彩丰富地图片/渐变图像/照片 |
| gif  |   文件小，支持动画，无兼容性问题   |         只支持支持256种颜色          |   色彩简单地logo/icon/动图   |
| png  | 无损压缩、支持透明、简单图片尺寸小 |   不支持动画，色彩丰富的图片尺寸大   |       logo/icon/透明图       |



## 第九章 PC端项目-兼容问题

### 1、说说你了解的浏览器兼容问题有哪些？

>- 图片间隙问题
>
>>- 水平空白
>>
>>>- 图片浮动
>>>- 图片父元素设置font-size:0;
>>
>>- 垂直空白
>>
>>>- 图片设置vertical-align:middle;
>>>- 图片设置display:block;
>>>- 图片父元素设置font-size:0;line-height:0;
>>>- 图片浮动

>- ie中图片边框问题  
>  ie中图片放在a标签中显示边框。解决方法：图片设置border:none;

>- ie8中背景复合属性写法问题  
>  background:url("images/bg.jpg")no-repeat;在标准浏览器中均能正常显示背景图片，但是在ie中显示异常。解决方法：在url和no-repeat之间加上空格。

>- 其他ie低版本兼容性问题
>
>>- 在ie6及更早版本浏览器中，定义小高度的容器。
>>
>>```css
>>#test{
>> overflow:hidden;
>> height:1px;
>> font-size:0;
>> line-height:0;
>>}
>>
>>```

>>- ie6及更早版本浏览器下，浮动时产生双倍边距的bug。  
>>  解决方案:针对ie6设置该标签的display属性为inline即可。
>>
>>```css
>>#test{
>> float:left;
>> _display:inline;
>>}
>>
>>```

>>- ie7及更早版本浏览器下，子标签相对定位时，父标签overflow属性的auto|hidden失效的问题。  
>>  解决方案：给父标签也设置相对定位position:relative;

>>- ie7及以下块转行内块不在一行显示的问题，解决方案如下:
>>
>>```css
>>div{
>> display:inline-block;
>> *display:inline;
>> *zoom:1;/*设置或检索对象的缩放比例，1或100%时表示不缩放，更大更小表示放大缩小，不支持负数，兼容性不好。*/
>>}
>>
>>```

>>- ie7及以下浏览器中，当li中出现浮动子元素时，li之间产生的空白间隙。  
>>  解决方案：将垂直对齐方式vertical设置为top/middle/bottom。

### 2、什么是CSSHack？

> 由于不同厂商的浏览器或同一厂商不同版本的浏览器，对css的解析认识不完全一样，因此会导致生成的页面效果不一样，这时候就需要针对不同的浏览器去写不同的css，让它能同时兼容不同的浏览器，能在不同的浏览器中也能得到我们想要的页面效果。



## 第十章 PC端项目-测试检查

### 1、在项目中你是如何做图片优化的？

>- 使用base64编码代替图片。
>
>>Base64编码是一种图片处理格式，通过特定的算法将图片编码成一长串字符串，在页面上显示时，可以用该字符串来代替图片的url属性。
>>
>>优点：
>>
>>> - 减少HTTP网络请求。网页上的图片资源如果采用http形式的url的话都会额外发送一次请求，网页发送的http请求次数越多，会造成页面加载速度越慢。采用base64格式的编码，将图片转化为字符串后，图片文件会随着html元素一并加载。这样就可以减少http请求的次数，对于网页优化时一种较好的手段。
>>>
>>> - 采用base64编码的图片是随着页面一起加载的，不会造成跨域请求的问题。
>>>
>>> - 没有图片，更新要重新上传，不会造成清理图片缓存的问题。
>>
>>缺点：
>>
>>>- 浏览器支持问题，IE6/IE7均不支持base64编码。
>>>- 增加了css文件的尺寸。将图片转化为base64格式编码，生成的字符串往往会大于图片源文件的大小。如果将其写在一个css文件中，这样的一个css文件大小会剧增，造成代码不可读，还会造成请求传输的数据量增加。
>>>- 造成数据库数据量巨大。将base64编码的图片存入数据库中会造成数据库数据量增大，这样的效果还不如将图片存至图片服务器而只在数据库中存入url字段。

>- 使用精灵图，减少页面请求次数。
>- 在保证图片不失真的情况下合适地压缩图片。 https://tinify.cn/ 
>- 图片延迟加载(懒加载)：延迟加载图片或者符合某些条件时加载某些图片。
>- 图片预加载：在网页全部加载之前，提前加载图片，当用户需要查看时直接从本地缓存中渲染，以提供给用户更好的体验，减少等待的时间。
>- 使用css、svg、canvas、或iconfont代替图片。
>- 根据不同的终端需求加载对应尺寸的图片。
>- 根据图片特性和需求选择合适格式的图片。



## 第十一章 HTML5基础

### 1、HTML5有哪些新特性？

>语义化标签、多媒体（音视频）、智能表单、canvas画布、web存储、地理定位...

### 2、如何处理HTML5新标签的浏览器兼容问题？

>- 通过js创建出新增的标签，再在css中将新增标签转为块级，才能使宽高生效。
>
>```javascript
><script>
> document.creatElement('header');
> ...
></script>
>
>```
>
>```css
><style>
> header,footer{
>     display:block;
> }
></style>
>
>```

>- 使用封装好的插件html5shiv.js解决兼容性问题。
>
>```javascript
><!--[if lt ie 9]>
> <script type="text/javascript" src="./js/html5shiv.min.js"></script>
><![endif]-->
>
>```


## 第十二章 CSS3基础

### 1、css3有哪些新特性？

>新增选择器、文本样式、边框属性（阴影、圆角）、多背景、变形、渐变、动画、过渡、多列布局、弹性盒布局、用户界面等等。

### 2、css3新增选择器有哪些？

>属性选择器、结构伪类选择器、状态伪类选择器



## 第十三章 CSS3过渡、变换与动画

### 1、css3中过渡和动画的区别和各自的适用场景？

>- 过渡
>
>>- 不能自动运行，需要伪类或者js触发。
>>- 只有两种状态。
>>- 触发一次运行一次。

>- 动画
>
>>- 可以自动运行。
>>- 可以定义多种状态。
>>- 可以多次或无限次运行。

>- 适用场景
>
>>- 如果要灵活定制多个帧以及循环，用animation。
>>- 如果要简单的from to效果，用transition。
>>- animation与js的交互不是很紧密，如果要使用js灵活设定动画属性，用transition。



## 第十四章 弹性盒子和预处理

### 1、解释一下CSS3的flexbox(弹性盒布局模型),以及适用场景？

>弹性盒模型布局是css3中的新的布局方式，把父元素设置成弹性盒容器，可以更加方便地去规定子元素地排列方式、对齐方式、剩余空间。对居中对齐，规则布局（如两栏、三栏布局）非常高效。

### 2、什么是less？less有什么好处？

>定义：less是css的一种预处理语言，提供了一套新的语法，类似于编程语言，简化css代码，并且提供了一个编译器，用来把写好的less文件编译成css文件，在编译之后才能被浏览器识别使用。

>优点：使css代码更简洁，适应性强，可读性好，有利于代码的维护。



## 第十五章 移动端项目-布局方案

### 1、常见的移动端布局解决方案有哪些？原理如何？

>rem布局
>
>>rem是指相对于根元素的字体大小的单位。  

>>原理：利用rem作为布局单位，不同设备访问页面时，通过js脚本动态计算出一个最新的font-size值设置给html标签，从而达到界面整体缩放效果。

>vw+rem布局
>
>>vw是一个相对视口宽度的一个单位，视口被分为100vw。 

>>假设750px的设备中，font-size=100px，则设置html{font-size：13.33333vm;}，然后可以直接以rem为单位布局，不需借助插件。



## 第十六章 移动端项目-拓展知识

### 1、如何处理小于12px的字体？

>将容器元素转成块级或者行内块，利用-webkit-transform:scale()属性将容器缩小，再用-webkit-transform-orign-X:left center;将变形原点靠右居中，可以使字体左对齐并居中。



## 第十八章 响应式布局

### 1、什么是响应式？

>响应式布局是在不同的设备上网页可以呈现不同的布局。一套代码可以兼容pc端、移动端。(不适应复杂网站，适合一些简单的展示网站，如企业官网、后台管理系统。)

### 2、响应式项目中常用到哪些核心技术？

>媒体查询
>
>```css
>@media screen and (max-width:750px){
>···
>}
>
>```

>百分比布局
>
>>宽度不固定，可以使用百分比，内外边距也可以使用百分比。

>弹性盒布局
>
>```css
>container{
>display:flex;
>flex-direction:row/row-reverse/column/colunm-reverse;
>justify-content:flex-start/flex-end/center/space-between/space-around/space-enenly;
>align-items:stretch/flex-start/flex-end/center/baseline;
>flex-wrap:nowrap/wrap/wrap-reverse;/*wrap-reverse表示主轴方向向下的情况下，换行且第一行在下方*/
>align-content:stretch/flex-start/flex-end/center/space-around/space-between/space-evenly;/*表示多行项目在交叉轴上的对齐，可参考justify-content*/
>}
>items{
>order:0;/*定义项目的排列顺序，数值越小，排列越靠前，默认为0*/
>flex-grow:0;/*定义项目的放大比例，默认为0，即如果存在剩余空间也不放大。*/
>flex-shrink:1;/*定义项目缩小比例，默认为1，即如果空间不足，该项目将缩小。*/
>align-self:auto/stretch/center/flex-start/flex-end/baseline/inherit;/*定义该子项目单独在交叉轴上的对齐方式，与align-items属性作用相同，但该属性用于项目。*/
>}
>
>```

>响应式图片
>
>```css
>img{
>width:100%;
>height:auto;/*保证图片保持原始的宽高比。*/
>}
>/*为了防止图片宽度过大而使图片失真，需要用max-width属性设置图片最大宽度不超过图片原始宽度。*/
>img{
>max-width:100%;
>height:auto;
>}
>
>```

>响应式字体
>
>>通过rem/vw布局搭配媒体查询实现响应式字体。

