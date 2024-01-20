---
title: 包与NPM
tags: [Node.js,NPM]
categories: Node开发
cover: false
img: /image/15.jpg
summary: 包与NPM
abbrlink: 9c2a
date: 2020-09-13 21:48:03
---

# Node开发学习笔记

## 包与NPM

### NPM概述

**模块**：按照CommonJS规范写的js文件。

**包**：包含js文件和其他附带信息的整体。

**npm**：包的管理工具。

- 根据包的名字下载并安装（npmjs.com）。
- 解决了包之间的依赖关系。



### 常用命令

#### 安装包

```shell
npm install <Module Name>
#简写
npm i <Module Name>
```

如果不指定包名，就会在项目的package.json中寻找相关依赖包并下载。

#### 卸载包

```shell
npm uninstall <Module Name>
```

卸载全局安装的包需要加上*`-g`*。

#### 更新包

```shell
npm update <Module Name>
```

#### 搜索包

```shell
npm search <Module Name>
```

#### 查看包

```shell
#查看当前目录下已经安装的包
npm list
#查看全局安装的包
npm list -g
#查看某个包的版本号
npm list <Module Name>
```

#### 查看npm版本

```shell
npm -v
```



### 创建模块

package.json记录npm对包管理的信息。

- **name** - 包名。
- **version** - 包的版本号。
- **description** - 包的描述。
- **homepage** - 包的官网 url 。
- **author** - 包的作者姓名。
- **contributors** - 包的其他贡献者姓名。
- **dependencies** - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
- **devDependencies**-开发依赖包列表，将安装包放在c盘/usr/local下或者node的安装目录。
- **repository** - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
- **main** - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
- **keywords** - 关键字

```shell
#输入npm init之后，根据自己情况输入包的信息，输入完成后会生成package.json文件。
npm init
#然后使用以下命令在npm资源库中注册用户
npm adduser
#然后使用以下命令来发布模块，模块发布之后就可以使用npm下载使用了
npm publish
```



### 局部安装/本地安装

将安装包放在./node_modules下（运行npm命令时所在的目录），如果没有该目录，会在当前执行npm命令的目录下生成node_modules目录。

可以通过require()用来引入本地安装的包。

```shell
# 本地安装express
npm install express
```



### 全局安装

将安装包放在c盘/usr/local下或者node的安装目录。

可以直接在命令行里使用。

```shell
# 全局安装express
npm install express -g
```



### 淘宝镜像cnpm

使用淘宝镜像进行安装npm包，淘宝npm镜像是一个完整的npmjs.org镜像，目前同步频率为10分钟与官网同步一次。

#### 方法一：临时使用

```shell
npm --registry https://registry.npm.taobao.org install <Module Name>
```

#### 方法二：永久使用

将配置文件下载源改为淘宝镜像，然后就可以使用cnpm或npm命令安装。

```shell
npm config set registry https://registry.npm.taobao.org
```

可以用*`npm config get registry`*验证配置是否成功。

#### 方法三：不改下载源镜像，用cnpm。（不推荐使用）

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

然后就可以使用cnpm代替npm。

#### 恢复npm使用

```shell
npm config set registry https://registry.npmjs.org
```



### 依赖和开发依赖

**开发依赖**（devDependencies）：像less这种只有在开发的时候用到的，在项目运行的时候是不需要的，就是开发依赖。在安装开发依赖时，需要加上*`--save-dev`*，可以简写成*`-D`*。

**依赖**（dependencies）：像jQuery这种不管是运行期还是开发期都需要的，就属于依赖，需要包含在项目中。在安装依赖包时，需要加上*`--save`*，可以简写成*`-S`*。

