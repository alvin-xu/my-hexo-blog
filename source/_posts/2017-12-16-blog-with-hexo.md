---
layout: post
title:  "使用Hexo生成博客"
date:   2017/12/16 16:00:00
excerpt: "想把博客整顿下，重新上路，这次整高级点，好好写，写到写不动为止！"
tags: 
  - hexo
  - blog
comments: true
---

## 选择Hexo作为静态博客生成工具的原因
- 主题多，好看
- 支持目录
- 使用nodejs来生成静态博客，速度快

## 过程

1. 系统需要有nodejs和git。此处安装省略。

2. 使用nodejs的包管理工具npm，安装hexo
	`npm install -g hexo-cli`, mac下注意全局安装需要加sudo。

	npm要是很慢，半天没动静，那就重新配置个repository，`npm config set registry https://registry.npm.taobao.org`. 配置好后使用`npm config get registry`来验证。

3. 初始化hexo
	`hexo init <folder>`, 文件夹替换为准备放置自己本地博客的目录。
	这个命令会从hexo的master分支上clone必要的配置文件到本地，并且自动安装nodejs依赖。

4. 迁移jekyll中的文章到hexo
  将jekyll目录下的所有文章（_post目录）直接拷贝到hexo的`source/_posts`目录下。然后记得修改`_config.yml`文件：
  `new_post_name: :year-:month-:day-:title.md`
  让文章标题符合生成规则。

5. 启动hexo-server本地预览博客
   需要另外在本地博客目录下安装nodejs库：hexo-server
   
   `npm install hexo-server --save`

   然后使用命令`hexo server`启动服务器，默认运行在4000端口。成功的话，本地浏览器`http://localhost:4000`就能看到效果了。 
   要是启动报错了，就够呛了。。。一般都没有提示具体哪里错了。。。官方有问题页面，自行排查吧。

6. 部署配置
  暂时部署到github上，使用[github pages]来运行博客。所以hexo配置文件中内容：
  ```
  deploy:
    type: git
    repo: git@github.com:alvin-xu/alvin-xu.github.io.git
    branch: master
  ```
  使用git进行自动部署，先安装依赖：`npm install hexo-deployer-git --save` 
  然后直接`hexo deploy`命令。该命令会把`public`目录下的文件直接部署到上面配置的仓库中。这样就完成了网站发布。

  注意，部署之前一般会先生成最新的网站：`hexo generate`

  我其实有点想法部署到自己的服务器上。源码放github上。这样的话，应该要考虑使用github的Webhooks机制，在每次提交代码都触发服务器自动去更新网站。有需要再弄。

## 一些命令

  `hexo new page about`, 在hexo跟目录执行，会在source目录下，新建一个带有index.md的文件夹，新页面的内容添加到此处即可。
  
  标签页也需要自行添加，`hexo new page tags`, 页面内容：
  ```
  ---
  title: 标签
  date: 2017-12-16 20:29:54
  type: "tags"
  comments: false
  ---
  ```

## 问题

启动报错：

```
INFO  Start processing
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Template render error: (unknown path) [Line 4, Column 2]
  unknown block tag: highlight
```

## 参考
- [hexo]


[hexo]:https://hexo.io/
[github pages]:https://pages.github.com/

