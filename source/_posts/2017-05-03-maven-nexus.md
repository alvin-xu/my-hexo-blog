---
layout: post
title:  "Maven私服搭建"
date:   2017-05-03 14:44:25
excerpt: "记录Maven搭建私服的过程"
tags:
  - java
comments: true
---

> 记录Maven搭建私服的过程

## Maven私有服务器是什么
Maven中央仓库我们都很熟悉，上面存储了基本上所有的Java开源库的Jar包，我们需要用到某个开源库(比如Spring）则在项目的pom配置文件中增加该开源库的依赖配置就可以了。本地Maven工具会帮我们自动把远程中央仓库的Jar包下载到本地，供项目使用。

但远程的中央仓库会有网络不稳定，下载速度慢的问题。并且，如果公司内部有一些公共的Jar包是不能传到网络上开源的，这个时候开源的中央仓库就无法起作用。

此时，就需要Maven的私有服务器。一般来讲，都搭建在公司局域网内。相当于局域网内的另外一个中央仓库。使用者要使用Jar包时，都首先从该私服下载。局域网内速度快。另外，公司内部不宜公开的通用Jar包也可以传到私有服务器进行共享。

## 可以用来搭建Maven私有服务器的软件有哪些

- [Nexus Repository OSS], _(OSS，Open Source Software)_

> Store and distribute parts accross your software supply chain including: Java, npm, Docker, RubyGems, PyPI, NuGet, Bower, and more.

可以存储分发软件的供应链部分，包括Java，npm等。其实就是统一存储分发软件依赖。

有详细的说明文档：[nexus-book]

Nexus的声誉实在太高，以至于都不用查看比较同类软件，就可以放心选用。

- [Archiva], Apacha旗下软件，应该只能用来管理Java的Jar包.

## 搭建过程



[Nexus Repository OSS]: https://www.sonatype.com/nexus-repository-oss
[nexus-book]: http://books.sonatype.com/nexus-book/reference/index.html
[Archiva]: http://archiva.apache.org/index.cgi
