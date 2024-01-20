---
title: JavaScript-流程控制语句
tags: JavaScript
categories: JavaScript
img: /image/2.jpg
cover: false
summary: 第三章：JavaScript流程控制语句
abbrlink: 1f2d
date: 2020-08-23 18:57:46
---

# JavaScript学习笔记



## 第三章 流程控制语句

JavaScript中的流程控制语句和其他程序设计语言基本时一样的，主要分为：

- 顺序结构：即按顺序执行代码；
- 条件选择结构（分支语句）：包括if-else以及switch
- 循环结构：包括for循环、while、do-while，for-in；
- 其他语句：break、continue

### 1、分支语句

- if

  语法：if（条件）{条件成立执行的代码}

- if-else

  语法：if（条件）{条件成立执行的代码}else{条件不成立执行的代码}

- if-else嵌套

  语法：if（条件）{条件成立执行的代码}else if（条件）{条件成立执行的代码}else if（条件）{条件成立执行的代码}else{以上条件都不成立执行的代码}

  当然，最后一个else分支可以省略。

- switch

  语法：

  ```js
  var a = n;
  switch (a){
      case 1:
          ...;
          break;
      case 2:
          ...;
          break;
      case 3:
          ...;
          break;
      default:
          ...;
          break;
  };
  ```

  - switch语句中比较的是全等（===）。

  - switch只能使用break，不能使用continue，当匹配到其中一个值时，后面的匹配都不再进行。

  - default中的break可以省略。

> - if能实现的switch不一定能实现，switch能实现的if一定能实现。
> - 当条件是一个范围或值是一个Boolean值的时候用if。
> - 当条件是固定的值或字符时用switch。

### 2、循环语句

循环三要素：初始化循环变量、跳出循环的条件、更新循环变量

- for循环

  语法：

  ```js
  for(1.初始化循环变量;2.判断条件;4.更新循环变量){
  	3.循环体;
  }
  //执行的顺序为1->2->3->4->2->3->4(更新循环变量为++i除外)
  ```

  当然，for循环可以嵌套使用，两层嵌套中，外循环执行一次，内循环遍历所有。

- for-in

  for-in循环是专门为循环对象设置的（数组也可以，但不常用），因为对象没有长度没有顺序，所以不能使用for循环。

  ```js
  var obj = {
      name:'张三',
      age:21,
      sex:'man'
  };
  for(var key in obj){
  	console.log(key);//name age sex
      console.log(obj[key]);//张三 21 man
  };
  //由于这里是将对象中的属性赋值给了for-in中的key变量，所以操作属性时用方括号。
  ```

- while

  语法

  ```js
  var a = 1;//初始化循环变量
  while(a < 10){//循环结束条件
  	console.log(a);
      a++;//更新循环变量
  };
  ```

- do-while

  do-while循环先执行后判断。

  ```js
  var a = 1;
  do{
  	console.log(a);
      a++;
  }while(a < 10);
  ```

### 3、break与continue

break与continue的区别是：break结束循环，之后的循环不执行。continue是结束本次循环，直接执行下一次循环。

