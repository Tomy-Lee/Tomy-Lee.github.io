---
layout:     post
title:      "Golang Web之Martini框架"
subtitle:   "martini框架初学及源码阅读"
date:       2017-11-13
author:     "tomylee"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Golang
    - Web
---

### Martini框架是使用Go语言作为开发语言的一个强力的快速构建模块化web应用与服务的开发框架。

### 一、安装
```
go get github.com/codegangsta/martini
```
### 二、使用
可以使用如下的代码来测试我们安装的包是否可用：

#### server.go
```
package main

import "github.com/codegangsta/martini"

func main() {
  m := martini.Classic()　　//创建一个典型的martini实例
  m.Get("/", func() string {     //接收对\的GET方法请求，第二个参数是对一请求的处理方法
    return "Hello world!"
  })
  m.Run()  //运行服务器
}
```
在命令行中输入下面的命令运行上面的代码：

```
go run server.go
```
接下来可以使用如下的网址访问应用：
http://localhost:3000

---
### 三、API举例
常量用于指定应用所处的环境：
```
const (
    Dev  string = "development"
    Prod string = "production"
    Test string = "test"
)
```
变量控制应用所处的环境：
```
var Env = Dev
```

#### 1.type BeforeFunc

BeforeFunc类型的方法在ResponseWriter方法被生效前调用。
```
type BeforeFunc func(ResponseWriter)
```

#### 2.type ClassicMartini
带有典型方法的Martini实例类型。
```
type ClassicMartini struct {
    *Martini
    Router
}
```

#### 3.func Classic() *ClassicMartini
使用这个方法创建一个典型的Martini实例。可以使用这个Martini实例来进行应用的管理：
```
func Classic() *ClassicMartini
```
#### 4.type ReturnHandler
ReturnHandler是Martini提供的用于路由处理并返回内容的服务。ReturnHandler对于向基于值传递的ResponseWriter写入是可响应的。
```
type ReturnHandler func(Context, []reflect.Value)
```
#### 5.type Route
Route是一个用于表示Martini路由层的接口。
```
type Route interface {
    // URLWith returns a rendering of the Route's url with the given string params.
    URLWith([]string) string
    Name(string)
}
```
#### 6.type Router
Router是Martini的路由接口。提供HTTP变量、处理方法栈、依赖注入。
```
type Router interface {
    // Get adds a route for a HTTP GET request to the specified matching pattern.
    Get(string, ...Handler) Route
    // Patch adds a route for a HTTP PATCH request to the specified matching pattern.
    Patch(string, ...Handler) Route
    // Post adds a route for a HTTP POST request to the specified matching pattern.
    Post(string, ...Handler) Route
    // Put adds a route for a HTTP PUT request to the specified matching pattern.
    Put(string, ...Handler) Route
    // Delete adds a route for a HTTP DELETE request to the specified matching pattern.
    Delete(string, ...Handler) Route
    // Options adds a route for a HTTP OPTIONS request to the specified matching pattern.
    Options(string, ...Handler) Route
    // Head adds a route for a HTTP HEAD request to the specified matching pattern.
    Head(string, ...Handler) Route
    // Any adds a route for any HTTP method request to the specified matching pattern.
    Any(string, ...Handler) Route

    // NotFound sets the handlers that are called when a no route matches a request. Throws a basic 404 by default.
    NotFound(...Handler)

    // Handle is the entry point for routing. This is used as a martini.Handler
    Handle(http.ResponseWriter, *http.Request, Context)
}
```

#### 7.func NewRouter() Router
创建一个路由实例。
```
func NewRouter() Router
```

#### 8.type Routes
Routes是Martini路由层的辅助服务。
```
type Routes interface {
    // URLFor returns a rendered URL for the given route. Optional params can be passed to fulfill named parameters in the route.
    URLFor(name string, params ...interface{}) string
    // MethodsFor returns an array of methods available for the path
    MethodsFor(path string) []string
}
```

---

参考博客：

[灰大羊-博客园](http://www.cnblogs.com/sitemanager/)

[golang martini 源码阅读笔记之martini核心](http://www.cnblogs.com/bicowang/p/5251267.html)
