title: Linux学习要点
date: 2015-10-21 22:04:18
tags: [linux]
categories: [Linux系统管理]
---


####前言

作为一个已经把[鸟哥的私房菜](http://vbird.dic.ksu.edu.tw/linux_basic/linux_basic.php)基础篇看过３遍以上，平时使用Ubuntu开发程序的程序员，最近闲来无事，从图书馆又借来了一本[Linux系统管理技术手册](http://baike.baidu.com/link?url=lVvLnV6vA3ElovRCBN9gpFhmH1bCTZQadshraC8R5QsySzxoo4b1Rl1LothpiV__UYJb-U_0JhAXWoMF20atc_)拿来啃。鸟哥的书很不错的，本人在Ubutntu系统上进行程序开发时间也比较久了，因此关于系统方面基本的知识，读起来并没有什么障碍。从这一片开始，我会将书中对我有指导意义的知识点记录下来，方便自己提高。

<!-- more -->
####vi

vi或者vim编辑器，当然还有emacs或者pico等编辑器。按照我的观点，掌握vi编辑器就可以了，至于更方便的编辑器，我还是比较倾向于sublime text编辑器。

####Python

在Linux系统中，脚本可以简化我们的工作，通常我们会使用Shell，Perl，Ruby或者Python等脚本语言来编写脚本。我倾向于学习Python作为我编写脚本的语言。

####expect

之前刚开始使用Linux的时候，一直想实现自动登陆校园网的功能，记得当时在网上下载的代码中就使用了expect，实现用户名和密码的输入功能。有时间可以详细地玩一玩。

####man和info

使用man命令可以查看相应命令的具体用法。其实使用man命令就可以满足我们日常的需求，大多时候使用百度就可以查到我们需要的命令，如果能google的话再好不过。

```bash
man vim
```
####which

使用which命令可以查找相关命令是否已经在搜索路径中。whereis和locate命令可以查找文件所在的目录。

```bash
$ which  gcc
/usr/bin/gcc
```

####总结

这几个知识从书中的第一章中挑选出来的，属于平时经常会用到的，无论做什么都可以用到。


