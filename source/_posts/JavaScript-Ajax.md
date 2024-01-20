---
title: JavaScript-Ajax
tags:
  - JavaScript
  - Ajax
  - JSON
categories: JavaScript
img: /image/24.jpg
summary: Ajax请求、JSON数据格式
abbrlink: d0ec
date: 2020-11-20 17:05:13
---

# JavaScript学习笔记

## AJAX

### 1、传统方法请求服务器

传统的web交互是用户触发一个http请求服务，服务器收到之后做出响应，并且返回一个新的的页面，每当服务器处理客户端提交的请求时，客户端处于空闲等待。并且哪怕只是与服务端进行一个简单的交互或请求一个简单的数据，都要返回一个完整的HTML页面，这造成了大量的带宽浪费，以及应用响应非常慢。

### 2、什么是AJAX

AJAX即“Asynchronous JavaScript And XML”（异步JavaScript和XML），是指一种创建交互式网页应用的网页开发技术，用于创建快速动态网页的技术，通过在后台与服务器进行少量数据交换，是网页实现异步更新，意味着可以不重新加载网页而对网页的部分进行更新。

> 异步：浏览器在请求数据的过程中，不会一直等待，这段时间还可以做其他的操作。

> 同步：浏览器在请求数据的的过程中，会一直等待事件的响应，直到返回结果才会执行其他的操作。

> 尽管X在AJAX中代表XML，但是由于JSON的许多优势，比如更加轻量以及作为JavaScript的一部分，目前JSON比XML更加普遍。JSON和XML都被用于在ajax模型中打包信息。

### 3、ajax的使用步骤

- 创建XMLHTTPRequest对象

  ```js
  var ajax = new XMLHttpRequest();
  ```

- 使用open()方法设置和服务器的交互信息

> open()有三个参数：请求类型、url、受否异步(布尔类型)。

> 请求类型又简单分为两种：get、post。get方式通过将数据放在url地址中传到服务器。post方式通过send()方法传送数据，因此相对安全。

> url为向服务端请求数据的地址，get方法传输时，发送到服务端的数据放在url中以？与前面的地址分隔。而post方法则不用。

> 第三个参数为Boolean类型，决定请求方式是否为异步，true为异步方式。

- 设置发送的数据，开始和服务端交互

  通过send()方法向服务端发送数据，通过post类型传输时，将数据以参数形式传通过send方法发送数据。

- 注册事件

  ```js
  ajax.onreadystatechange = function(){//通过onreadystatechange事件监听前后台数据交互阶段
      if(ajax.readyState === 4){//判断ajax的状态，当为4的时候代表ajax请求成功。
          if(ajax.statuus === 2){//判断服务器的状态，200表示请求成功。
              console.log(ajax.responseText);//通过ajax.responseText获取返回的数据
          }
      }
  }
  ```

  readyState属性可以获取ajax的状态，由0-4表示，分别表示如下：

  - 0：（初始化）还没调用open方法
  - 1：（载入）已调用send()方法，正在发送请求
  - 2：（载入完成）send()方法完成，已收到全部响应内容
  - 3：（解析）正在解析响应内容
  - 4：（完成）响应内容解析完成，可以在客户端调用了

  status属性可以获取服务器返回的网络状态码，一共5个系列：

  - 1xx系列：消息，指定客户端响应的某些动作这一类状态码代表请求已被接受，需要继续处理，这类响应是临时响应，只包含状态行和某些可选的响应头信息，以空行结束。由于HTTP/1.0协议中没有定义任何1xx状态码，所以除非在某些实验条件下，服务器禁止向此类客户端发送响应。
  - 2xx系列：成功，代表请求已经成功被服务器接收、理解并接受。常见的有200、201。
  - 3xx系列：重定向，代表需要客户端采取进一步的操作才能完成请求，这些状态码用来重定向，后续的请求地址（重定向目标）在本次响应的location中指明。
  - 4xx系列：请求错误，代表客户端看起来可能发生了错误，妨碍了服务器处理。如401、404。
  - 5xx系列：服务器错误，代表服务器在处理请求的过程中有错误或异常状态发生，也有可能是服务器意识到当前的软硬件资源无法完成对请求的处理。

#### 3-1、get方式请求

```js
var ajax = new XMLHttpRequest();
ajax.open('get','./data/data.json?username=张三&&age=18',true);//get方式数据通过url发送到服务端
ajax.send();
ajax.onreadystatechange = function(){
    if(ajax.readyState === 4){
    	if(ajax.status === 200){
    		console.log(ajax.responseText);
		}
	}
}
```

#### 3-2、post方式请求

```js
var ajax = new XMLHttpRequest();
ajax.open('post','./data/data.json',true);
ajax.setRequestHeader('content-type','application/x-www-form-urlencoded');//post方式需要设置请求头(第二个参数是form表单enctype属性的一个值)
ajax.send('username=张三&&age=18');//post方式数据通过send()方法发送到服务端
ajax.onreadystatechange = function(){
    if(ajax.readyState === 4){
        if(ajax.status === 200){
			console.log(ajax.responseText);
        }
    }
}
```

### 4、封装ajax请求

```js
//method：请求类型
//url：请求地址
//data：传输到服务端的数据
//callback：回调函数
function ajax(method, url, data, callback) {
    var ajax = new XMLHttpRequest(); //创建ajax对象
    if (method.toLowerCase() === 'get') { //get方法
        if (data) { //判断数据是否为空
            url += '?' + data; //将数据加到url后面
        }
        ajax.open(method, url, true); //设置请求信息
        ajax.send(); //发送数据
    } else if (method.toLowerCase() === 'post') { //post方法
        ajax.open(method, url, true);
        ajax.setRequestHeader('content-type', 'application/x-www-form-urlencoded'); //设置post方法设置请求头
        ajax.send(data);
    }
    ajax.onreadystatechange = function () {
        if (ajax.readyState === 4) { //判断ajax状态
            if (ajax.status === 200) { //判断服务器状态
                callback && callback(ajax.responseText); //将数据放入回调函数
            } else {
                throw new Error('请求失败，错误码为：' + ajax.status); //请求失败则抛出错误
            }
        }
    }
}
```

### 5、数据解析

JSON的常规用途是同web服务器进行数据传输，在从web服务器接收数据时，数据永远是字符串形式，需要转成数组或对象形式来使用，一般用JSON.parse()方法。

JSON.parse()方法将一个JSON字符串转换为对象，对于衍生自数组的JSON使用JSON.prase()，则会返回JavaScript数组，而不是对象。

对象形式的json数据

```json
{
    "jData":[
        {
            "username": "张三",
            "age":18
        },
        {
            "username":"李四",
            "age":20
        }
    ]
}
```

```js
<script src="./js/ajax.js"></script> //调用前面封装好的ajax
<script>
    ajax('get', './data/data.json', 'name=ajax&&method=get', function (response) {
    console.log(JSON.parse(response) instanceof Object);//true
});
</script>
//instanceof用于测试左边的对象是否是右边对象的实例
```

数组形式的json数据

```json
[
    {
        "username": "张三",
        "age":18
    },
    {
        "username":"李四",
        "age":20
    }
]
```

```js
<script src="./js/ajax.js"></script> //调用前面封装好的ajax
<script>
    ajax('get', './data/data.json', 'name=ajax&&method=get', function (response) {
    console.log(JSON.parse(response) instanceof Array);//true
});
</script>
```

> eval()方法也能将字符串数组转成真正的数组，但是该函数官方说明为 eval() 函数可计算某个字符串，并执行其中的的 JavaScript 代码。该方法只接受原始字符串作为参数，如果 string 参数不是原始字符串，那么该方法将不作任何改变地返回。因此请不要为 eval() 函数传递 String 对象来作为参数。该方法的用途并不是用来将字符串转数组，不推荐在这使用。
>
> ```js
> eval("x=10;y=20;document.write(x*y)"); //200
> console.log(eval("2+2")); //4
> var x = 10
> console.log(eval(x + 17)); //27
> var str = '[1,2,3]';
> console.log(eval(str),eval(str) instanceof Array);//(3) [1, 2, 3]  true
> ```
>
> 虽然该函数功能强大，但也不经常使用。

ajax请求数据实例

```js
<script src="./js/ajax.js"></script> //调用前面封装好的ajax
var ul = document.getElementsByTagName('ul')[0];//获取ul
ajax('post','./data/data.json','',addLi);//ajax请求json中的数据
function addLi(data){
    var d = JSON.parse(data);//json数据转成数组形式，原json数据为衍生数组的json，可以直接用JSON.parse()转成数组类型
    d.forEach(function(item){
        ul.innerHTML += `<li>${item.username}</li>`;//将数据在页面中渲染
    })
}
```



