---
layout:     post
title:      "Docker架构初探及安装"
subtitle:   "系统分析与设计个人学习报告1"
date:       2018-04-10
author:     "tomylee"
header-img: "img/post-bg-2015.jpg"
tags:
    - 系统分析与设计
---

### 一、Docker简介
Docker是一个开源的应用容器引擎，基于 Go 语言 并遵从Apache2.0协议开源。Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。
 
相关链接:  &nbsp;&nbsp; [Docker 官网](http://www.docker.com) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [Github Docker 源码](https://github.com/docker/docker)

一个完整的Docker有以下几个部分组成:
```
dockerClient    客户端
Docker Daemon   守护进程
Docker Image    镜像
DockerContainer 容器
```

### 二、Docker的应用场景：
#### 1.Web 应用的自动化打包和发布。
#### 2.自动化测试和持续集成、发布。
#### 3.在服务型环境中部署和调整数据库或其他的后台应用。
#### 4.从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环境。

### 三、Docker 架构
Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。Docker 容器通过 Docker 镜像来创建。容器与镜像的关系类似于面向对象编程中的对象与类。

![jiagou](http://www.runoob.com/wp-content/uploads/2016/04/576507-docker1.png)
(图片及部分内容来自百科)

|||
|---|---|
|Docker 镜像(Images)|Docker 镜像是用于创建 Docker 容器的模板。|
|Docker 容器(Container)|容器是独立运行的一个或一组应用。|
|Docker 客户端(Client)|Docker 客户端通过命令行或者其他工具使用 Docker API (https://docs.docker.com/reference/api/docker_remote_api) 与 Docker 的守护进程通信。|
|Docker 主机(Host)|一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。|
|Docker 仓库(Registry)|Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。Docker Hub(https://hub.docker.com) 提供了庞大的镜像集合供使用。|
|Docker Machine|Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。|

### 四、Docker 的优点
#### 1、简化程序：Docker 让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，便可以实现虚拟化。Docker改变了虚拟化的方式，使开发者可以直接将自己的成果放入Docker中进行管理。方便快捷已经是 Docker的最大优势，过去需要用数天乃至数周的	任务，在Docker容器的处理下，只需要数秒就能完成。
#### 2、避免选择恐惧症：如果你有选择恐惧症，还是资深患者。Docker 帮你	打包你的纠结！比如 Docker 镜像；Docker 镜像中包含了运行环境和配置，所以 Docker 可以简化部署多种应用实例工作。比如 Web 应用、后台应用、数据库应用、大数据应用比如 Hadoop 集群、消息队列等等都可以打包成一个镜像部署。
#### 3、节省开支：一方面，云计算时代到来，使开发者不必为了追求效果而配置高额的硬件，Docker 改变了高性能必然高价格的思维定势。Docker 与云的结合，让云空间得到更充分的利用。不仅解决了硬件管理的问题，也改变了虚拟化的方式。 

### 五、windows10安装Docker

首先需要打开win10的Hyper-V，右键开始菜单，选择应用和功能（F），找到相关设置：程序和功能，单击：

![1](/img/system/1.png)

然后选择左侧的启用或关闭Windows功能：

![2](/img/system/25.png)

打开Hyper-V：

![3](/img/system/5.png)

Toolbox下载地址：https://www.docker.com/get-docker 

点击 Get Docker Community Edition，并下载 Windows 的版本：

![4](/img/system/4.png)

![5](/img/system/3.png)

点击图标按流程进行安装：

![6](/img/system/6.png)

![7](/img/system/7.png)

安装完成后，Docker 会自动启动。通知栏上会出现个鲸鱼图标，这表示 Docker 正在运行。

可以在命令行执行 docker version 来查看版本号，docker run hello-world 来载入测试镜像测试。

---
---

镜像加速（来自菜鸟教程）：鉴于国内网络问题，后续拉取 Docker 镜像十分缓慢，我们可以需要配置加速器来解决，我使用的是网易的镜像地址：http://hub-mirror.c.163.com。

新版的 Docker 使用 `/etc/docker/daemon.json（Linux）` 或者 `%programdata%\docker\config\daemon.json（Windows）` 来配置 `Daemon`。

请在该配置文件中加入（没有该文件的话，请先建一个）：
```
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
```
