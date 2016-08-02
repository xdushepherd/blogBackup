title: git 修改远程仓库
date: 2016-08-02 15:57:44
tags: [git]
categories: 开发工具
---

   电脑之前坏了，拿去修理了。修理之后，重做了系统。重做了系统之后，原本的数据就都没有了。当然也包括github的ssh指纹。因此，想要用ssh推送项目的话，就需要做一些ssh配置工作。

   但是呢，我这个人懒，当需要在推送我的的轮子[autocomplete](https://github.com/xdushepherd/autocomeplete-0-1)的时候，就直接使用了https进行项目推送了。然而，出来混总是要还的，我受不了每次都输入用户名密码啊，思来想去，还是重新配置了一下ssh推送，并且博客的configuration就使用的ssh，方便推送博客么。

   既然ssh已经配置好了，那我的轮子项目也想要通过ssh推送了，人之常情。初始化的时候，我将远程设置成了这样的：


####git设置远程仓库
   ```bash
   $ git remote add origin https://github.com/xdushepherd/autocomeplete-0-1.git
   ```


   那么问题来了，我要怎么才能ssh推送呢，通过度娘加上我的拓展，总结如下：


####1.粗鲁形式

```bash
   $ git push git@github.com:xdushepherd/autocomplete-0-1.git
```

####2.修改origin

```bash
   $ //方法一：
   $ git remote origin set-url git@github.com:xdushepherd/autocomplete-0-1.git
   $
   $ //方法二: 直接在config文件中修改origin的值
   $ vi .git/config
   $
   $ //方法三：删除origin，再添加新的origin
   $ git remote rm origin
   $ git remote add git@github.com:xdushepherd/autocomplete-0-1.git
```

####3.另建远程

```bash
   $ git remote ori git@github.com:xdushepherd/autocomplete-0-1.git
   $ git push ori master
```

我大概能记得住的方法就这么多了。