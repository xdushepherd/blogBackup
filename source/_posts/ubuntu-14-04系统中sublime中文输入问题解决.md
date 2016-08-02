title: ubuntu-14.04 下 subl 中文输入问题解决
date: 2015-07-24 02:07:31
tags: [sublime,ubuntu-14.04,中文输入] 
categories:  开发工具
---


[sublime](http://www.sublimetext.com/)这款编辑神器用起来感觉非常舒服。但是，对我这样一个使用ubuntu操作系统的代码汪来说，中文输入的问题一直困扰我。之前在做一个项目的时候，自己一直没有写注释，原因如上。而当我把队友的工作pull下来的时候，内心是很复杂的，看着他的注释，我感到了深深的歉意。当他看到我的代码的时候，心里得有多少疑惑？所以，当上一个项目完成的时候，我抽时间在网上找到了一片博客，把这个问题解决了，顺便还在ubuntu系统上装了搜狗输入法。O(∩_∩)O~。

<!-- more -->

### 安装下载sublime

请戳进这个网址： [sublime](http://www.sublimetext.com/)

### 下载并安装搜狗输入法

####下载
[搜狗输入法](http://pinyin.sogou.com/linux/help.php)
#### 安装
这里有一片百度经验教程: http://jingyan.baidu.com/article/ad310e80ae6d971849f49ed3.html

### 中文输入解决

请进该博客: http://blog.csdn.net/cywosp/article/details/32350899

### subl命令中文输入的解决

在.bashrc文件中添加一个别名

``bash
alias subl='LD_PRELOAD=/usr/lib/libsublime-imfix.so subl'
``


在我的ubuntu 14.04系统下，到这一步，无论是使用subl命令还是桌面图标启动，中文输入都没有问题。这篇博客就是在ubuntu下使用搜狗输入法在sublime编辑器中写的。