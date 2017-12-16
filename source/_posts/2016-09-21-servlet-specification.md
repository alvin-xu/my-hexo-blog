---
layout: post
title:  "Servlet 规范"
date:   2016-09-21
excerpt: "了解Servlet规范，知道哪些蛋疼的配置文件都怎么规定的，什么含义"
tags:
  - java
comments: true
---

> 了解Servlet规范，知道哪些蛋疼的配置文件都怎么规定的，什么含义.

## 来源

Java规范在[Java Community Process]上可以查到。这个网站的所有参与者也会负责规范的制定更新。

网站从规范所属的平台（EE，SE）、相关技术、提交者等不同维度来划分规范。

查询servlet后，出来好多版本，选择哪个版本？ 在实际应用中又应该选择哪个使用呢？

发现一个翻译资源：[Java Servlet 3.1 翻译]. 可以看中文的不错。

## Servelt3.1 规范

原来web.xml是『web应用部署描述符』（Deployment Descriptor)

## Servlet 版本选择问题

参考这篇[Servlet 3 vs 2.5]问答，其中提到3版本具有一些新功能包括：
- 支持注解配置servlet、fileter、listener这些
- 可以模块化的web.xml描述文件
- 请求响应支持异步模式！（感觉这个会极大程度提升性能呀）

那不是应该用最新的稳定的规范才对嘛？！

但是web.xml文件的头部，不同版本的是不一样的，哪里有说明呢？ 只找到一个[非官方链接](https://www.mkyong.com/web-development/the-web-xml-deployment-descriptor-examples/). 看来有空得了解下xml头部哪些xmlns、xsi、xsd等等的内容。。。

规范中只有一句描述感觉相关的：
> The deployment descriptor for this revision of the specification is available at http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd

## 专有名词

- JSR，Java Specification Request，就是指Java规范
- JCP，Java Community Process

[Java Community Process]: https://jcp.org/en/home/index
[Java Servlet 3.1 翻译]: https://www.gitbook.com/book/waylau/servlet-3-1-specification/details
[Servlet 3 vs 2.5]: http://stackoverflow.com/questions/1638865/what-are-the-differences-between-servlet-2-5-and-3