---
layout: post
title:  "Java Server Security"
date:   2016-08-14
excerpt: "调研目前主流的Java认证和授权的框架,对比选型。了解认证和授权的基本原理。"
tags: 
  - java
comments: true
---

* 目录
{:toc}

> 调研目前主流的Java认证和授权的框架,对比选型。了解认证和授权的基本原理。

# HDIV

[HDIV](https://hdivsecurity.com/)号称解决了OWASP上的前10个web安全问题！并且能够很好的兼容Web框架，比如Spring MVC。有社区开源版、专业版和企业版之分，后面两个收费。

> Real-time, runtime application self-protection that’s easy to implement. Get the best protection against OWASP Top 10 threats with a future-proofed, open source solution.

# Shiro

# Spring Security

[Spring Security 官网]

> Security is an ever-moving target, and it’s important to pursue a comprehensive, system-wide approach. In security circles we encourage you to adopt "layers of security", so that each layer tries to be as secure as possible in its own right, with successive layers providing additional security. The "tighter" the security of each layer, the more robust and safe your application will be. At the bottom level you’ll need to deal with issues such as transport security and system identification, in order to mitigate man-in-the-middle attacks. Next you’ll generally utilise firewalls, perhaps with VPNs or IP security to ensure only authorised systems can attempt to connect. In corporate environments you may deploy a DMZ to separate public-facing servers from backend database and application servers. Your operating system will also play a critical part, addressing issues such as running processes as non-privileged users and maximising file system security. An operating system will usually also be configured with its own firewall. Hopefully somewhere along the way you’ll be trying to prevent denial of service and brute force attacks against the system. An intrusion detection system will also be especially useful for monitoring and responding to attacks, with such systems able to take protective action such as blocking offending TCP/IP addresses in real-time. Moving to the higher layers, your Java Virtual Machine will hopefully be configured to minimize the permissions granted to different Java types, and then your application will add its own problem domain-specific security configuration. Spring Security makes this latter area - application security - much easier.

> Of course, you will need to properly address all security layers mentioned above, together with managerial factors that encompass every layer. A non-exhaustive list of such managerial factors would include security bulletin monitoring, patching, personnel vetting, audits, change control, engineering management systems, data backup, disaster recovery, performance benchmarking, load monitoring, centralised logging, incident response procedures etc.

来自手册的这段话，基本把系统安全的不同层次点出来了。可提供方向性的参考。

> In particular, if you are building a web application, you should be aware of the many potential vulnerabilities such as cross-site scripting, request-forgery and session-hijacking which you should be taking into account from the start. The OWASP web site (http://www.owasp.org/) maintains a top ten list of web application vulnerabilities as well as a lot of useful reference information.

又学到一点。开始构建网站时，就应该考虑可能受到的攻击了。参考[OWASP]，里面罗列了web网站最易受到的攻击类型。

## 认证

支持多种认证模型，或者说认证方式，比如http、LDAP等

## 授权

主要有三种授权模式：

- 对web请求进行授权(通过Filter)
- 授权方法是否能被执行(通过AOP)
- 授权是否能访问domain对象实例（通过AspectJ）

## Spring配置，命名空间

- Web/HTTP Security
- Business Object (Method) Security 
- AuthenticationManager
- AccessDecisionManager
- AuthenticationProviders
- UserDetailsService


## 跨域伪造请求

[Cross Site Request Forgery (CSRF)] , 跨域伪造请求的防范，对于所有需要update state的HTTP请求，需要包含额外的一个随机token，服务器端将会校验该token的合法性。（token的保密性怎么保证呢？）

> We can relax the expectations to only require the token for each HTTP request that updates state. This can be safely done since the same origin policy ensures the evil site cannot read the response. Additionally, we do not want to include the random token in HTTP GET as this can cause the tokens to be leaked.

> CSRF 主流防御方式是在后端生成表单的时候生成一串随机 token ，内置到表单里成为一个字段，同时，将此串 token 置入 session 中。每次表单提交到后端时都会检查这两个值是否一致，以此来判断此次表单提交是否是可信的。提交过一次之后，如果这个页面没有生成 CSRF token ，那么 token 将会被清空，如果有新的需求，那么 token 会被更新。 攻击者可以伪造 POST 表单提交，但是他没有后端生成的内置于表单的 token，session 中有没有 token 都无济于事。

所有使用浏览器的请求都应该使用上述的防范方式。非浏览器的客户端可以不用。

## 内部实现

`SecurityContextHolder`中存储安全信息的上下文，默认使用`ThreadLocal`模式存储信息，因此同个线程内的所有方法可以共享信息。

在全局范围都可以去获取相关信息

``` java
Object principal = SecurityContextHolder.getContext().getAuthentication().getPrincipal();

if (principal instanceof UserDetails) {
String username = ((UserDetails)principal).getUsername();
} else {
String username = principal.toString();
}
```

上述代码片段中获取到的`principal`对象，其实就是在`UserDetailsService`中返回的对象。

`UserDetailsService`用来返回认证信息，如果要自定义认证流程，去实现`AuthenticationProvider`.

`Authentication`对象，除了包含上述的，还包含`GrantedAuthority`列表，表示具有的所有『角色』.

自定义登陆行为， 参考[Spring Security for a REST API].

# 密码编码

不要使用MD5、SHA这种直接的文本hash算法来加密，因为hash的计算容易，所以有被穷举暴力破解、或者使用网络上流传的『通用字典库』来破解的风险。

如果在原密码中加入与用户相关的『salt』，再进行hash，则破解的难度将大大提升。

Bcrypt, 原理见[How can bcrypt have built-in salts?]. 强大！文中还提到代价因子为10还不够安全，推荐12及以上！

# 其他

遇到一个jsp el表达式未生效的问题。。。google才发现原来servlet2.3 和2.3以前是默认不开启el表达式的，参考[这篇](http://www.cnblogs.com/o-andy-o/archive/2013/05/09/3068741.html)


[Spring Security 官网]: http://projects.spring.io/spring-security/
[OWASP]: http://www.owasp.org/
[Cross Site Request Forgery (CSRF)]: http://docs.spring.io/spring-security/site/docs/current/reference/html/csrf.html
[How can bcrypt have built-in salts?]: http://stackoverflow.com/questions/6832445/how-can-bcrypt-have-built-in-salts
[Spring Security for a REST API]: http://www.baeldung.com/2011/10/31/securing-a-restful-web-service-with-spring-security-3-1-part-3
