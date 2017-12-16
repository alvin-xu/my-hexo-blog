---
layout: post
title:  "WEB Technology Introduction"
date:   2016-08-05 11:24:25 +0800
excerpt: "前端框架概览学习,包括Angular、Vue、Backbone、Regularjs..."
tags: 
  - web
comments: true
---

# 前端框架概要

# Regularjs
网易开发的前端框架，用他们的话说：

>Regularjs是基于动态模板实现的用于构建数据驱动型组件的新一代类库

[github介绍][1]

通过 模板+html（view） <---> js（data） 的方式来构成**组件**.

>regularjs中，组件被拆分为了：模板template + 数据data + 业务逻辑(实例函数)的组合，每一份组件可以视为一个小型的mvvm系统。

js部分，组件数据和行为的封装很清晰。
view部分，数据的绑定通过模板中的变量来实现。

view和js的交互，通过模板中的事件定义来实现，耦合在html代码中。可以通过计算属性定义配合内置的r-model属性来简化。
因为上一点的存在，所以带来的是js中代码的简洁，没有任何dom操作！或者说regularjs中将dom的操作通过模板来控制，而模板的变化又是跟数据的动态变化直接相关。这应该就是他们说的`数据驱动`的意思了。
这个与backbone不一样，backbone中需要自己在js中操作dom，比较烦。

数据改变直接能反应到模板中，是通过脏检查机制。

#### 优点：
见[Regularjs是什么][2]
比较重要的是，据说兼容ie6

#### 缺点：
1. 没有进行服务器端渲染，也就是说你代码写出来什么样，浏览器直接看到是什么样，模板、组件，一目了然，不好！
2. 目前没有看到有进行『数据与后端url的封装』，难道所有对数据的请求要自己写ajax请求？不好。backbone中有将数据模型与RESTful API进行绑定，个人感觉会比较方便。比如backbone：

```javascript
var Books = Backbone.Collection.extend({
  url: '/books'
});

GET  /books/ .... collection.fetch();
POST /books/ .... collection.create();
GET  /books/1 ... model.fetch();
PUT  /books/1 ... model.save();
DEL  /books/1 ... model.destroy();
```

3. 本身不具有路由功能，好像可以使用regular-state。看这个[issue][3],最近好像要更新，连服务器端渲染也要有了。
4. 在view中有大量的模板内容存在，与数据强相关，这样前端人员的职能是不是就不好分离了呀！！这样前端人员也需要去了解数据结构，与后台的交互，我本意前端人员不需要太了解业务的。是不是目前主流的前端框架都是这样的？？？

# Backbone.js
[官网][4]
非常轻量级的前端mvc框架，脚本才7.6kb大小。需要强制依赖Underscore库来进行模板的加载。

通过查看[TODO注释范例][5]，可以较好的理解该库的基本使用模式。

**Model** 和 **Collection**用来表示数据，并提供对数据的基本操作，查询（where）、保存（save）等。数据模型的中有一个亮点值得一提，那就是`把RESTful API`进行了集成，可以很方便的通过api进行和服务器通信！如下：

```javascript
var Books = Backbone.Collection.extend({
  url: '/books'
});

GET  /books/ .... collection.fetch();
POST /books/ .... collection.create();
GET  /books/1 ... model.fetch();
PUT  /books/1 ... model.save();
DEL  /books/1 ... model.destroy();
```

**View** 是数据和页面进行交互的纽带。在View中将模板关联到页面的某一元素：

```javascript
el: $("#todoapp"),
statsTemplate: _.template($('#stats-template').html())
```

而数据，即上面提到的Collection和Model，是在外部就创建了实例，在View内部直接使用的。

比如在外部使用`var Todos = new TodoList;`创建了一个Todos的Collection。那么在View内部需要自行做两件事，
一个是当数据修改时保证更新到页面，在View初始化时做：

```javascript
this.listenTo(Todos, 'add', this.addOne);
this.listenTo(Todos, 'reset', this.addAll);
this.listenTo(Todos, 'all', this.render);
```

另一个是当页面有用户输入时，信息更新到模型数据，需要在View中定义好监听事件：

```javascript
events: {
      "keypress #new-todo":  "createOnEnter",
      "click #clear-completed": "clearCompleted",
      "click #toggle-all": "toggleAllComplete"
},
```

### 总结
所以，通过观察其最简单的一个TODO的例子可以知道，这个框架确实非常轻量，轻量到感觉并没有起到很好的简化作用，感觉更像了强行规定了数据视图交互的一个规范、一个骨架，剩下的所有事都得开发者自己做，按照规定来，开发者依旧要通过操作dom来将数据更新到页面（在render中预定义, 在initialize中监听数据）。

这样，写起组件来会很麻烦吧。

# Avalon
[官网][6]
国人佳作，**司徒正美**出品，去哪儿网前端架构师。

能够兼容ie6，并且不是使用的**脏检查**机制，而是通过`defineProperty`来实现的双向绑定，似乎**Vue.js**也是使用类似的机制。这种机制性能据说会比**脏检查**好，但在低版本的ie中不兼容。。。但作者针对低版本ie使用了其他机制来实现！

可惜的是，其官网的教程页面似乎有点问题。暂时没有细看，但使用上应该跟Angular js区别不大。

# Vue.js
[官网][7]
谷歌大神[尤雨溪][8]作品，同样是国人翘楚呀！大赞，网评很高，代码简洁美观。看看一点简单的[介绍][9].

>Vue.js（读音 /vjuː/, 类似于 view）是一个构建数据驱动的 web 界面的库。Vue.js 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。

>Vue.js 自身不是一个全能框架——它只聚焦于视图层。因此它非常容易学习，非常容易与其它库或已有项目整合。另一方面，在与相关工具和支持库一起使用时，Vue.js 也能完美地驱动复杂的单页应用。

这个库资料很全面，github上关注度也仅次于Angular，有许多实用这个库构建的炫酷的站。并且声称入门很容易。

很有趣的是，作者很详细的说明了[Vue与其他主流框架对比][10]

#### 兼容性
也只是兼容到ie9

![Alt text](http://img.codfly.com/16-8-13/80229839.jpg)

# Angular js
[官网][11]
HTML很适合静态文档的显示。但对于web应用中动态视图很勉强(只能通过修改DOM）。AngularJS能够扩展HTML，达到更好的展现、可读和快速开发的目的。

Google出品，必属精品呀。简单试用了下1.x版本。通过指令来实现模板和控制器的分离，具有较好的组件化的模式。通过脏检查来实现的双向绑定。基本该有的都有了。并且资料很完善，使用者众多，是现在非常流行的前端框架！github上关注度也是所有同类框架中最高的。

但功能太全面，完善，往往也带来不够灵活的特点，笨重，脏检查效率低等，被众多网友吐槽的地方。

#### 兼容性
1.3之后最低兼容ie9。参考[官方说明][12].
1.2说是会兼容ie8，但好像不准备花太多精力维护。。。

> AngularJS 1.2 will continue to support IE8, but the core team does not plan to spend time addressing issues specific to IE8 or earlier.

这是致命问题，现在非常流行好用的框架似乎都淘汰ie8以下了。。。Vue也是.

# Angularjs 2
[官网][13]

### 参考
[一个对前端模板技术的全面总结][14]


  [1]: http://regularjs.github.io/guide/zh/index.html
  [2]: http://regularjs.github.io/guide/zh/intro/what.html
  [3]: https://github.com/regularjs/regular/issues/83
  [4]: http://backbonejs.org/
  [5]: http://backbonejs.org/docs/todos.html
  [6]: https://avalonjs.github.io/
  [7]: https://vuejs.org.cn/
  [8]: http://baike.baidu.com/view/7942212.htm
  [9]: http://www.html-js.com/article/The-frontend-stew-stew-author-You-Xiaoyou-Vuejs-interview-official
  [10]: https://vuejs.org.cn/guide/comparison.html
  [11]: https://angularjs.org/
  [12]: https://docs.angularjs.org/guide/ie
  [13]: https://angular.io/
  [14]: http://www.html-js.com/article/2313
