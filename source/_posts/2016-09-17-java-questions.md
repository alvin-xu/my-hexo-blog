---
layout: post
title:  "Java 问题记录"
date:   2016-09-17
excerpt: "记录Java相关的一些琐事问题，供参考"
tags: 
  - java
comments: true
---

> 记录Java相关的一些琐事问题，供参考

**Spring中使用classpath*加载配置文件，jar包中的配置文件不加载问题**

> jar中配置文件放在“/”(根目录)下时，通过classpath*加载配置文件，jar包中的配置文件不会加载，
这是因为Spring使用classpath加载配置文件时需要借助JDK的ClassLoader.getResources(String name)方法，而该方法有一个局限：当传入的参数为空字符串时，即我们本意是想从根目录获取文件，这时JDK只会返回存在于文件系统中的资源，而在jar包中的资源并不会被返回。

> 解决方法是将配置文件放在根的下一级目录内，例如/conf/application-context.xml，web.xml中配置为classpath*:conf/**/*application-context.xml。

***

__web.xml中servlet匹配的url-pattern怎么配？试过/*可以匹配到Restful的Controller,但不能访问静态资源；『/』都可以匹配到。/** 则只能匹配静态资源，RESTful的controller匹配不到（404错误）。不理解，真正的规则到底是怎么样的？__

`/`匹配到controller，不能静态资源；`/*`都匹配不到；`/**`可以静态资源，不能controller；

这篇[url-pattern]问答中提到了部分原理，但还不是很理解。

> If you want to serve .html files, you must add this <mvc:default-servlet-handler /> in your spring config file. .html files are static. Hope that this can help someone

***

__Restful的结果返回需要增加`jackson-databind`的依赖，将对象转化为json数据，否则报错。__

***

__多模块log4j配置问题__

在开发环境中，每个模块可以将自己的配置放在resource下面，这样模块单独调试时可以方便的控制日志等级。 在生产环境中，模块被打包成了jar包，在web模块有一个全局的log4j的配置文件控制所有模块的日志输出情况即可。（不会加载jar包内的模块配置文件）

***

__定为文件资源问题__

参考[class vs classloader .getResource]

*** 

__json库__

- [net.sf.json-lib], 官网显示最后更新是10年，比较旧，而且显示需要依赖较多其他的库。

- org.codehaus.jackson , jackson 库的1.x版本，现在只维护bug，不再更新功能 
- com.fasterxml.jackson, jackson 2.0版本, 改了包名。应该使用这个，参考[GitHub文档]

json-li和jackson还有很大的性能差距，看这篇普通的[性能测评文章].

![测评](http://img.codfly.com/16-9-10/64435577.jpg)

***

__Spring-data-redis, 序列化类型选择__

参考[对象序列化比较1]/[对象序列化比较2]这两篇文章。

key使用String、Value使用Jackson

***

__多模块property-placeholder问题__

在多个模块都有配置文件的情况下，使用多个，启动报错，找不到某个配置文件的属性。
```
    <context:property-placeholder location="classpath*:conf/jdbc.properties"/>
```

需要增加一个配置项, 如下，忽略解析不了的情况。
```
    <context:property-placeholder location="classpath*:conf/jdbc.properties" ignore-unresolvable="true"/>
```

***

__Spring MVC测试报错__

集成测试，启动报错为：`java.lang.ClassNotFoundException: javax.servlet.SessionCookieConfig`

网上查到是由于项目引用的servlet版本过低导致。需要升级到3.0或以上。注意servlet-api从3.1开始构件名称改了！

```
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
	<version>3.1.0</version>
	<scope>provided</scope>
</dependency>
```

***

__好友关系数据结构怎么设计？__

参考[知乎问题](https://www.zhihu.com/question/20216864)

> 好友关系要分两种，强好友关系（你是他好友，他必须也是你好友比如QQ），和弱好友关系（你可以关注他，但他不一定关注你比如微博）， 如果是强关系那么 <1,2>与<2,1>其实是一样的，但是如果是弱的<1,2>与<2,1>就不一样了，至于要设计成什么样的，当然看需求啦！

作者：杜朋
链接：https://www.zhihu.com/question/20216864/answer/92695981
来源：知乎
著作权归作者所有，转载请联系作者获得授权。

***

__Spring MVC 跨域访问问题__

Cross-origin resource sharing (CORS) ，现在大部分浏览器都有实现该模式（通用规范）。

可以通过设置某个资源（答复）的头部“Access-Control-Allow-Origin”字段表示允许的跨域请求。

找到两个官方文档：[Spring MVC 中设置全局跨域](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#_global_cors_configuration), [Spring Security 跨域设置](http://docs.spring.io/spring-security/site/docs/4.1.3.RELEASE/reference/htmlsingle/#cors)

但经过初步尝试，Spring MVC中的全局配置并没有生效，不知道什么原因。倒是单独使用Spring Security的配置生效了。参考了部分内容，[Spring Data Rest and Cors](http://stackoverflow.com/questions/31724994/spring-data-rest-and-cors).

***

__HTTP 自定义 Header 中文值报错__

> SyntaxError: Failed to execute 'setRequestHeader' on 'XMLHttpRequest': '工厂A:1481172518553:0656d2df4d1fd1c214350b98a1c70e65' is not a valid HTTP header field value.

原因：
[stackoverflow](http://stackoverflow.com/questions/34670413/regexp-to-validate-a-http-header-value)

***

__Mybatis 一对多查询结果分页问题__

使用的Pagehelper库不能处理嵌套查询的分页情况。必须手动采用`子查询分页`.
参考[博客园文章](http://www.cnblogs.com/dreamroute/p/4479187.html)

[class vs classloader .getResource]: http://blog.csdn.net/zhouysh/article/details/5889564
[url-pattern]: http://stackoverflow.com/questions/4140448/difference-between-and-in-servlet-mapping-url-pattern
[net.sf.json-lib]: http://json-lib.sourceforge.net/
[性能测评文章]: http://blog.163.com/linfenliang@126/blog/static/127857195201382173845516/
[GitHub文档]: https://github.com/FasterXML/jackson-databind/
[对象序列化比较1]: http://www.voidcn.com/blog/hotdust/article/p-6084492.html
[对象序列化比较2]: http://shift-alt-ctrl.iteye.com/blog/1887370