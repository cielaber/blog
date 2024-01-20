---
title: Node.js文件系统
tags: Node.js
categories: Node开发
cover: false
img: /image/16.jpg
summary: 第五章：Node.js文件系统
abbrlink: ea0
date: 2020-09-13 22:27:01
---

# Node开发学习笔记

## 第五章 Node.js文件系统

### 文件系统简介

Node.js提供一组类似UNIX标准的文件操作API，Node导入文件系统模块（fs）语法如下：

```js
var fs = require('fs')
```

所有的文件系统操作都具有同步的、回调的、以及基于promise的形式。

### 获取文件信息

异步获取文件信息语法：

```js
fs.stat(path,(err,stats))
```

path：文件路径，如果是相对路径，参照路径不是当前文件，而是控制台node的启动路径。

callback：回调函数，带有两个参数（err，stats），err是异常，stats是fs.Stats对象，包含了文件的信息。

stats具有如下两个常用方法：

- stats.isFile()：判断是否是文件，是则返回true。
- stats.isDirectory()：判断是否为目录，是则返回true。

### 读取文件

异步读取文件语法：

```js
fs.readFile(path,code,(err,data))
```

同样也有path和callback两个参数，但是它还有另一个参数code为编码格式。

与fs.stats()不同的是，fs.readFile()回调函数中有两个参数（err,data），err是异常，data是读取到的数据。

同步读取文件语法：

```js
var data = fs.readFileSync(path,code)
```

### 写入文件

异步覆盖写入文件语法：

```js
fs.writeFile(path,data,(err)=>{})
```

同步覆盖写入文件语法：

```js
fs.writeFileSync(path,data)
```

异步追加写入文件语法：

```js
fs.appendFlie(path,data,(err)=>{})
```

同步追加写入文件语法：

```js
fs.appendFileSync(path,data)
```

注意：

- 写入文件时，当文件不存在时会创建文件，当path是带有文件夹的文件名时，文件不存在也能创建，但如果文件夹也不存在时，就会报错。

- 异步是通过err的形参接收异常，而同步如果发生错误则会直接报错。

### 修改文件

异步修改文件语法：

```js
fs.rename(oldName,newName,(err)=>{})
```

除了修改文件，该方法还能修改路径，如果路径不存在则会报错。

同步修改文件语法：

```js
fs.renameSync(oldName,newName)
```

### 删除文件

异步删除文件语法：

```js
fs.unlink(path,(err)=>{})
```

同步删除文件语法：

```js
fs.unlinkSync(path)
```

### 创建目录

```js
fs.mkdir(path,(err)=>{})
```

如果path中被创建目录的父级目录存在才能创建，如果包含不存在的父级目录，则无法创建。

### 读取目录

```js
fs.readdir(path,(err,files)=>{})
```

回调函数中files参数是被读取目录下的文件/目录组成的数组，且只能读取一层。

### 删除目录

```js
fs.rmdir(path,(err)=>{})
```

删除一个一个空目录。

以上方法创建和删除目录无法递归创建或者删除。

```js
//同步方法递归删除
const fs = require('fs');
const Path = require('path');

function deleteDir(path){
	var stats = fs.statSync(path);
    //如果是文件，直接删除
    if(stats.isFile()){
		fs.unlinkSync(path);
    }else{
        //如果是文件夹，先读取所有子文件
        var files = fs.readdirSync(path);
        for(let file of files){
            //用path包提供的join()方法把要删除的目录及子文件拼成一个完整的路径
			deleteDir(Path.join(path,file))
        }
        //子文件删除完之后删除自己
        fs.rmdirSync(path);
    }
}
```

```js
//异步方法
const fs = require('fs').promises;
const Path = require('path');

async function deleteDir(path){
	var stats = await fs.stat(path);
    if(stats.isFile()){
        await fs.unlink(path);  
    }else{
        var files = await fs.readdir(path);
        for(let file of files){
            await deleteDir(Path.join(path,file));
        }
        await fs.rmdir(path);
    }
}
```

Nodejs官方文档中，fs.redir()和fs.redirSync()方法给出了可以递归删除的参数，将{recursive:true}作为第二个参数，但是文档中说明了递归删除是实验的。



### 文件流

#### 读取流

```js
const fs = require('fs')
//创建一个读取流
var readStream = fs.createReadStream(path);
//设置读取流的编码
readStream.setEncoding('utf8');
//绑定一个事件，每次读取一块内容都会触发data事件
//在回调函数中，能够设置一个形参chunk，接收每次读取的那一小块内容
var data = '';
readStream.on('data',(chunk)=>{
    data += chunk;
})
//end事件代表所有内容读取完成
readStream.on('end',function(){
    console.log(data)
})
//error事件代表程序出现错误
readStream.on('error',(err)=>{
    console.log(err.stack)
})
```

#### 写入流

```js
var fs = require('fs');
//创建写入流
var writeStream = fs.createWriteStream(path);
//写入数据
writeStream.write(data,'utf8');
//标记文件末尾
writeStream.end();
//处理流事件
writeStream.on('finish',()=>{});
writeStream.on('error',(err)=>{})
```

### 管道流

管道提供了一个输出流到输入流的机制。通常我们从一个流中获取数据传递到另一个流中。

```js
var fa = require('fs');
//创建读取流
var readStream = fs.createReadStream(path);
//创建写入流
var writeStream = fs.createWriteStream(path);
//管道操作读写
readStream.pipe(writeStream);
```

