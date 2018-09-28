---
layout: post
title: 'SpringBoot简单的示例demo'
subtitle: '简单的搭建SpringBoot框架'
date: 2018-07-28
categories: 技术
cover: ''
tags: SpringBoot Simple
---

> 该demo只是简易的搭建SpringBoot的框架。
>
> 适合当作SpringBoot的一个简易示例。
>
> 编辑器：IDEA。
>
> 所用技术：Maven SpringBoot

## 第一步

打开 ·idea· 编辑器。

创建Spring Initializr 项目。

![image-20180928165615603](https://raw.githubusercontent.com/MonkeyKing2016/spring-boot-demo-share/master/springboot-simple/src/main/resources/static/assets/img/image-20180928165615603.png)



## 第二步

点击next

![image-20180928170419290](https://raw.githubusercontent.com/MonkeyKing2016/spring-boot-demo-share/master/springboot-simple/src/main/resources/static/assets/img/image-20180928170419290.png)



## 第三步

选择相应的组件。我们这里就用web组件。

![image-20180928170614941](https://raw.githubusercontent.com/MonkeyKing2016/spring-boot-demo-share/master/springboot-simple/src/main/resources/static/assets/img/image-20180928170614941.png)



## 第四步

在idea中将其显示出来。

找到如下视图（Maven Projects）

![image-20180928170815154](https://raw.githubusercontent.com/MonkeyKing2016/spring-boot-demo-share/master/springboot-simple/src/main/resources/static/assets/img/image-20180928170815154.png)

点开之后

![image-20180928170947627](https://raw.githubusercontent.com/MonkeyKing2016/spring-boot-demo-share/master/springboot-simple/src/main/resources/static/assets/img/image-20180928170947627.png)

将刚才我们创建的项目pom选择。

![image-20180928171127474](https://raw.githubusercontent.com/MonkeyKing2016/spring-boot-demo-share/master/springboot-simple/src/main/resources/static/assets/img/image-20180928171127474.png)

这样就成功啦。

## 最后一步

运行SimpleApplication 查看控制台。输出以下信息则为创建成功。

![image-20180928171244548](https://raw.githubusercontent.com/MonkeyKing2016/spring-boot-demo-share/master/springboot-simple/src/main/resources/static/assets/img/image-20180928171244548.png)



## HelloWord

因为我们的项目选用了Web组件，所以我们可以通过页面请求来实现HelloWord。

编写如下代码

![image-20180928171847652](https://raw.githubusercontent.com/MonkeyKing2016/spring-boot-demo-share/master/springboot-simple/src/main/resources/static/assets/img/image-20180928171847652.png)

再次运行项目，网址输入 

`localhost:8080/hello/word`

![image-20180928171949044](https://raw.githubusercontent.com/MonkeyKing2016/spring-boot-demo-share/master/springboot-simple/src/main/resources/static/assets/img/image-20180928171949044.png)

则为成功。