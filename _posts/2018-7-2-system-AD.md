---
layout:     post
title: 皮皮点餐系统软件设计文档
subtitle:  系统分析与设计团队项目
date:       2018-7-2
author:     "tomylee"
header-img: "img/1217913296033.jpg"
tags:
    - JavaScript
    - team
---
目录：

- [一、项目环境](#一项目环境)
- [二、项目构建和管理工具](#二项目构建和管理工具)
- [三、技术选型](#三技术选型)
- [四、架构设计](#四架构设计)
- [五、模块划分与设计](#五模块划分与设计)
    - [系统整体模块图](#系统整体模块图)
        - [1.登录模块划分](#1登录模块划分)
        - [2.后台管理模块划分](#2后台管理模块划分)
        - [3.客户点餐模块划分](#3客户点餐模块划分)
        - [4.厨师管理模块](#4厨师管理模块)
- [六、数据库设计](#六数据库设计)
    - [数据库设计原则：](#数据库设计原则)
        - [1.概念设计](#1概念设计)
            - [1.1 商家信息实体属性图](#11-商家信息实体属性图)
            - [1.2 员工信息实体属性图](#12-员工信息实体属性图)
            - [1.3 菜品信息实体属性图](#13-菜品信息实体属性图)
            - [1.4 食材信息实体属性图](#14-食材信息实体属性图)
            - [1.5 订单信息实体属性图](#15-订单信息实体属性图)
        - [2.逻辑结构设计](#2逻辑结构设计)
        - [3.数据库表结构设计](#3数据库表结构设计)
            - [3.0 session表](#30-session表)
            - [3.1 商家信息表](#31-商家信息表)
            - [3.2 员工信息表](#32-员工信息表)
            - [3.3 菜品信息表](#33-菜品信息表)
            - [3.4 食材信息表](#34-食材信息表)
            - [3.5 订单信息表](#35-订单信息表)
- [七、路由设计](#七路由设计)
- [八、功能模型及用例设计](#八功能模型及用例设计)
    - [1.点菜的功能模型设计如下：](#1点菜的功能模型设计如下)
    - [2.后台用例设计](#2后台用例设计)
        - [2.1.登录用例](#21登录用例)
        - [2.2.用户信息管理用例](#22用户信息管理用例)
        - [2.3.食材信息管理用例](#23食材信息管理用例)
        - [2.4.菜品信息管理用例](#24菜品信息管理用例)
        - [2.5.账目信息管理用例](#25账目信息管理用例)
        - [2.6.评价信息管理用例](#26评价信息管理用例)
        - [2.7.员工信息管理用例](#27员工信息管理用例)
        - [2.8.厨师管理订单用例](#28厨师管理订单用例)
- [九、逻辑视图](#九逻辑视图)
- [十、物理视图](#十物理视图)
- [十一、技术备忘录](#十一技术备忘录)
- [十二、设计技术](#十二设计技术)

## 一、项目环境
- 服务器
  操作系统：Windows Server 2012标准版
  Web服务器：IIS
  数据库：MongoDB
- 客户端
  商户端：PC版Chrome浏览器
  用户端：常见手机浏览器

## 二、项目构建和管理工具
- 构建
  基于微修改的Express框架进行开发。
- 管理
  采用Github管理平台托管代码。

## 三、技术选型
1. 概述
   基于Nodejs + Express4 + MongoDB + Bootstrap4的web程序。
2. 前端技术
   基本技术：HTML + CSS + JavaScript
   - Jade
     > Jade是一个高性能的HTML模板引擎，由JavaScript实现，可供NodeJS使用。

     选择理由：
     - 数据传递
       在编写Jade时，一些无法确定的数据可用#{data}的形式代替，在编译时将数据以Json的形式传入即可，如{data:'Bob'}，对于一个web程序，这个功能是很实用的（如登录后显示不同的信息）。
     - 模板继承&前置、追加代码块
       支持使用`block`和`extends`关键字来实现模板继承，使用`block append`和`block prepend`实现代码追加、前置。
       该功能可大大减少重复代码量：将要多次使用的代码写成一个单独的模板，并在需要扩展的地方声明一个块，而其余模板继承这个模板，将要扩展的功能追加、前置或写在相应的块下既可。
     - 循环
       对于未知数量的数据，如传递过来的是一个菜单，现在要将其中的每一项菜品单独列出，如何实现？Jade提供了`each`和`for`关键字，实现遍历功能，方便易用。
   - Bootstrap4
     > Bootstrap是当前世界最受欢迎的用于建立**响应式、移动设备优先**的站点和应用的框架。

     选择理由：
     其实现了大量优美的样式，可以快速搭建出一个模板化的页面。大大减少了CSS样式的代码量，让开发者有更多的时间放在逻辑功能开发实现上。
   - JQuery
     > JQuery是一个JavaScript库，极大地简化了JavaScript编程。

     选择理由：
     JQuery提供了大量简洁明了、功能更强大的API，使得JS编程更简单高效，同时也使得代码更美观。
   - JQuery Ajax
     Ajax可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页。JQuery Ajax与Ajax的关系和JQuery与原生JS的关系一样。
     选择理由：
     因为原生HTML的数据交互功能有限，比较难实现本项目需求的功能，故采用功能更强大，也简单易用的Ajax技术。
3. 后端技术
   - NodeJS
     > NodeJS是一个基于Chrome V8引擎的JavaScript运行环境。
     
     选择理由：
     - 其使用了一个事件驱动的、异步I/O模型，轻量又高效。
     - 基于实际情况考虑，本团队成员有一定的NodeJS项目经验，对其较为熟悉。 
   - Express
     > Express是一个简洁而灵活的NodeJS Web应用框架，其有一套健壮的特性，和丰富的HTTP工具，可以快速搭建一个功能完整的网站。

     选择理由：
     - Express开发框架提供了一个较为合理且易于理解和扩展的目录结构，如下：（图）
       从网站的架构角度来说，这样的分层结构清晰易懂，也易于扩展成MVC结构。
   - MongoDB
     > MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。
     
     选择理由：
     - 相较于MySQL，MongoDB更轻量，且扩展性强。
     - MongoDB是面向文档的数据库，其数据是用类似JSON的BSON格式存储的，可读性更高。

## 四、架构设计
本项目的目录结构如下：
```
an-Order-system/
    bin/              #可执行文件
        www           
    models/           #业务的抽象化对象
        user.js
        ...
    node_modules/     #存放依赖的模块
    public/           #存放前端的静态文件
        images/       
        javascripts/
        stylesheets/
    routes/           #服务器路由
        index.js
    views/            #存放前端模板文件
        home.jade
        ...
    README.md
    app.js            #启动文件
    package.json      #存放工程的信息和模块依赖，执行npm install命令以安装依赖
    setting.js        #服务器与数据库的通讯配置文件
```
在Express框架原本的目录结构下新增了models文件夹，组成一个简单MVC结构。

 - M-models：存放数据实体，并定义其相关操作，主要与数据库进行交互。如user（用户），其提供save
（保存当前用户）、get（获取指定用户）、update（更新指定用户信息）方法。
 - V-views：存放前端的模板文件，路由根据请求用相应的数据填充、渲染指定的模板，然后将其返回给前端。
 - C-routes：控制处理所有的请求，根据请求执行相应的操作。

业务逻辑架构图：
![业务逻辑架构](/img/supplement/三层架构图.PNG)

## 五、模块划分与设计

### 系统整体模块图

本点餐系统包括了用户点餐以及后台管理两大部分，总体由四个大模块大模块（登录模块、后台管理模块、客户点餐模块、厨师管理模块）组成。

![总体模块](/img/model/总体模块.png)

#### 1.登录模块划分

![登录模块](/img/model/登录模块.png)

1.1用户登录

本点餐系统的用户登录分为店主登录和厨师登录，都需需要输入用户名和密码，登录时会自动检查用户输入的用户名以及密码的格式是否符合要求，并会给出提示，当用户名以及密码均符合格式时会根据用户的输入在数据库中检索是否有匹配项进行验证。

![用户登录](/img/model/用户登录.png)

1.2用户注册

用户注册只能进行店主注册，注册时需要输入用户名、密码、确认密码以及其他信息，并且会检测用户的输入是否符合格式要求，当用户的输入符合要求时，会先在数据库中进行匹配检测该用户名是否已经被占用，如没有，则注册成功。

![用户注册](/img/model/用户注册.png)

#### 2.后台管理模块划分

![后台管理模块](/img/model/后台管理模块.png)

2.1个人信息管理

当店主登录到后台管理界面时，可以在个人信息管理界面中更改自己的账户信息以及店铺属性信息，修改完成后点即保存会将修改后的信息同步保存到后台数据库用户表单中。另外也可以选择退出登录。

![个人信息管理](/img/model/个人信息管理.png)

2.2食材信息管理

店主可以在后台的食材信息管理界面中查看自己店铺中的现有食材列表信息，并且可以对现有的食材的信息详情进行修改以及删除，另外店主也可以添加新的食材。修改完成后点即保存会将修改后的信息同步保存到后台数据库中的食材表单中。

![食材信息管理](/img/model/食材信息管理.png)

2.3菜品信息管理

店主可以在后台的菜品信息管理界面中查看自己店铺中的现有菜品列表信息，并且可以对现有的菜品的信息详情进行修改以及删除，另外店主也可以添加新的菜品。修改完成后点即保存会将修改后的信息同步保存到后台数据库菜品表单中。

![菜品信息管理](/img/model/菜品信息管理.png)

2.4评价信息管理

店主可以在后台的评价信息管理界面查看顾客用完餐后提交的评价，包括上餐速度，味道等指标。

![评价信息管理](/img/model/评价信息管理.png)

2.5账目信息管理

店主可以在后台的账目信息管理界面查看本店具体每一笔完成的订单的收入、支出和盈利。同时店主也可以查看总体的收入、支出与利润。

![账目信息管理](/img/model/账目信息管理.png)

2.6员工信息管理

店主可以在后台的员工信息管理界面进行对本店员工的账户信息的管理，包括查看现有的员工列表，修改员工的账户信息，删除已有的员工以及添加新的员工。

![员工信息管理](/img/model/员工信息管理.png)


#### 3.客户点餐模块划分

![点菜模块](/img/model/点菜模块.png)

客户通过扫描二维码进入商家点菜界面，会显示本店的所有菜品信息，按类别排列；客户可以通过点击菜品按钮将此菜品加入购物车，选购完成后可以在购物车中看到点的所有菜品信息及总价格，也可以点击删除去掉不想要的菜。点击结算后进入订单详情页面，确认无误后点击支付，跳转到支付完成页面（根据课程要求，不添加真正的支付功能），并可以更改评价信息。

#### 4.厨师管理模块

![厨师模块](/img/model/厨师管理模块.png)

厨师用自己的账户密码登录后可以看到本店所有的已支付但是还没有上菜的订单，当订单制作完成并送到客户时，厨师可以更改订单状态，不再显示在本页面。

## 六、数据库设计

### 数据库设计原则：

一个好的数据库产品并不等于就是有一个好的应用系统，假设不能设计一个逻辑合理的数据库模型，不仅仅会增加程序客户端和服务器端的编程与维护的难度，而且还会大大影响系统在实际操作运行的性能。

数据库设计的两种方法:

(1)面向数据:以信息需求为主，同时兼顾处理需求; 

(2)面向过程:以处理需求为主，同时兼顾信息需求。

数据库设计是建立数据库和应用系统的核心和基础，它要求对于一个给定的应用环境，构造最优的数据库模式，建立一个数据库应用系统，该系统可以有效地存储数据，满足用户的应用需求。一般来说，在按照一个标准化的设计方法，设计数据库通常分为几个阶段：  

```
系统规划阶段:主要是确定系统的名称、范围; 确定系统功能和性能的发展目标，确定所需的系统资源;估计系统开发成本，确定系统实施计划和时间表;分析估计该系统的有效性可达到确定系统的设计原则和技术路线。 
需求分析阶段:需要在用户调查的基础上，通过分析，逐步的明确用户对系统的各种需求，包括数据需求以及围绕这些数据的业务处理需求。
概念设计阶段:要产生反映的信息需求，组织结构数据库的概念，即概念模型。概念模型必须有能力来表达丰富的语义，容易沟通和理解，而且要很容易改变，易于转换为各种数据模型，概念模型来自容易与DBMS和其他相关特性的逻辑模型。选择的系统数据库E-R图模型的概念设计，也就是所谓的实体 - 关系模型。
逻辑设计阶段:除了要把E-R图的实体-联系类型转换成选定的 DBMS支持的数据类型，还要设计子模式并且对模式进行评价，而且最后为了使模式适应信息的不同表示，需要进行模式的优化。  
物理设计阶段：主要任务是数据库中的数据存储在物理设备上的结构和存取方法的设计。数据库的物理结构依赖于给定的计算机系统，并有密切的关系数据库管理系统的具体选择。物理设计约束通常包括一些操作，如响应时间和存储要求。  
系统实施阶段:主要包括建立实际的数据库结构、装入试验数据对应用程序进行测试以及装入实际数据建立实际数据库三个步骤。
```

#### 1.概念设计  

本系统涉及到的实体有：session信息、信息、菜品信息、食材信息、订单信息。  

本系统主要实现了用户点菜、用户评价、后台管理员工、后台管理食材菜品、后台管理订单评价信息、后台修改个人资料等等。其中session表用来存储登录信息和状态，商家信息表是主表，它的从表都有员工信息表、菜品信息表、食材信息表、订单信息表。 

##### 1.1 商家信息实体属性图

![商家实体](/img/model/商家信息.png)

##### 1.2 员工信息实体属性图

![员工实体](/img/model/员工信息.png)

##### 1.3 菜品信息实体属性图

![菜品实体](/img/model/菜品信息.png)

##### 1.4 食材信息实体属性图

![食材实体](/img/model/食材属性.png)

##### 1.5 订单信息实体属性图

![订单实体](/img/model/订单属性.png)


系统整体的E-R图：

![E-R](/img/model/ER.png)

#### 2.逻辑结构设计

通过上述属性图的描述，根据E-R向关系模型的转化规则，可以得到以下关系模型：  

商家信息（用户名、密码、电话、邮箱、店名、地址）

员工信息（店主、头像、用户名、密码、姓名、年龄、电话、职位）

菜品信息（店主、菜名、图片、食材、类别、成本、售价）

食材信息（店主、食材名、市场价、进价、存货量）

订单信息（桌号、流水号、店主、菜单、数量、单价、口味、上菜速度、服务态度、总评价、总成本、总售价、状态）

#### 3.数据库表结构设计

##### 3.0 session表

![session表](/img/model/session表.png)

##### 3.1 商家信息表

![商家信息表](/img/model/user表.png)

##### 3.2 员工信息表

![员工信息表](/img/model/employee表.png)

##### 3.3 菜品信息表

![菜品信息表](/img/model/menu表.png)

##### 3.4 食材信息表

![食材信息表](/img/model/ingredients表.png)

##### 3.5 订单信息表

![订单信息表](/img/model/order表.png)

## 七、路由设计

### 1.返回状态码

|  类型  | stateCode | info |
| :--: | :-------: | :--: |
|  请求操作成功  |    200    | NULL |
|成功GET且文件未改变|304|NULL|
|页面重定向|302|NULL|
|  错误  |    500    | 错误信息 |

### 2.店主相关操作

| 路由  |  方法  |   说明     |  测试  |
| :--: | :-------: | :--: | :--: |
| /?op=regist | POST | 店主进行注册账户 | OK |
| /?op=managerLogin | POST | 店主登录 | OK |
| /user/?username={username}&info=personal|GET|获取店主个人主页|OK|
| /user/?op=logout| GET | 店主登出账户 | OK |
| /user/?username={username}&info=personal-account | POST | 店主修改个人信息 | OK |
|  /user/?username={username}&info=personal-store | POST | 店主修改商家信息 | OK |
|/user/?username={username}&info=ingredients | GET | 获取食材信息页面 | OK |
|  /user/?username={username}&info=ingredients | POST | 店主增加删除食材 | OK |
| /user/?username={username}&info=menu |GET| 获取菜品信息页面 | OK |
| /user/?username={username}&info=menu | POST | 修改菜品信息 | OK |
| /user/?username={username}&info=accounts | GET | 店主获取店铺的收支信息 | OK |
|/user/?username={username}&info=evaluation|GET|查看订单评价信息|OK|
|/user/?username={username}&info=employee|GET|获取员工信息页面|OK|
| /user/?username={username}&info=employee&op=save|POST|修改员工信息|OK|
| /user/?username={username}&info=employee&op=new|POST|新增员工|OK|
| /user/?username={username}&op=uploadImg | POST | 修改图片 | OK |
| /images/upload/{image_address} | GET | 加载新图片 | OK |


### 3.厨师相关操作

| 路由  |  方法  |   说明     |  测试  |
| :--: | :-------: | :--: | :--: |
|  /?op=employeeLogin | POST | 厨师登录 | OK |
| /employee/?username={username}&managername={managername} | GET | 显示订单界面 | OK |
| /employee/?username={username}&managername={managername} |POST | 更改订单状态 | OK |


### 4.顾客相关操作

| 路由  |  方法  |   说明     |  测试  |
| :--: | :-------: | :--: | :--: |
| /{managername}/client?tableID={tableid} | GET | 顾客扫码登录获取菜单界面 | OK |
| /{managername}/client?tableID={tableid} | POST | 顾客完成点餐后进入确认订单支付界面 | OK |
| /{managername}/handin/?id={streamid}&order={orderdetail}&tableID={tabelid} | GET | 顾客获取订单详情信息 | OK |
| /{managername}/handin/?id={streamid}&order={orderdetail}&tableID={tabelid}  | POST | 顾客确认支付 | OK |
| /{managername}/handin/?id={streamid}&order={orderdetail}&tableID={tabelid}&finish={true} | GET | 顾客获取订单评价界面 | OK |
| /{managername}/handin/?id={streamid}&order={orderdetail}&tableID={tabelid}&finish={true}  | POST | 顾客提交订单评价信息 | OK |



### 5.API示例说明---店主添加菜品
#### 5.1.应用场景
 店主在点餐系统中添加一项之前没有的菜品。
#### 5.2.接口示例
localhost:3000/user/?username=qqqqqq&op=uploadImg

localhost:3000/user/?username=qqqqqq&info=menu
#### 5.3.是否需要证书
 不需要
#### 5.4.请求参数
| 字段名 |  变量名  |   必填     |  类型  |  示例值 |  描述 |
| :--: | :-------: | :--: | :--: | :--: | :--: |
| 店主账号| owner | 是| string| qqqqqq | 店主用于管理该系统的账号|
| 菜名| name|是|string|麻辣香锅|新加菜品的名称|
|图片|imgSrc|否|string|images/upload/f.png|新加入的菜品的图片地址|
|菜品食材|ingredients|是|string|肉片、菜花|菜品的制作食材|
|菜品类别|class|是|string|rice|用于在点菜界面分类|
|菜品成本|cost|是|string|13|菜品的制作成本价|
|菜品售价|price|是|string|25|菜品的售价|


#### 5.5.举例如下
```
Menu {
  owner: 'qqqqqq',
  name: '水',
  imgSrc: 'images/upload/50d535e8fa166d1cfd92fa9c9a36ebaf.png',
  ingredients: '水',
  class: 'drink',
  cost: '3',
  price: '5',
 } 
 ```
 
#### 5.6.返回结果

| 字段名 |  变量名  |   必填     |  类型  |  示例值 |  描述 |
| :--: | :-------: | :--: | :--: | :--: | :--: |
|返回状态码|result|是|string|SUCCESS|添加菜品操作成功|




## 八、功能模型及用例设计

### 1.点菜的功能模型设计如下：
![15331150](/img/System_Sequence_Diagram/15331150.png)

### 2.后台用例设计

#### 2.1.登录用例
![1](/img/usecase/账户登录.png)

#### 2.2.用户信息管理用例
![2](/img/usecase/账户信息管理.png)

#### 2.3.食材信息管理用例
![3](/img/usecase/食材管理.png)

#### 2.4.菜品信息管理用例
![4](/img/usecase/菜品管理.png)

#### 2.5.账目信息管理用例

![5](/img/model/账目用例.png)

#### 2.6.评价信息管理用例
![6](/img/usecase/评价信息管理.png)


#### 2.7.员工信息管理用例
![7](/img/usecase/员工账户管理.png)

#### 2.8.厨师管理订单用例
![8](/img/usecase/厨师.png)

## 九、逻辑视图

我们此次的皮皮怪点餐系统采用的是经典的分层架构，整个系统比较简单，主要分为UI层，领域层以及技术服务层。
![逻辑视图](/img/supplement/逻辑视图.png)

## 十、物理视图

![物理视图](/img/supplement/物理视图.png)

## 十一、技术备忘录



#### 1.问题：框架中循环+异步的执行使得mongodb数据库操作出错。

解决方案：将循环操作移动至数据库模型定义中，一次保存操作，不管修改多少条信息，只打开一次数据库，然后循环增删改，在最后一次循环时等待执行结束后关闭数据库，然后等待1000ms执行刷新当前页面。

动机：如果不解决这个问题，我们只能在一次保存中增加/修改/删除一条相关信息，这样的操作比较复杂，使用性能也不高。解决问题之后可以修改多条信息一次保存。这样的优势是显而易见的。

未决问题：同时增加多条信息由于循环异步问题会导致保存的多条信息都是最后一条的信息的复制。

其他可供选择的方案：使用Q的promise机制实现同步；或者使用async实现同步。

#### 2.问题：循环操作使得数据库写入赋值出现问题。

解决方案：使用函数闭包，在每次循环中使用function(i)，保持每次的i值数据库操作不会被后续循环修改，解决了写入多条信息时数据重复的问题。

动机：如果不解决这个问题，我们在新建employee、menu时一次只能新建一条，点击保存写入数据库后再新建，不能同时新建多条一次写入。解决之后管理后台操作更加便捷。

#### 3.问题：session问题，用户可以通过修改url直接进入他人账户。

解决方案：使用session判断机制，保存当前登录用户的session信息，并在每次get时进行判断，如果输入的或要跳转的不是本用户的页面地址，则强制跳转到本人主页。

动机：这个问题的后果显而易见，如果其他人可以通过url直接访问每个账户界面，那么毫无安全性可言，不能保证每家店的数据安全和私密。

未决问题：试图引入单点登录机制，使得同一个用户不能用时在线，但现在还没有找到更好的解决思路。

#### 4.问题：第一次项目基本完成时，固定桌号和用户，不能动态查看其他店的信息。

解决方案：使用url绑定用户名和桌号，在后台管理个人主页新建生成二维码的功能，使用二维码访问不同的桌子，更贴近实际应用。

动机：如果不添加这个功能，我们就只能固定访问一个店的一个桌子的点菜界面，无法贴近实际点餐系统的逻辑。


## 十二、设计技术

### 1.结构化程序设计

结构化程序设计提出的原则可以归纳为32个字：自顶向下，逐步细化；清晰第一，效率第二；书写规范，缩进格式；基本结构，组合而成。

```
自顶向下：程序设计时，应先考虑总体，后考虑细节；先考虑全局目标，后考虑局部目标。不要一开始就过多追求众多的细节，先从最上层总目标开始设计，逐步使问题具体化。

逐步求精：对复杂问题，应设计一些子目标作为过渡，逐步细化。

模块化：一个复杂问题，肯定是由若干稍简单的问题构成。模块化是把程序要解决的总目标分解为子目标，再进一步分解为具体的小目标，把每一个小目标称为一个模块。
```
首先是对于项目整体结构的确定：

![](/img/model/设计技术1.png)

自顶向下，逐步细化，确定整体结构后，划分为几个大的模块结构，协定好各个模块的功能和联系，然后再进行逐步细化，将模块内的函数实现：

a.数据库模块文件结构：

![](/img/model/设计技术2.png)

b.前端数据控制模块文件结构：

![](/img/model/设计技术3.png)

c.前端静态文件模块文件结构：

![](/img/model/设计技术4.png)

以order数据库的结构为例，其他模块文件同理：

![](/img/model/设计技术5.png)

建立order的构造函数，order数据库的get函数，order数据库的update函数结构接口，然后再进行细化，具体实现内部的逻辑。

对于底层的结构化程序设计技术，结构化的程式是以一些简单、有层次的程式流程架构所组成，可分为顺序、选择及循环。

以index为例，对于get或post请求的操作码判断，首先是顺序的判断请求的url：

![](/img/model/设计技术6.png)

然后在单个函数内部，进行选择和循环，确定执行哪个操作：

![](/img/model/设计技术7.png)

![](/img/model/设计技术8.png)

采用结构化程序设计能带来的好处：
```
①整体思路清楚，目标明确。

②设计工作中阶段性非常强，有利于系统开发的总体管理和控制。

③在系统分析时可以诊断出原系统中存在的问题和结构上的缺陷。
```

### 2.面向对象程序设计（OOP）

OOP把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。而面向对象的程序设计把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。

在我们的项目中，所有的数据操作过程都是以数据对象为基本，以employee和order为例，操作数据集合都被视为对象，每个对象有自己的属性：

![](/img/model/设计技术18.png)

在index的操作中不是一组数据处理函数的顺序执行，而是如果要操作这些数据，必须首先创造出这些数据对象，然后给数据模型发送操作信息，让对象自己执行数据操作。

employee的对象和操作：


![](/img/model/设计技术16.png)

order的对象和操作：

![](/img/model/设计技术17.png)


### 3.设计模式

####  3.1 MVC模式
```
V即View视图是指用户看到并与之交互的界面。比如由html元素组成的网页界面，或者软件的客户端界面。MVC的好处之一在于它能为应用程序处理很多不同的视图。在视图中其实没有真正的处理发生，它只是作为一种输出数据并允许用户操纵的方式。

M即model模型是指模型表示业务规则。在MVC的三个部件中，模型拥有最多的处理任务。被模型返回的数据是中立的，模型与数据格式无关，这样一个模型能为多个视图提供数据，由于应用于模型的代码只需写一次就可以被多个视图重用，所以减少了代码的重复性。

C即controller控制器是指控制器接受用户的输入并调用模型和视图去完成用户的需求，控制器本身不输出任何东西和做任何处理。它只是接收请求并决定调用哪个模型构件去处理请求，然后再确定用哪个视图来显示返回的数据。
```
以menu相关请求为例：

model文件：

![](/img/model/设计技术12.png)

view文件：

![](/img/model/设计技术10.png)

controller文件：

![](/img/model/设计技术11.png)

```
an-Order-system/
    bin/              #可执行文件
        www           
    models/           #业务的抽象化对象
        user.js
        ...
    node_modules/     #存放依赖的模块
    public/           #存放前端的静态文件
        images/       
        javascripts/
        stylesheets/
    routes/           #服务器路由
        index.js
    views/            #存放前端模板文件
        home.jade
        ...
    README.md
    app.js            #启动文件
    package.json      #存放工程的信息和模块依赖，执行npm install命令以安装依赖
    setting.js        #服务器与数据库的通讯配置文件
```

![业务逻辑架构](/img/supplement/三层架构图.PNG)

![](/img/model/设计技术9.jpg)


MVC模式的优点：
```
1.耦合性低
视图层和业务层分离，这样就允许更改视图层代码而不用重新编译模型和控制器代码，同样，一个应用的业务流程或者业务规则的改变只需要改动MVC的模型层即可。因为模型与控制器和视图相分离，所以很容易改变应用程序的数据层和业务规则。

2.重用性高
MVC模式允许使用各种不同样式的视图来访问同一个服务器端的代码，因为多个视图能共享一个模型，它包括任何WEB（HTTP）浏览器或者无线浏览器（wap），比如，用户可以通过电脑也可通过手机来订购某样产品，虽然订购的方式不一样，但处理订购产品的方式是一样的。由于模型返回的数据没有进行格式化，所以同样的构件能被不同的界面使用。

3.部署快，生命周期成本低
MVC使开发和维护用户接口的技术含量降低。使用MVC模式使开发时间得到相当大的缩减，它使程序员（Java开发人员）集中精力于业务逻辑，界面程序员（HTML和JSP开发人员）集中精力于表现形式上。

4.可维护性高
分离视图层和业务逻辑层也使得WEB应用更易于维护和修改。
```

#### 3.2 数据访问对象模式(DAO)

数据访问对象模式（Data Access Object Pattern）或 DAO 模式用于把低级的数据访问 API 或操作从高级的业务服务中分离出来。以下是数据访问对象模式的参与者。
```
数据访问对象接口（Data Access Object Interface） - 该接口定义了在一个模型对象上要执行的标准操作。
数据访问对象实体类（Data Access Object concrete class） - 该类实现了上述的接口。该类负责从数据源获取数据，数据源可以是数据库，也可以是 xml，或者是其他的存储机制。
模型对象/数值对象（Model Object/Value Object） - 该对象是简单的 POJO，包含了 get/set 方法来存储通过使用 DAO 类检索到的数据。
```
数据访问对象接口，以order的数据处理为例，在index中提供了操作数据的接口：

![](/img/model/设计技术15.png)

数据访问对象实体，构造数据实体，通过上述接口更改数据：

![](/img/model/设计技术14.png)

模型对象，以order数据模型为例，通过数据库操作语句进行数据处理：

![](/img/model/设计技术13.png)
