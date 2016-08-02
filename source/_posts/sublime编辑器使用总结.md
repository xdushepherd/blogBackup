title: sublime编辑器使用总结
date: 2015-08-05 23:12:31
tags: [sublime]
categories: 开发工具
---


使用sublime text编辑器差不多两年了，今天就把一些学习和使用心得总结一下。

<!-- more -->

#### 基本功能

sublime编辑器自身有许多非常有用的功能。我们从sublime编辑器的工具栏中可以看到功能的分类有file，edit，selection，find，view，goto等等一些功能。比如说，撤销之前对文件的操作，我们可以使用Ctrl+Z，我们还可以使用Ctrl+Y来取消Ctrl+Z的撤销动作。在比方说，Ctrl+D可以选中光标当前所在的单词，继续敲击Ctrl+D可以选中当前文件中下一个相同的单词。当然在工具栏中我们还可以看到Alt+F3可以选中当前文件中所有与当前选中项相匹配的条目，形成多行游标。再比如，我们可以使用Ctrl+Shift+K删除当前所在行，也可以使用Ctrl+C来复制当前行，并使用Ctrl+V来粘贴之前复制的内容。另外，依次执行Ctrl+K和Ctrl+V两个命令，我们可以在得到复制面板，这样我们就可以得到之前几次使用Ctrl+C复制得来的内容，并将其粘贴到文件中来。


#### Package Control

[Package Control](https://packagecontrol.io/)是sublime编辑器中管理插件的工具，起安装方法比较简单。打开编辑器，敲击Ctrl+`，打开console菜单。并将下面这段代码复制到console菜单中去，然后敲击回车键。耐心等待编辑器完成Package Control的安装工作。

```bash
import urllib.request,os,hashlib; h = 'eb2297e1a458f27d836c04bb0cbaf282' + 'd0e7a3098092775ccb37ca9d6b2e4b7d'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

package control提供了大量的sublime编辑器的插件，我们可以在[Package Control](https://packagecontrol.io/)这个页面中查找自己需要的插件。

#### 使用 Package Control 安装插件

可以在[Package Control](https://packagecontrol.io/)这个网站中寻找你需要的插件，具体安装方法参考百度经验[Sublime Text3 插件安装教程](http://jingyan.baidu.com/article/4d58d541caeeaa9dd4e9c093.html)。

#### 总结

以上是我在学习sublime编辑器使用方法的时候觉得比较重要的两个方面。首先，对于基本功能，我们需要在实际的编码过程中根据自己的需要到工具栏中查找符合自己要求的功能，并记住每个功能的快捷键。这样子，在实际的开发工作中重复使用，熟能生巧，就可以很好的掌握基本功能。其次，对于插件，我们需要根据自己的需求，在网上查找网友推荐的插件，或者自行在[Package Control](https://packagecontrol.io/)这个网站中查找符合自己要求的插件。最后我想说的是，对于插件，适可而止最好，基本的功能已经非常不错了，不必要在过度追求对编辑器的功能强化。



