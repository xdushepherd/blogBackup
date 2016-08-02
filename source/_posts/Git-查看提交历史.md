title: Git-查看提交历史
date: 2015-08-08 00:03:35
tags: [git]
categories: 开发工具
---


声明：本文学习[Pro Git](http://git-scm.com/book/zh/v1)，对文中内容大量引用。

<!-- more -->

掐指一算，O(∩_∩)O~，从接触git到现在已经有了三年时间了。带我入门的是从西电图书馆借来的[Pro Git](http://git-scm.com/book/zh/v1)这本书，经过这本书的洗礼，对git有了皮毛的认识。在之后的三年中，我在不同的项目中使用git来控制项目的版本，总的来说，是用到什么，就去查什么，并没有很系统的来学习git，心中总是对git有一些似是而非的感觉。趁着这个暑假，我开始把自己在项目实践中遇到的常用操作进行依次较为系统的学习。


至于比较简单的操作我就不在这里多说了，毕竟只是一个工具，日常最简单的操作，熟能生巧即可。本篇主要学习和总结的是如何查看git的提交历史。在做项目的时候，当你第一次clone了一个项目仓库的时候，或者在将远程工作更新到本地的时候，我们就需要查看一下版本仓库的提交历史，以便了解自己或者同事之前的工作情况。

####git log

当我们的到了一个已经存在的git仓库之后，在项目中运行 git log，应该会看到下面的输出(使用[Pro Git](http://git-scm.com/book/zh/v1)的例子)：

```bash
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test code

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
```

####git log -p -n

  我们常用 -p 选项展开显示每次提交的内容差异，用 -n 则仅显示最近的n次更新，示例如下：

  ```bash
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,5 +5,5 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.name      =   "simplegit"
-    s.version   =   "0.1.0"
+    s.version   =   "0.1.1"
     s.author    =   "Scott Chacon"
     s.email     =   "schacon@gee-mail.com

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test code

diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
     end

 end
-
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
\ No newline at end of file
  ```

  该选项除了显示基本信息之外，还在附带了每次 commit 的变化。当进行代码审查，或者快速浏览某个搭档提交的 commit 的变化的时候，这个参数就非常有用了。


  ####git log --pretty=oneline

  每个提交都列出了修改过的文件，以及其中添加和移除的行数，并在最后列出所有增减行数小计。 还有个常用的 --pretty 选项，可以指定使用完全不同于默认格式的方式展示提交历史。比如用 oneline 将每个提交放在一行显示，这在提交数很大时非常有用。示例如下：

```bash
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test code
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
```

另外还有 short，full 和 fuller 可以用，展示的信息或多或少有些不同，请自己动手实践一下看看效果如何。


####提交历史查看总结

本篇的内容，都是摘抄自[Pro Git](http://git-scm.com/book/zh/v1)这本书中，选取了我自己觉得会使用到的一些命令，大家还可以自己在[查看提交历史](http://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)这个章节进行深入研究。如果有什么更好的想法，可以评论告诉我，大家一起成长，O(∩_∩)O~。
