---
layout: post
title: "快速搭建 Jekyll Blog (一)"
description: "Jekyll是一个简单的免费的Blog生成工具，类似WordPress。但是和WordPress又有很大的不同，原因是jekyll只是一个生成静态网页的工具，可以部署到github上面作为pages。"
category: github
tags: ["blog"]
---
{% include JB/setup %}


Jekyll是一个简单的免费的Blog生成工具，类似WordPress。但是和WordPress又有很大的不同，原因是jekyll只是一个生成静态网页的工具，可以部署到github上面作为pages。并且github可以配合许多第三方服务,例如评论服务disqus、分享服务jiathis提供完整的blog服务。下面我们一步一步看Jekyll是如何简单的搭建博客的

1.建立新库(Repository)
=======================

首先当然是得有github的账户，并建立一个新的库(Repository), 新库必须按照 **USERNAME.github.com** 命名。


2.安装 Jekyll-Bootstrap
===========================

::

$ git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
$ cd USERNAME.github.com
$ git remote set-url origin git@github.com:USERNAME/USERNAME.github.com.git
$ git push origin master

记得把USERNAME换成你的用户名，在push过程中也许会让你提供你ssh的公钥 rsa_id.pub


3.本地运行 Jekyll 
===================

先安装Jekyll:

::

$ gem install jekyll

如果有任何问题，可以参考 Jekyll 安装文档(If you run into a problem please consult the original Jekyll installation documentation)。
安装完成就可以在本地运行jekyll了

::

$ cd USERNAME.github.com 
$ jekyll --server # 记得把 USERNAME 改为你github的用户名

4.发布第一篇博客
===================

可以通过rake 任务轻松发布博客

::

$ rake post title="Hello World"

在_post目录下会生成一个md文件，对文件进行简单编辑
现在在浏览器打入 localhost:4000 是不是就可以看到一个运行的博客了


5.提交到github
=================

通过简单的git命令就可以把变更提交到github

::

$ git add .
$ git commit -m "Add new content"
$ git push origin master

现在通过USERNAME.github.io 就可以测试刚才的博客了

