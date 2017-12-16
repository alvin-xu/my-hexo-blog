---
layout: post
title:  "Web Server and APP Server Architecture"
date:   2016-08-15
excerpt: "了解Web服务器和APP服务器的架构设计"
tags: 
  - architecture
  - java
comments: true
---

> 了解Web服务器和APP服务器的架构设计

Varnish
Django, Python
自动化运维（DevOps）

## RESTful API

### 版本问题

> 无论你正在构建什么，无论你在入手前做了多少计划，你核心的应用总会发生变化，数据关系也会变化，资源上的属性也会被增加或删除。只要你的项目还活着，并且有大量的用户在用，这种情况总是会发生。

>请谨记一点，API是服务器与客户端之间的一个公共契约。如果你对服务器上的API做了一个更改，并且这些更改无法向后兼容，那么你就打破了这个契约，客户端又会要求你重新支持它。为了避免这样的事情，你既要确保应用程序逐步的演变，又要让客户端满意。那么你必须在引入新版本API的同时保持旧版本API仍然可用。

>注：如果你只是简单的增加一个新的特性到API上，如资源上的一个新属性或者增加一个新的端点，你不需要增加API的版本。因为这些并不会造成向后兼容性的问题，你只需要修改文档即可。

>随着时间的推移，你可能声明不再支持某些旧版本的API。申明不支持一个特性并不意味着关闭或者破坏它。而是告诉客户端旧版本的API将在某个特定的时间被删除，并且建议他们使用新版本的API。

>一个好的RESTful API会在URL中包含版本信息。另一种比较常见的方案是在请求头里面保持版本信息。但是跟很多不同的第三方开发者一起工作后，我可以很明确的告诉你，在请求头里面包含版本信息远没有放在URL里面来的容易。

Q: 使用RESTful API来设计，那么版本迭代的过程中，API是怎么变的呢？怎么变才能平滑过渡？代码的升级又是如何？

A: 目前仅知可以在URL中指定版本号。

Q: Spring MVC 对于RESTful API的支持情况？



### https? 

## 微服务

## 持久化框架 Mybatis vs Hibernate

参考了网上的比较，认为Mybatis是更优的选择，基于以下几点：

1. 数据结构本身可能会很复杂，强行使用Hibernate的ORM映射，比较难处理；（技术积累不够）
2. Mybatis自己写SQL语句的方式，对于后期数据部分的维护和调优很有优势；
3. 移植性方面，Mybatis因为跟SQL语句绑定了，想要移植到其他数据库是很麻烦的，但其实移植数据库的可能性很低，可以不考虑；

那接下来要做的就是了解清楚Mybatis的一些细节：

1. 怎么配合Spring进行依赖注入
2. 事务是怎么处理的
3. 具体查询怎么完成，连接查询，结果集，分页排序功能等
4. 有没可能兼容多种数据源，比如同时用MySQL和MongoDB

## Redis

[Redis+Spring缓存实例（windows环境，附实例源码及详解）] 文章中讲解了一种整合Redis的方式，就是使用Spring的AOP，对所有方法的结果进行缓存。

有没有其他的使用方式呢？

缓存的整体策略请参考[缓存策略].

> ehcache直接在jvm虚拟机中缓存，速度快，效率高；但是缓存共享麻烦，集群分布式应用不方便。
redis是通过socket访问到缓存服务，效率比ecache低，比数据库要快很多，处理集群和分布式缓存方便，有成熟的方案。
如果是单个应用或者对缓存访问要求很高的应用，用ehcache。
如果是大型系统，存在缓存共享、分布式部署、缓存内容很大的，建议用redis。
补充下：ehcache也有缓存共享方案，不过是通过RMI或者Jgroup多播方式进行广播缓存通知更新，缓存共享复杂，维护不方便；简单的共享可以，但是涉及到缓存恢复，大数据缓存，则不合适

## APP登录、会话

> 由于APP向服务端发起请求属于跨域访问，每次访问在服务端都会产生一个新的session，因此APP客户端与web端不同，无法通过session来保持登录状态。 
为了维护app用户的登录状态，我们可以利用token来实现。 
客户端输入账号密码，发起登录请求，服务端在登录接口验证通过后，给客户端返回一个任意字符串，既token，生成算法可随机，token必须与用户的账户关联，如用userid和token形成键值对，保存在内存中（redis）。客户端拿到这个token后，就相当于被服务端承认正常登录成功了，在之后所有需要验证的请求中，带上token，服务端验证token是否存在，是否有效。 
出于安全考虑，token在每次登录时重新生成，并可以设置有效期，每次有效操作后更新token的时间戳，保证token有效期往后延续。 
为了避免token被截获，伪造非法请求，在每次请求时，可以用userid+token+时间戳+密钥+请求参数，进行签名，服务端验证token，同时验证签名，以保证请求的安全性。

## APP消息安全性

## 参考

- [携程移动App架构优化之旅]
- [App架构设计经验谈:接口的设计]

- [理解本真的REST架构风格]
- [RESTful API 设计指南] 阮一峰文章，讲解的很清晰！
- [好RESTful API的设计原则]
- [来自于PayPal的RESTful API标准]

- [组件化、模块化、集中式、分布式、服务化、面向服务的架构、微服务架构]
- [微服务（Microservice）那点事]

- [Mybatis与Hibernate的详细对比]
- [MyBatis vs. Other ORMs]

- [分布式缓存 Redis 使用心得]
- [高性能服务器架构思路]
- [缓存更新的套路]


[携程移动App架构优化之旅]: http://www.infoq.com/cn/articles/ctrip-app-architecture
[App架构设计经验谈:接口的设计]: http://android.jobbole.com/82512/
[理解本真的REST架构风格]: http://www.infoq.com/cn/articles/understanding-restful-style  
[RESTful API 设计指南]:http://www.ruanyifeng.com/blog/2014/05/restful_api.html
[组件化、模块化、集中式、分布式、服务化、面向服务的架构、微服务架构]: http://www.hollischuang.com/archives/1628
[来自于PayPal的RESTful API标准]: https://segmentfault.com/a/1190000005924733
[好RESTful API的设计原则]: http://www.cnblogs.com/moonz-wu/p/4211626.html
[微服务（Microservice）那点事]: https://yq.aliyun.com/articles/2764
[Mybatis与Hibernate的详细对比]: http://blog.csdn.net/jiuqiyuliang/article/details/45378065
[MyBatis vs. Other ORMs]: https://archfirst.org/mybatis-vs-other-orms/
[Redis+Spring缓存实例（windows环境，附实例源码及详解）]: http://blog.csdn.net/u013142781/article/details/50515320
[缓存策略]: https://www.zhihu.com/question/27738066
[分布式缓存 Redis 使用心得]: http://gold.xitu.io/entry/56baa0cfc4c97100522945d3
[高性能服务器架构思路]: http://wetest.qq.com/lab/view/?id=80
[缓存更新的套路]: http://coolshell.cn/articles/17416.html

