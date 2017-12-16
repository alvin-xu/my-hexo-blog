---
layout: post
title:  "My blog is on the way!"
date:   2016-08-05 15:01:25 +0800
excerpt: "终于要开始了，一直想做，却一直懒惰，我的技术脉络，我的修行之路。希望博客的搭建能够给我足够的动力坚持下去！享受生活，奋力奔跑，加油！！！"
tags: 
  - jekyll
  - blog
comments: true
---

#### 给自己的话

> 终于要开始了，一直想做，却一直懒惰，我的技术脉络，我的修行之路。希望博客的搭建能够给我足够的动力坚持下去！**享受生活，奋力奔跑，加油！！！**

## 搭建过程

选择了[GitHub Pages]作为个人博客的载体。毫无悬念，程序猿怎么可以没有一个GitHub Page呢？？？

官网上有很简明的教程说明如何在GitHub上建立Pages的工程。在自己的GitHub下建一个`username.github.io`的仓库，往里面加静态网页就可以了。内容上传到GitHub后，直接访问`http://username.github.io`即可看到Hello World!

> 注意，username一定是自己的GitHub账号的用户名!

### Jekyll配合建站

当然前端弱鸡的我是不会乖乖去学前端怎么写的。。。于是为了方便，GitHub实际上也考虑到了，都送到嘴里了，配合使用Jekyll，傻瓜建站。但模板这种东西，效果肯定是满足不了每个自认为可以当设计师的程序猿的。先用着吧，炫酷狂拽的个人网站还有很长的路要走呀。。。无数血泪史告诉我，如果一开始就想完美，那最后一般都是完蛋=0=

回归正题。 正义的分割线

***

教程GitHub都有，良心呀。参考[Using Jekyll as a static site generator with GitHub Pages]

完成后，使用`bundle exec jekyll serve`命令执行，即可本地127.0.0.1:4000看到效果！另外一个`bundle update github-pages`或者简化的`bundle update`命令需要记住。更新Jekyll时用到，因为GitHub Pages服务器更新可能导致本地的页面部署到GitHub上时出问题。

把整个目录在推送到GitHub的前面提到的个人Pages仓库中, 就可以输网址看到效果了！！！

这里记录过程中遇到的问题。

1. **Gemfile 配置后，使用`bundle install`命令安装bundler开在某一命令上很久**

    Gemfile配置中，网络源的地址是`https://rubygems.org`，作为一个资深码农，对我天朝上国的网络是有着深深的忧虑的。果然卡住了，后面挂了vpn，还是不行。最后借鉴前辈的宝贵经验，替换为`https://ruby.taobao.org`即可！

2. **MarkDown中插入本地图片问题**

    目前是在Jekyll工程的根目录新建了images目录，图片放里面，Jekyll编译时会自动将这个目录复制到网站根目录。所以文章中的路径需要用`/images/xxx.png`。注意，这其实就是网站的绝对路径了。

    但也不是很方便，每次插图、截图还要重命名、保存到指定的目录。。。后续看看是否有更方便的解决方案，也许可以搭配一些MarkDown编辑器也说不定哦->.->

    ---

    测试下，Google Photos的图片分享功能是不是好用。 --不能用
    ![google photos](https://goo.gl/photos/j67ffjgGHdQS3G94A)

    测试，七牛，配合[yokutu.cn](http://www.yotuku.cn/)。挺方便的，截图可以直接张贴到这个网站上，就上传到七牛的服务器了(需要配置自己七牛服务器的秘钥），图片的外链直接生成好了。

    再在七牛内冲值10元开通CDN服务，可以有自定义域名，万网那边再加个子域名`img.codfly.com`的DNS，图片也可以用上自己的域名了，不错，哈哈~~~

    但是，偶然发现『极简图床』这个小工具有bug！有时，传一张图，七牛那边莫名其妙多出几个空白图，几kb大小。期待他们解决~~

    ![qinniu](http://img.codfly.com/16-8-6/28312669.jpg)

    找到两篇文章可以参考,

    [简化markdown写作中的贴图流程](http://www.jianshu.com/p/7bd4e6ed99be)

    [推荐一个稳定而强大的图床-七牛](http://www.jianshu.com/p/5f0d5451ca01)


3. **写这篇文章的时候发现 # 级别的标题比 ## 还小，怎么回事？**

### 自定义域名

_插播一个英语词汇：_  `apex domain 顶级域名`

参考GitHub教程[Using a custom domain with GitHub Pages]

域名我早已准备好了，万网很久之前申请过了，还是那句话，程序猿怎么能没有一个自己的域名呢？？？

两步搞定，

1. 自己GitHub上的Pages的工程设置里面，修改自定义域名
    ![setting](http://img.codfly.com/16-8-13/80229839.jpg)

2. 万网官网去修改DNS转发规则
    ![dns](http://img.codfly.com/16-8-13/33960586.jpg)

不过，DNS规则时好时坏呀，是不是还没更新完成？

可以使用`dig codfly.com +nostats +nocomments +nocmd`命令 和 `dig alvin-xu.github.io +nostats +nocomments +nocmd`命令的结果是否一致来判断DNS规则是否已经更新了。

### Jeklly主题自定义

想要有完全自己风格的网站,这个是绝对少不了的。其实就是实现一套自己的Jeklly模板。

先Mark一下:

> 评论、RSS和流量分析: 对你来说，添加Disqus、feed.xml和Google Analytics不过小菜一碟，只是提醒下别忘了它们～

统计网站的总访问量，使用[stat counter]. 不能统计到每个页面，与Google Analytics类似，有较为完善的访客信息分析。

简单好用的，有[Hit Counter], 每个页面都能统计到, 方便的显示文章访问数量。

国内，还有一个『友盟』做的类似的产品。

### 参考资源

- [kramdown 基本语法]
- [jekyll 原理介绍 好文]
- [Learn Stuff Visually] 这个网站有点酷，意外发现，开源，可以用来做动态演示，后续可能会用到。

[Github Pages]: https://pages.github.com
[Using Jekyll as a static site generator with GitHub Pages]: https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/
[kramdown 基本语法]: http://kramdown.gettalong.org/quickref.html#definition-lists
[Using a custom domain with GitHub Pages]: https://help.github.com/articles/using-a-custom-domain-with-github-pages/
[jekyll 原理介绍 好文]: http://jekyllbootstrap.com/lessons/jekyll-introduction.html
[Learn Stuff Visually]: http://nilclass.com/
[stat counter]: https://zh_cn.statcounter.com/
[Hit Counter]: http://jerryzou.com/posts/introduction-to-hit-kounter-lc/