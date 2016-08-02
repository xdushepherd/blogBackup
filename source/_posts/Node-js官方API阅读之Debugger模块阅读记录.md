title: Node.js官方API阅读之Debugger模块阅读记录
date: 2015-11-13 19:53:46
tags: [调试，debugger]
categories: [node.js]
---


###概述
V8引擎自带了一个扩展的debugger，这个调试工具可以通过简单的TCP协议实现在进程外访问程序。Node.js为这个调试器实现了一个内建的客户端。想要使用这个客户端，你只需要在在Node.js文件执行时添加一个debug参数就可以；

```javascript
	$node debug myscript.js
	< debugger listening on port 5858
	connecting... ok
	break in /home/indutny/Code/git/indutny/myscript.js:1
	  1 x = 5;
	  2 setTimeout(function () {
	  3   debugger;
	debug>
```
<!-- more -->
Node.js调试工具客户端不支持所有的命令，但是简单的单步测试和查看时可以实现的。通过在源代码中添加'debugger;'语句，你就添加了一个断点。

举个例子,假设有这样一个myscript.js文件:
```javascript
	// myscript.js
	x = 5;
	setTimeout(function () {
	  debugger;
	  console.log("world");
	}, 1000);
	console.log("hello");
```
那么当调试器开始运行的时候，会在第四行断开:

```javascript
	% node debug myscript.js
	< debugger listening on port 5858
	connecting... ok
	break in /home/indutny/Code/git/indutny/myscript.js:1
	  1 x = 5;
	  2 setTimeout(function () {
	  3   debugger;
	debug> cont
	< hello
	break in /home/indutny/Code/git/indutny/myscript.js:3
	  1 x = 5;
	  2 setTimeout(function () {
	  3   debugger;
	  4   console.log("world");
	  5 }, 1000);
	debug> next
	break in /home/indutny/Code/git/indutny/myscript.js:4
	  2 setTimeout(function () {
	  3   debugger;
	  4   console.log("world");
	  5 }, 1000);
	  6 console.log("hello");
	debug> repl
	Press Ctrl + C to leave debug repl
	> x
	5
	> 2+2
	4
	debug> next
	< world
	break in /home/indutny/Code/git/indutny/myscript.js:5
	  3   debugger;
	  4   console.log("world");
	  5 }, 1000);
	  6 console.log("hello");
	  7
	debug> quit
	%
```
在上面使用到的命令中，repl命令允许你运行代码。next命令则使得程序继续向下一行执行。其他的一些命令接下来会为大家做介绍。在调试环境下，输入help命令可以看到其他可用命令及其介绍。


###Watchers
你可以在调试代码的过程中观察表达式及变量。在每一个断点上，在watchers列表的表达式，会在当前上下文中被执行。，并且展示断点前的代码列表。

输入watch('my_expression').watchers来开始观察一个表达式，输入unwatch('my_expression')来移除一个watcher。

###命令介绍

####步骤执行
cont,c-继续执行稳健
next，n－执行一行
step, s - Step in
out, o - Step out
pause - Pause running code (like pause button in Developer Tools)
####断点
setBreakpoint(),sb()-在当前行设置断点
setBreakpoint(line),sb(line)-在指定行设置断点。
setBreakpoint('fn()'),sb('fn()')-在函数题定义的第一句处设置断点
setBreakpoint('script.js',1),sb()-在script.js的第一行设置断点
clearBreakpoint('script.js',1),cb(...)-清除在script.js的第一行设置的断点。
###info
backtrace,bt-打印当前已经执行的代码信息。
list(5)-列出当前行之前和之后5行代码
watch(expr)-将一个表达式添加到监测列表
unwatch(expr)-将表达式从检测列表中移除
watchers-列出所有的watchers
repl-打开一个调试器的repl，在调试脚本的上下文中执行语句。
###执行控制
run-运行脚本
restart-重启脚本
kill-杀死当前脚本。
###Various
scripts－列出已经加载的脚本
version-展示V8引擎的版本。


###后记
阅读的过程中试着调试代码，其实也就理解了诸如cont,next,list(n),sb()等等一些命令的用法，对step，out，pause等等命令的用途并没有体会。希望在以后项目的开发过程中可以有所体会。













