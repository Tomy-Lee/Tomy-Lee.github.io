---
layout:     post
title:      "cloudgo-in&out"
subtitle:   "服务计算作业golang语言简单web应用"
date:       2017-11-20
author:     "tomylee"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Golang
    - Web
---

### 一、概述
设计一个 web 小应用，展示静态文件服务、js 请求支持、模板输出、表单处理、Filter 中间件设计等方面的能力。（不需要数据库支持）

### 二、任务要求
编程 web 应用程序 cloudgo-io。 请在项目 README.MD 给出完成任务的证据！

**基本要求**

>支持静态文件服务
>
>支持简单 js 访问
>
>提交表单，并输出一个表格
>
>对 `/unknown` 给出开发中的提示，返回码 `5xx`

---
**由于这次是在上次的基础上继续实验（我没有使用上次的martini框架，改成了老师提供的框架），所以其他过程不再赘述，只展示测试过程。**
---
### 三、通过老师博客进行学习与设计(学习内容来自pmlpml的CSDN博客)
#### 1.1 使用文件服务
一般来说，生产环境静态文件的访问交给 WEB 服务器 Apache / Lighttpd / Nginx 处理。在开发、测试阶段也可以让 net/http 库处理。

server.go
```
package service

import (
    "net/http"
    "os"

    "github.com/codegangsta/negroni"
    "github.com/gorilla/mux"
    "github.com/unrolled/render"
)

// NewServer configures and returns a Server.
func NewServer() *negroni.Negroni {

    formatter := render.New(render.Options{
        IndentJSON: true,
    })

    n := negroni.Classic()
    mx := mux.NewRouter()

    initRoutes(mx, formatter)

    n.UseHandler(mx)
    return n
}

func initRoutes(mx *mux.Router, formatter *render.Render) {
    webRoot := os.Getenv("WEBROOT")
    if len(webRoot) == 0 {
        if root, err := os.Getwd(); err != nil {
        
            panic("Could not retrive working directory")
        } else {
            webRoot = root
            //fmt.Println(root)
        }
    }

    //mx.HandleFunc("/api/test", apiTestHandler(formatter)).Methods("GET")
    mx.PathPrefix("/").Handler(http.FileServer(http.Dir(webRoot + "/assets/")))

}
```

Go 的 `net/http` 包中提供了静态文件的服务，`ServeFile` 和 `FileServer` 等函数。

首先我们需要在服务器上创建目录，以存放静态内容。例如：
```
assets(静态文件虚拟根目录)
  |-- js
  |-- images
  +-- css
```
仅一条语句就实现了 `mx.PathPrefix("/").Handler(http.FileServer(http.Dir(webRoot + "/assets/")))` 静态文件服务。它的含义是将 path 以 “/” 前缀的 URL 都定位到 `webRoot + "/assets/"` 为虚拟根目录的文件系统。 有必要描述这语句中的函数：
```
http.Dir 是类型。将字符串转为 http.Dir 类型，这个类型实现了 FileSystem 接口。（Dir 不是函数）
http.FileServer() 是函数，返回 Handler 接口，该接口处理 http 请求，访问 root 的文件请求。
mx.PathPrefix 添加前缀路径路由。
```
#### 1.2 支持 JavaScript 访问
随着 web 页面技术的进步，页面中大量使用 javascript。 添加一个服务：

apitest.go
```
package service

import (
    "net/http"

    "github.com/unrolled/render"
)

func apiTestHandler(formatter *render.Render) http.HandlerFunc {

    return func(w http.ResponseWriter, req *http.Request) {
        formatter.JSON(w, http.StatusOK, struct {
            ID      string `json:"id"`
            Content string `json:"content"`
        }{ID: "8675309", Content: "Hello from Go!"})
    }
}
```

这段代码非常简单，输出了一个 匿名结构 ，并 JSON (JavaScript Object Notation) 序列化输出。 打开前面程序的注释，运行网站并用 curl 测试输出!
```
$ curl http://localhost:8080/api/test
{
  "id": "8675309",
  "content": "Hello from Go!"
}
```

为了便于理解，课程给的案例非常简答。index.html 是
```
<html>
<head>
  <link rel="stylesheet" href="css/main.css"/>
  <script src="http://code.jquery.com/jquery-latest.js"></script>
  <script src="js/hello.js"></script>
</head>
<body>
  <img src="images/cng.png" height="48" width="48"/>
  Sample Go Web Application!!
      <div>
          <p class="greeting-id">The ID is </p>
          <p class="greeting-content">The content is </p>
      </div>
</body>
</html>
```

使用的 hello.js 是：
```
$(document).ready(function() {
    $.ajax({
        url: "/api/test"
    }).then(function(data) {
       $('.greeting-id').append(data.id);
       $('.greeting-content').append(data.content);
    });
});
```

通过 web 应用控制台 Negroni 输出追踪，获知网页使用 javascript 获取了信息。

现在，你应该可以顺利的与 VUE，Bootstrap 这些做的应用前端与 golang 集成在一起了！

#### 1.3 处理静态路径前缀
在 web 应用中，部分应用会将所有静态文件访问路径用独立前缀，例如： `http://localhost:8080/static/js/hello.js`，这时路由如何设置呢？
```
mx.PathPrefix("/static").Handler(http.FileServer(http.Dir(webRoot + "/assets/")))
```

显然，404 页面出现了。

正确的代码是：
```
mx.PathPrefix("/static").Handler(http.StripPrefix("/static/", http.FileServer(http.Dir(webRoot+"/assets/"))))
```

#### 2.1 输出 html 页面

如果你仅是打算输出 html 页面，github.com/unrolled/render 是最为简单和直接。

srver.go
```
package service

import (
    "net/http"
    "os"

    "github.com/codegangsta/negroni"
    "github.com/gorilla/mux"
    "github.com/unrolled/render"
)

// NewServer configures and returns a Server.
func NewServer() *negroni.Negroni {

    formatter := render.New(render.Options{
        Directory:  "templates",
        Extensions: []string{".html"},
        IndentJSON: true,
    })

    n := negroni.Classic()
    mx := mux.NewRouter()

    initRoutes(mx, formatter)

    n.UseHandler(mx)
    return n
}

func initRoutes(mx *mux.Router, formatter *render.Render) {
    webRoot := os.Getenv("WEBROOT")
    if len(webRoot) == 0 {
        if root, err := os.Getwd(); err != nil {
        
            panic("Could not retrive working directory")
        } else {
            webRoot = root
            //fmt.Println(root)
        }
    }

    mx.HandleFunc("/", homeHandler(formatter)).Methods("GET")
    mx.PathPrefix("/").Handler(http.FileServer(http.Dir(webRoot + "/assets/")))
}
```

要点： 

>（1）formatter 构建，指定了模板的目录，模板文件的扩展名 
>
>（2）homeHandler 使用了模板


我们在当前目录下，建立了 `assets` 和 `templates` 目录。 `index.html` 在 `templates` 目录中。

```
<html>
<head>
  <link rel="stylesheet" href="css/main.css"/>
</head>
<body>
  <img src="images/cng.png" height="48" width="48"/>
  Sample Go Web Application!!
      <div>
          <p class="greeting-id">The ID is {{.ID}}</p>
          <p class="greeting-content">The content is {{.Content}}</p>
      </div>
</body>
</html>
```

其中 {{.}} 表示数据填充位置。 {{.ID}} 表示该数据的 ID 属性。

home.go 处理了数据填充。
```
package service

import (
    "net/http"

    "github.com/unrolled/render"
)

func homeHandler(formatter *render.Render) http.HandlerFunc {

    return func(w http.ResponseWriter, req *http.Request) {
        formatter.HTML(w, http.StatusOK, "index", struct {
            ID      string `json:"id"`
            Content string `json:"content"`
        }{ID: "8675309", Content: "Hello from Go!"})
    }
}
```

我们使用 formatter 的 HTML 直接将数据注入模板，并输出到浏览器。 更多内容见 render 在 git 上的 README。

运行程序：$ go run main.go

在浏览器访问：`http://localhost:8080`

注意观察控制台 negroni 输出。

#### 2.2 使用text/template库
golang 提供了强大的 template 库，上述 Render 仅是它的简单包装。 官网的例子也非常简单：
```
type Inventory struct {
    Material string
    Count    uint
}
sweaters := Inventory{"wool", 17}
tmpl, err := template.New("test").Parse("{{.Count}} items are made of {{.Material}}")
if err != nil { panic(err) }
err = tmpl.Execute(os.Stdout, sweaters)
if err != nil { panic(err) }
```
这里给出了模板使用的过程：创建 - 编译 - 执行。 
即 `template.New("name").Parse("{{.Content}} string").Execute(writer,data)`

[golang模板语法简明教程](http://www.cnblogs.com/Pynix/p/4154630.html)

[Go语言核心之美 3.6－template模版](https://studygolang.com/articles/6727)

#### 3.1 处理Request及negroni 与 Filter 模式

[golang web 服务器 request 与 response 处理](http://blog.csdn.net/pmlpml/article/details/78539261)

### 四、测试

### <1>使用文件服务
一般来说，生产环境静态文件的访问交给 WEB 服务器 Apache / Lighttpd / Nginx 处理。在开发、测试阶段也可以让 net/http 库处理。
#### 1.静态文件服务,建立/api/test，并在服务器上创建目录，以存放静态内容，输出ID，age，content。下面是/api/test实现后的输出情况。
![这里写图片描述](http://img.blog.csdn.net/20171118211043005)

![这里写图片描述](http://img.blog.csdn.net/20171118211054496)

![](http://img.blog.csdn.net/20171118211103952)

#### ２.下面依次展示了服务器添加文件结构以及文件夹下添加css、js文件后的情况。

![这里写图片描述](http://img.blog.csdn.net/20171118210347558)

![这里写图片描述](http://img.blog.csdn.net/20171118210340438)

![这里写图片描述](http://img.blog.csdn.net/20171118210328201)

#### 3.添加完成后，再增加index.html文件，访问端口并查看监听情况。
![这里写图片描述](http://img.blog.csdn.net/20171118210856644)

![这里写图片描述](http://img.blog.csdn.net/20171118211229460)

#### 4.使用模板template输出。
![这里写图片描述](http://img.blog.csdn.net/20171118211347767)

#### 5.输出一个登录账户密码的表单表格。
![这里写图片描述](http://img.blog.csdn.net/20171118211436069)

#### 6./api/unknown正在开发中的501未实现信息提示。
![这里写图片描述](http://img.blog.csdn.net/20171118211526245)

代码位置：[cloudgo-io](https://github.com/Tomy-Lee/cloudgo-io)
