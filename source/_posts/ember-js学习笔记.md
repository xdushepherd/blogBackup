title: ember.js学习笔记
date: 2015-10-08 21:47:13
tags: [javascript,前端框架,MVC] 
categories: [前端]
---

####背景

作为一个学习欲望极其强烈的码农，有时间就要涉猎一下大家极具争议性的热门技术。之前详细学习了AngularJS的相关概念，也在自己的简书中有所记录，这次，就来看看，在Rails社区中呼声很高的ember.js框架，看很多帖子中提到ember.js框架比AngularJS框架要怎么怎么好之类的话。好奇心的驱使，就在网上找到了ember.js的[入门指南](http://www.emberjs.cn/guides/getting-started/)，花了点时间，了解一下这个框架的相关概念。

<!-- more -->

2011年，Yehuda Katz（他是jQuery项目和Ruby on Rails项目的核心贡献者）发起了EmberJS项目，因此Ember.js与Rails的很多概念上有相似之处。而我之前学习Rails开发框架时间很久，对其的相关概念理解也较为深刻，因此，在理解Ember.js的相关概念的时候也并不困难。

####相关概念

作为一个现代框架，Ember.js框架和其他前端和后端框架一样，都遵循MVC设计模式。在Ember.js中，与V(view)相关的概念有[模板](http://www.emberjs.cn/guides/templates/the-application-template/)和[组件](http://www.emberjs.cn/guides/components/defining-a-component/)；与M(model)相关的概念则是[模型](http://www.emberjs.cn/guides/models/)和[对象模型](http://www.emberjs.cn/guides/object-model/classes-and-instances/)；与C(Controller)相关的概念有[路由](http://www.emberjs.cn/guides/routing/)，[控制器](http://www.emberjs.cn/guides/controllers/)。

#####模板和组件

1. 模板。Ember.js使用[Handlebars模板库](http://handlebarsjs.com/)增强HTML语言能力，便于开发人员渲染数据。在这一点上，和Rails使用的erb，slim等模板语言还有Laravel框架使用的blade模板语言功能相似，使用过这些语言的开发者可以很快上手Handlebars模板语言。


2. 组件。顾名思义，组件是一段可重用的代码，Ember.js也为组件部分设计了一些独有的语法，可以通过阅读有关源代码来进行快速学习。

 #####模型和对象模型

1. 对象模型。对象模型是Ember.js设计的一种创建Javascript类，继承类，实例化的方法，还包括了属性及其相关操概念。

2. 模型。模型是一个定义了需要呈现给用户的数据的属性和行为的类。任何用户往返于应用（或者刷新页面）能看到的内容都需要使用模型来表示。

#####路由器和控制器

1. 路由器。下面是从Ember.js的入门指南中复制的关于路由功能的一些描述:
     1.在任何时刻，你的应用中都会有一个或多个活跃的路由处理器。下列两个条件都可以触发它们：

               用户与视图发生交互，产生事件导致URL改变。

               用户手动改变URL（如: 点击后退按钮），或者是第一次载入页面。

     2.当前的URL发生改变时，活跃的新路由处理器可能会做以下事情：

              根据条件选择跳转到新的URL上

              更新控制器（controller）以便映射到特定的模型（model）上

              更改屏幕（浏览器窗口）上的模板，或者在已存在的出口（outlet）上替换新的模板

2.控制器。控制器用于将显示逻辑与模型绑定在一起。通常模型包含需要保存到服务器端的属性，而控制器的属性可以不需要保存至服务器端。

####总结

1. 从概念上来讲，Ember.js在很多方面和Rails都很相似，当然，Ember.js不需要通过模型对数据库进行操作，而且模型可以通过Ajax获取来自服务器端的数据。粗粗地看过一遍，对其有了一个大概的了解。相信如果日后需要用到Ember.js的时候，可以很快上手把。

2. 与AngularJS对比，由于没有使用过这两个框架进行过开发工作。不过，从初步来看，其实AngularJS的指令系统与Ember.js的组件有类似之处。AngularJS以控制器为核心，而Ember.js则以路由为核心。至于优劣，我无从评判。

