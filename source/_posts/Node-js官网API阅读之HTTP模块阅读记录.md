title: Node.js官网API阅读之HTTP模块阅读记录
date: 2015-11-15 21:54:39
tags: [Http，http协议]
categories: [node.js]
---


###概述

require('http')方法可以得到HTTP服务器模块。

Node.js中的HTTP接口被设计来支持许多该协议的特点，而这些特点传统上对我们来说使用起来时比较麻烦的。这个接口注重的是不缓存整个请求或者响应。开发者可以实现数据流的传输。

HTTP消息的头部时这样一个对象：

```javascript
	{ 'content-length': '123',
	  'content-type': 'text/plain',
	  'connection': 'keep-alive',
	  'host': 'mysite.com',
	  'accept': '*/*' }
```
健的值是小写的。值时未修改的。

为了支持所有的可能的HTTP应用。Node.js中HTTP的API事一个非常底层的接口。这个API处理流数据并且可以完成消息的解析。这个接口可以将一个消息解析成一个头部和主体部分的两个消息，但是，这个接口不回解析真正的头部或者主体部分。

默认的头部允许一些键有多个值，这些值有'，'分隔开来，除了set-cookie和cookie，这两个键的值时存在一个数组中的。
像content-length这样的键智能有一个值。

原生的headers事保存在rawHeaders属性中，这个属性是这样的一个数组：[key,value,key2,value2,...]。举个例子，上面的那个消息头对象可以有下面这样一个rawHeaders列表：

```javascript
	[ 'ConTent-Length', '123456',
	  'content-LENGTH', '123',
	  'content-type', 'text/plain',
	  'CONNECTION', 'keep-alive',
	  'Host', 'mysite.com',
	  'accepT', '*/*' ]
```







