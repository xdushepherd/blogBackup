title: Node.js官网API阅读之事件模块阅读记录
date: 2015-11-12 23:36:17
tags: [事件，events]
categories: [node.js]
---

###概述
许多Node.js中的对象都可以触发事件：一个net.Server对象会在每一次有会话连接到它的时候出发一个事件。一个fs.readStream对象会在一个文件打开的时候触发一个事件。所有可以触发事件的对象都是events.EventEmitter的实例。你可以通过require('events')方法得到这个模块。

一般而言，事件的名字是一个驼峰形的字符串，但是，这也不是硬性的规定，所以任何形式的字符串都可以作为事件的名字。

###类:events.EventEmitter
使用require('events')来获取EventEmitter类。
```javascript
	var EventEmitter = require('events').EventEmitter;//导入events模块
	var event = new EventEmitter(); //创建一个EventEmitter的实例对象
```
<!-- more -->

当一个EventEmitter实例发现了一个错误，那么，一般情况下，会触发一个error事件。在Node.js中，错误事件会被作为特殊情况来对待。如果没有一个事件没有监听函数，那么默认的动作是打印一个栈的调用轨迹并结束程序。

当一个EventEmitter类的实例添加一个新的事件监听器的时候会触发'newListener'事件，当一个监听器呗移除的时候会触发'removeListener'事件。
###实例方法
事件的方法返回事件本身，因此可以事件链式调用。
####emitter.on(event,listener);emitter.addListener(event,listener)
向一个特定时间的监听器数组的末尾添加一个新的监听器。这个方法并不会检查监听器数组中是否已经存在了同名的监听器，因此会出现对一个事件添加了多个同名的监听器。

```javascript
	//event.js 文件,该例子来自互联网
	var EventEmitter = require('events').EventEmitter; 
	var event = new EventEmitter(); 
	event.on('some_event', function() { 
		console.log('some_event 事件触发'); 
	}); 
	setTimeout(function() { 
		event.emit('some_event'); 
	}, 1000); 
```

####emitter.once(event,listener)
对事件添加一个一次性的监听器。这个监听器只能呗事件触发一次，触发之后就被移除。

####emitter.removeListener(event,listener)
从一个事件的监听器数组中移除指定的监听器。

```javascript
	var callback = function(stream) {
	  console.log('someone connected!');
	};
	server.on('connection', callback);
	// ...
	server.removeListener('connection', callback);
```

removeListener方法一次至多只能移除一个监听器，所以，如果一个监听器如果呗多次添加到事件的监听器队列中，那么就需要多次调用removeListener方法。

####emitter.removeAllListener([event])
移除一个触发器的所有监听器或者指定事件的所有监听器。将代码中其他地方添加的监听器移除听起来并不是一个好处一，尤其是当这个触发器并不是你创建的时候。

####emitter.setMaxListeners()
EventEmitters默认情况下会在一个触发器对一个事件设置多余十个监听器的时候打印出警告。这对于寻找内存泄漏来说是一个不错的默认配置。但很显然，不是所有的触发器应该被限制只有十个触发器。这个函数就可以允许触发器提高默认的事件监听器的个数。我们可以将其设置为Infinity，这样就没有限制了，或者设置为0，不允许设置监听器。

####emitter.getMaxListeners()

返回触发器当前可以设置的最大的监听器数量的值。emitter.setMaxListener(n)或者EventEmitter.defaultMaxListeners的默认值。

####EventEmitter.defaultMaxListeners
返回触发器类的默认最大监听器数量的值。

####emitter.listeners(event)
返回触发器的指定事件的所有监听器的拷贝。
```javascript
	server.on('connection', function (stream) {
	  console.log('someone connected!');
	});
	console.log(util.inspect(server.listeners('connection'))); // [ [Function] ]
```
####emitter.emit(event[,arg1][,arg2][,...])
调用这个方法可以触发相应事件的监听器，并且需要按照监听器的参数顺序传递参数。
如果事件拥有该监听器，则返回true，反之，返回false

####emitter.listenerCoutn(event);EventEmitter.listenerCount(emitter,event)
返回指定事件的监听器数量。

###事件
####newListener
每当emitter实例新添加了一个事件监听器的时候，都会触发该事件。
####removeListener
每当一个监听器呗移除的时候都会触发该事件。
###继承EventEmitter的方法
```javascript
	'use strict';
	const util = require('util');
	const EventEmitter = require('events');

	function MyEventEmitter() {
	  // Initialize necessary properties from `EventEmitter` in this instance
	  EventEmitter.call(this);
	}

	// Inherit functions from `EventEmitter`'s prototype
	util.inherits(MyEventEmitter, EventEmitter);
```
###附录－－阅读API时的一些代码，参考网友博客。
```javascript
	var events =  require("events");
	var util = require('util');
	var  x =new events.EventEmitter();
	x.on('newListener',function(event, listener){
	 console.log(event + ' : '+listener);
	});
	x.addListener('y', function(a,b,c){
	 console.log('it\'s work1!'+a+b+c);
	});
	x.on('y', function(a,b,c){
	 console.log('it\'s work2!'+a+b+c);
	});
	x.on('y', function(a,b,c){
	 console.log('it\'s work3!'+a+b+c);
	});
	x.on('y1', function(a,b,c){
	 console.log('it\'s work3!'+a+b+c);
	});
	//x.emit('y','111','222', '3333');
	//x.removeAllListeners('y');
	//var z = x.listeners("y1");
	//console.log(z+'');
	x.setMaxListeners(100);
	console.log(x.getMaxListeners());
	x.on('connection', function (stream) {
	   console.log('someone connected!');
	});
	console.log(x.listenerCount('y1'));
	console.log(util.inspect(x.listeners('connection')));
```


###参考

[一个事件继承的例子](http://biyeah.iteye.com/blog/1308954)









