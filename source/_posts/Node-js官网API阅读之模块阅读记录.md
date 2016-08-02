title: Node.js官网API阅读之模块阅读记录
date: 2015-11-10 22:56:19
tags: [模块，modules]
categories: [node.js]
---

###概述
   Node.js拥有一套简单的模块加载系统。在Node.js中，文件和模块之间是一一对应的关系。在接下来的例子中，foo.js和circle.js处于同一个文件夹下面，以foo.js文件夹在模块circle.js模块为例，展示一下模块加载机制。


###基本定义和使用
foo.js中的内容：
```javascript
	var circle = require('./circle.js');
	console.log('The area of a circle of radius 4 is' + circle.area(4));
```

circle.js的内容：
```javascript
	var PI＝ Math.PI;

	exports.area = function(r){
		return PI * r * r;
	}

	exports.circumference = function(r){
		return 2 * PI * r;
	}	
```
<!-- more -->

circle.js模块则已经输出了area()和circumference()两个函数。通过将模块中的函数和对象添加到exports这个特殊对象中，我们就实现了将其添加到该模块的根路径中，以便接下来的使用。

模块定义文件中的本地变量是私有的，看起来好像是模块被一个函数包裹起来的。在上面的例子中，变量PI就是circle.js的私有变量。

如果你想要你设计的模块的根元素输出一个函数（比如说一个类构造器）或者你想要一次赋值就得到一个完整的对象而不是每次创建一个属性，那就把构造函数赋值给module.exports,而不是exports。

下面，bar.js使用了square模块，该模块就是输出了一个构造器。
```javascript
	var square = require('./square.js');
	var mySquare = square(2);
	console.log('The area of my square is ' + mySquare.area());
```
square.js的内容
```javascript
	module.exports = function(width){
		return {
			area: function() {
				return width * width;
			}
		}
	}
```

###循环引用

当我们使用了循环引用的时候，一个模块可能在其没有完成执行动作的时候就返回。

来看一下下面这种情况：

a.js
```javascript
	console.log('a starting');
	exports.done = false;
	var b = require('./b.js');
	console.log('in a , b.done = %j',b.done);
	exports.done = true;
	console.log('a done');
```

b.js
```javascript	
	console.log('b starting');
	exports.done = false;
	var a = require('./a.js');
	console.log('in b, a.done = %j', a.done);
	exports.done = true;
	console.log('b done');
```

main.js
```javascript
	console.log('main starting');
	var a = require('./a.js');
	var b = require('./b.js');
	console.log('in main, a.done=%j, b.done=%j', a.done, b.done);
```

主文件main.js加载a.js,然后a.js又夹在b.js。之后， b.j尝试加载a.js。为了防止出现无限循环的调用，一个a.js的不完全拷贝将exports对象返回到b.js模型中去。之后b.js完成夹在，b.js将它的exports对象提供给a.js模块。

当main.js将两个模块都加载完成之后，a和b模块都完成了加载。这个小程序的输出如下所示：

```javascript
	$ node main.js
	main starting
	a starting
	b startging
	in b,a.done = false
	b done
	in a,b.done = true
	a done
	in main,a.done = true ,b.done = true
```

###核型模块

Node.js拥有一些被编译成二进制文件的模块。这些模块在这个文档的其他部分有详细的介绍。

核心模块被定义在Node.js源代码中的／lib文件夹中。

当你使用require()来夹在模块的时候，核心模块会优先被夹在。举个例子，require('http')总是会调用核心的内建HTTP模块，即使是有一个取名http的文件。

###文件模块

如果指定名字的模块没有找到的话，Node.js会尝试依次加载带有.js，.json，.node扩展名的文件

.js文件时javascript语言的文本文件，.json则是被当作JSON数据解析的文本文档，.node文件则是一些通过dlopen加载的第三方的编译文件（这一块翻译的不好）。

如果给定的路径并不存在的话，require()方法酒会抛出一个错误，并将代码属性设置为"MODULE_NOT_FOUND"。

###从node_modules文件夹加载模块

如果require()方法接受的标志符病不是一个原生模块，而且其路径也不是以'/','../'或者'./'开头的话，Node.js酒会开始在当前模块的父文件夹的node_modules中加载指定的模块。

如果在父文件夹中并没有找到，那么它就会再向上一级文件夹中的node_modules文件夹中寻找，直到找到系统的根目录下的node_modules文件夹中。

举个例子，如果位于'/home/ry/projects/foo.js'的问价调用了require('bar.js'),那么Node.js酒会按照下面这种方式依次寻找bar.js文件：

```javascript
	/home/ry/projects/node_modules/bar.js
	/home/ry/node_modules/bar.js
	/home/node_modules/bar.js
	/node_modules/bar.js
```
这种机制允许程序定位其依赖，保证程序不崩溃。

###文件夹作为一个模块

将程序和类库组织在一个自包含的文件夹里时比较方便的，只需要提供为一个进入类库的入口点即可。有三种讲文件夹作为参数传递给require()的方法。

第一种是在文件夹的根目录中创建一个package.json，这个文件中将声明这个类库的主模块。下面是一个package.json的例子：

```javascript
	{ "name" : "some-library",
  	"main" : "./lib/some-library.js" }
```

这样，我们就可以通过require('./some-library')来实现对./some-library/lib/some-library.js的调用。

如果没有文件夹中并没有package.json文件的存在，那么Node.js会尝试调用index.js或者index.node来实现调用。举例来说
，如果上面的例子中没有package.json文件的存在，那么，require('./some-library')就会尝试加载

```javascript
	./some-library/index.js
	./some-library/index.node
```

缓存

当一个模块第一次被加载之后，该模块就被缓存了。这意味着在同一个文件下，每一次对同一模块的调用实际上得到的是同一个对象。
队医require('foo')的多次调用病不会引起该模块代码被多次执行。这是一个非常重要的特性。有了这种特性，部分执行的对象可以被返回，继而可以允许过渡的依赖被加载甚至当他们会引起循环调用。

如果你想要一个模块的代码多次执行，那就使该模块返回一个函数，然后多次执行函数即可。

###模块对象

在每一模块文件中，module变量时一个对代表当前模块的对象的饮用。为了方便，module.exports可以通过exports来访问。module实际上不是一个全局量而是每一个模块的本地变量。

####moudle.exports

moudle.exports对象是有Module系统创建的对象。有时候，这一点并不被接受，很多开发者希望他们的模块是模个类的实力对象。为了达到这个目的，我们可以讲想要输出的对象赋值给module.exports.请注意，将对象赋值给exports对象会简单的将本地变量重新绑定，这往往不是你想要的结果。

###总结

这是对Node.js官网文章的简单翻译，一方面难免出错，另外一方面，就是会有遗漏，如果你们感兴趣的话，可以和我一起翻译官网的文档。





