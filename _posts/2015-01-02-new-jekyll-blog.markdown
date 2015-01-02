---
layout: post
title: "new-jekyll-blog"
date: 2015-01-02 18:07
comments: true
categories: 
---

在windows上安装octopress遇到一些问题，如下：

* 我安装的是ruby2.0，但gemfile.lock中的ffi版本太低，无法本地编译ffi，修改gemfile中的ffi为高版本就可以了。
* bundle install成功后，rake generate提示iconv找不到, 原来ruby2.0, iconv module被deprecated，没办法，ruby2.0安装octopress不成功，除非修改rakefile，将用到iconv的地方换成其他库。

最后决定回归本真，还是用jekyll，ruby2.0直接`gem install jekyll`就ok了，无任何问题。

仔细阅读了下jekyll官网文档，知道了原来github任意repo的ghpage分支也能用来部署website，只要把
website代码push到repo的ghpage分支，然后通过yourname.github.com/repo-name就可以访问到。这样就太好了,
原来的博客可保留在[sleefd.github.com](http://sleefd.github.com)链接，新jekyll博客在[sleefd.github.com/newblog](http://sleefd.github.com/newblog)。最后我的github repo是这样的:

	repo name: newblog, 于是我可以通过sleefd.gihtub.io/newblog，来访问我新部署的jekyll blog.
	remote branch: master与gh-pages,其中gh-pages跟踪_site文件夹的内容,master跟踪其他所有文件夹.

jekyll使用时遇到的两个问题：

* 开始我github新建的repo名字为blog，导致原博客的post无法访问，是因为原博客的post访问链接以sleefd.github.io/blog开始，新jekyll博客也是，名字冲突，改repo名为newblog就好了。
* jekyll serve 无法在localhost:4000预览博客内容，部署后也无法在newblog链接访问新post内容。看jekyll官网文档解决。修改_config.yml中的baseurl为'/newblog',启动server命令改为`jekyll serve --baseurl ''`即可。

另外jekyll不像octopress那样, 可以直接执行rake task来创建新post和部署博客到github，于是把Octopress中的Rakefile拿过来改改，就可通过`rake post['title']`创建新post，通过`rake deploy`同步source和gh-pages代码到远程的github repo。附上Rakefile如下：

{%gist 0ec44f5202e4f864e4a5 %}
