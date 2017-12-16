---
layout: post
title:  "RESTful API 实践"
date:   2016-08-25
excerpt: "基于Spring MVC的RESTful API实现方案"
tags: 
  - java
  - RESTful API
comments: true
---

> 基于Spring MVC的RESTful API实现方案.

# 版本控制

使用的url中带版本信息的方案，这样较为直观。需要解决一个重要的实际问题就是：*如果部分API升级，而旧版的API又还在使用中，不能废除的情况怎么办？* 

主要参考[让SpringMVC支持可版本管理的Restful接口], 设计了一个自定义的HandlerMapping，通过注解的方式来区分不同版本api，可以做到向下兼容旧版API。也就是说，如果只有v1的api，就算使用v2的路径也可以自动匹配到v1的api。

与原文不同的是，我还是想使用xml的配置方式，因此放弃了使用`<mvc:annotation-driven/>`标签，因为这个标签会自动加载许多默认的组件，其中就包括一个`handleMapping`，这样就无法使用我们自定义的了。改成所有要用到的组件都要自行在spring配置文件中声明，其实更不容易搞错，掌控性也更强。

# 身份认证

OAuth 2.0

# https

需要web服务器的配置支持，TLS属于传输层安全协议，应该还需要第三方的CA证书机构认证。后续跟进。

# 参数过滤

# 结果返回

## 操作成功的情况

- 状态码直接通过Controller的方法中使用`@ResponseStatus(HttpStatus.NO_CONTENT)`答复对应的状态码；
- 内容根据规范该返回实体返回实体，该空文档的空。

## 请求失败的情况

Spring MVC默认处理的请求失败情况框架已经帮我们做了。参看[Handling Standard Spring MVC Exceptions]. 说说自定义的失败情况，比如找不到内容、参数检查错误等。

- 通过Spring MVC的异常机制来完成，在程序中出错的地方直接抛出对应的异常，一般为自定义的。
- 这些异常可能还需要返回一些指定的信息给客户端，另外每种异常对应的HTTP状态码可能不一样，因此需要一个地方集中处理这些异常转化为响应。就要使用到Spring MVC中的`@ControllerAdvice`. 示例代码如下。 这样头部的状态码和body的内容ErrorTip都有了~

``` java
@ControllerAdvice
public class WebExceptionHandler extends ResponseEntityExceptionHandler{

    @ExceptionHandler(ParamException.class)
    @ResponseBody
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ErrorTip handleParamException(ParamException ex) {
        return new ErrorTip(ex.getMessage() == null ? "param error" : ex.getMessage());
    }

}
```

注意： 这里异常到响应的处理，需要配置`ExceptionHandlerExceptionResolver`. 当然如果有用`<mvc: annotation-driven/>`那会有默认的配置，就不需要单独配置了。参看[spring 文档].

若是需要对Spring MVC的默认异常处理重新定义，可以扩展`ExceptionHandlerExceptionResolver`类来实现。

# 参考

- [REST APIs must be hypertext-driven] 员作者的声明！
- [GitHub api v3] GitHub的实践

[wikipedia SSL/TLS介绍]: https://zh.wikipedia.org/wiki/%E5%82%B3%E8%BC%B8%E5%B1%A4%E5%AE%89%E5%85%A8%E5%8D%94%E8%AD%B0
[SSL/TLS协议运行机制的概述]: http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html
[https 从原理到实践]: https://github.com/wxyyxc1992/infrastructure-handbook/blob/master/Network/Protocol/HTTP/HTTPS/HTTPS.md
[我所认为的RESTful API最佳实践]: http://www.scienjus.com/my-restful-api-best-practices/
[REST APIs must be hypertext-driven]: http://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven
[GitHub api v3]: https://developer.github.com/v3/
[Handling Standard Spring MVC Exceptions]: http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc-ann-rest-spring-mvc-exceptions
[spring 文档]: http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc-config-enable
[让SpringMVC支持可版本管理的Restful接口]: http://www.cnblogs.com/jcli/p/springmvc_restful_version.html