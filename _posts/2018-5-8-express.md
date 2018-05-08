---
layout:     post
title: express框架搭建和基本结构
subtitle:  Windows下web应用开发
date:       2018-5-8
author:     "tomylee"
header-img: "img/t015c96c4462fd21afb.jpg"
catalog: true
tags:
    - web开发
---

### 一、什么是express?
Express 是一个简洁而灵活的 node.js Web应用框架, 提供了一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具。

使用 Express 可以快速地搭建一个完整功能的网站。

Express 框架核心特性：

```
可以设置中间件来响应 HTTP 请求。

定义了路由表用于执行不同的 HTTP 请求动作。

可以通过向模板传递参数来动态渲染 HTML 页面。
```

### 二、环境准备

#### 1.node.js

express 是 Node.js 上最流行的 Web 开发框架，首先需要下载node.js，可以到[Node.js官网](https://nodejs.org/en/)直接下载，下载好之后正常安装就可以了。

安装好后我们可以在命令行用`node -v`查看是否安装成功，出现版本号则表示安装成功：

![node.js安装](https://img-blog.csdn.net/20180508095023294)

Linux可以使用源码安装：

```
curl -O https://nodejs.org/dist/v6.9.1/node-v6.9.1.tar.gz
tar -xzvf node-v6.9.1.tar.gz
cd node-v6.9.1
./configure
make
make install
```

#### 2.npm

npm是随同Node.js一起安装的包管理工具，同样也可以用`npm -v`来查看版本；
npm可以用来安装卸载一些api包，例如`npm install (要安装的包)`，这样安装的包默认是本地安装，如果要全局安装可以在后面加上`-g`或者`--global`,详细用法可参考[npm使用小结](http://www.cnblogs.com/jingmoxukong/p/6228358.html)。


![npm版本](https://img-blog.csdn.net/20180508095634510)

由于npm的源有时候下载很慢，我们可以切换到国内的一下镜像，比如淘宝镜像等。具体教程以及npm的其他内容参考[NPM使用介绍](http://www.runoob.com/nodejs/nodejs-npm.html)。

#### 3.mongodb 
MongoDB 是一个基于分布式文件存储的数据库。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

首先需要下载mongodb，下面是不同环境下的地址：
Windows 用户向导：https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/

Linux 用户向导：https://docs.mongodb.com/manual/administration/install-on-linux/

Mac 用户向导：https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/

配置与连接启动mongodb可以参考[Windows 平台安装 MongoDB](http://www.runoob.com/mongodb/mongodb-window-install.html)，这里不再赘述。

安装配置完成以后启动mongodb，以管理员身份运行，如下图：

![mongodb启动](https://img-blog.csdn.net/20180508104803669)

#### 4.Robomongo / Mongochef

[Robomongo](https://robomongo.org/) 是一个基于 Shell 的跨平台开源 MongoDB 可视化管理工具，支持 Windows、Linux 和 Mac，嵌入了 JavaScript 引擎和 MongoDB mongo，它还提了供语法高亮、自动补全、差别视图等。

[下载地址](https://robomongo.org/download)

下载并安装成功后点击左上角的 `Create` 创建一个连接，给该连接起个名字如: `localhost`，使用默认地址（localhost）和端口（27017）即可，点击 `Save` 保存。

双击 `localhost` 连接到 MongoDB 并进入交互界面。
![robomongo](https://img-blog.csdn.net/20180508100757197)

[MongoChef](http://3t.io/mongochef/) 是另一款强大的 MongoDB 可视化管理工具，支持 Windows、Linux 和 Mac。

[MongoChef 下载地址](http://3t.io/mongochef/#mongochef-download-compare)，选择左侧非商业用途的免费版下载。

MongoChef 相较于 Robomongo 更强大一些，但 Robomongo 比较轻量也能满足大部分的常规需求，这里不再赘述。

### 三、安装express及简单使用

#### 1.安装

可以使用`npm install express -g`全局安装，也可以新建一个项目再进行安装。

首先新建一个项目文件，并用`npm init`初始化一个`package.json`，有关`package.json`参考[package.json](https://github.com/nswbmw/N-blog/blob/master/book/2.5%20package.json.md)。

![package.json](https://img-blog.csdn.net/20180508103253338)

然后安装`express`并写入`package.json`：

```sh
npm i express@4.14.0 --save 
```

#### 2. 简单使用

新建 index.js，添加如下代码：

```js
const express = require('express')
const app = express()

app.get('/', function (req, res) {
  res.send('hello, express')
})

app.listen(3000)
```

这段代码生成一个express实例app，挂载了一个根路由控制器，监听3000端口并启动程序。运行 `node index`，打开浏览器访问 `localhost:3000` 时，页面显示hello, express。

![index](https://img-blog.csdn.net/20180508104038352)

#### 3.supervisor

在开发过程中，每次修改代码保存后，我们都需要手动重启程序，才能查看改动的效果。使用 [supervisor](https://www.npmjs.com/package/supervisor) 可以解决这个繁琐的问题，全局安装 supervisor：

```sh
npm i -g supervisor
```

运行 `supervisor index` 启动程序：

![supervisor](https://img-blog.csdn.net/2018050810390284)

supervisor 会监听当前目录下 node 和 js 后缀的文件，当这些文件发生改动时，supervisor 会自动重启程序。

### 四、express目录结构

![这里写图片描述](https://img-blog.csdn.net/2018050810421167)

```
bin——存放命令行程序。
node_modules——存放所有的项目依赖库。
public——存放静态文件，包括css、js、img等。
routes——存放路由文件。
views——存放页面文件（ejs模板）。
app.js——程序启动文件。
package.json——项目依赖配置及开发者信息。
```

有关于express基本框架和依赖的搭建大概就是这些，深入的学习和开发可以参考下面这些资料：

[Node.js express框架](http://www.runoob.com/nodejs/nodejs-express-framework.html)

[express API手册](http://www.expressjs.com.cn/4x/api.html)

[搭建开发框架Express，实现Web网站登录验证](https://blog.csdn.net/keliyxyz/article/details/51956814)

[express框架开发案例](http://www.cnblogs.com/ssqqhh/p/6289690.html)


