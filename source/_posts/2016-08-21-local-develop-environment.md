---
layout: post
title:  "Maven本地Web开发环境搭建"
date:   2016-08-21
excerpt: "基于Maven的本地Java Web开发环境搭建指引"
tags: 
  - java
  - maven
comments: true
---

> 基于Maven的本地Java Web开发环境搭建指引. 使用OS X EI Capitan系统.

## 基础工具

### 1 JDK 安装

[jdk 下载地址]

![jdk](http://img.codfly.com/16-8-21/44130731.jpg)

项目统一使用1.8版本, 安装完成后，配置环境变量.

命令行输入`java -version`命令验证:

![java-v](http://img.codfly.com/16-8-21/17672179.jpg)

mac下配置JAVA_HOME, 对`~.bash_profile`文件添加PATH变量，如下图：

![javahome](http://img.codfly.com/16-8-21/5520164.jpg)

### 2 maven 安装

[maven下载地址]

![maven down](http://img.codfly.com/16-8-21/16460014.jpg)

绿色版, 解压到指定目录后，配置环境变量。

命令`mvn -version`验证:

![mvn-verison](http://img.codfly.com/16-8-21/84563308.jpg)

配置文件修改, 默认的maven仓库路径可能无法访问，可尝试修改安装目录下`conf/settings.xml`配置文件的最后部分如下：

``` xml
  <profile>
      <id>http</id>
      <repositories>
	  <repository>
	    <id>central</id>
	    <name>Maven Repository</name>
	    <layout>default</layout>
	    <url>http://repo1.maven.org/maven2</url>
	    <snapshots>
	      <enabled>false</enabled>
	    </snapshots>
	  </repository>
        </repositories>
      </profile>
  </profiles>
  <activeProfiles>
    <activeProfile>http</activeProfile>
  </activeProfiles>
```
    
### 3 tomcat 安装

[tomcat 下载地址]

![tomcat down](http://img.codfly.com/16-8-21/73060279.jpg)

绿色版，解压到指定目录即可.

### 4 编译器

推荐使用IntelliJ IDEA, [IDEA 下载地址]. 使用Ultimate，参考[IDEA 破解].

### 5 git客户端

可以使用source tree，也可以直接用命令行，IDEA内部也有集成git功能.

## 快速开始

1. git check out项目
2. IDEA配置检查，maven、git、tomcat

![idea config](http://img.codfly.com/16-8-21/3828631.jpg)

maven配置为之前的安装目录

![maven install](http://img.codfly.com/16-8-21/83208727.jpg)

tomcat服务器配置

![tomcat install](http://img.codfly.com/16-8-21/93733553.jpg)

3. IDEA导入maven项目

![idea import](http://img.codfly.com/16-8-21/33965261.jpg)

4. 新建服务器，运行项目

![idea run](http://img.codfly.com/16-8-21/16907226.jpg)

5. 浏览器验证

![browser verify](http://img.codfly.com/16-8-21/40554054.jpg)

## 注意事项

1. tomcat服务器部署配置

[maven下载地址]: http://maven.apache.org/download.cgi
[jdk 下载地址]: http://www.oracle.com/technetwork/cn/java/javase/downloads/index.html
[tomcat 下载地址]: http://tomcat.apache.org/download-90.cgi
[IDEA 下载地址]: https://www.jetbrains.com/idea/#chooseYourEdition
[IDEA 破解]: http://idea.lanyus.com/
