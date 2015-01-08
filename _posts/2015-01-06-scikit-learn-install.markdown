---
layout: post
title: "python scikit-learn安装"
date: 2015-01-06 22:59
comments: true
categories: python
---
前段时间做mapreduce算法部分设计的时候，正好看过python scikit-learn的api文档，学习了一下它的设计。
最近正好要做算法结果对比，可借机学习一下scikit-learn的使用。

在64位win8上安装scikit-learn.

* 安装python2.7。
* 安装python包管理工具pip。下载get-pip.py,然后执行`python get-pip.py`即可. 输入python -m pip，出现一堆pip命令说明，则安装成功。
* 安装scikit-learn. `python -m pip install -U scikit-learn` 
* 安装numpy. 那么问题就来了。开始同样想通过pip install来安装numpy,但一堆问题。这个命令会下载numpy源码包，然后编译，提示vc++编译器找不到。于是根据提示的下载链接下载
python vc++编译器，重新执行该命令，没问题了，可是该命令一直执行，无任何屏幕输出，卡住了，等了好久都没
结果出现，于是考虑去官网上下载可执行安装包。在sourceforge上numpy主页找到了安装包列表，但只有win32位的，
下载下来准备用win32位试试。可是安装界面找不到python27目录。提示Python27没有注册。
网上搜索下发现原因是32位应用与64位应用的注册表目录不一致，我安装的是64位python，因此32位应用会找不到python27位置，可通过修改注册表解决。
改注册表略麻烦，幸运的在stackoverflow上找到了64位python包的[下载列表](http://www.lfd.uci.edu/~gohlke/pythonlibs)，直接下载64位numpy安装包搞定.
* 安装scipy. 同样直接列表下载安装包安装。

综上，scikit-learn安装问题最后归结为在64位windows上安装numpy与scipy。numpy与scipy本身根据机器做了优化，
因此需要在本机上对源码进行编译，但由于编译出了问题，我直接下载别人提供的unofficial python lib，感谢提供
各个版本python lib下载列表的这位仁兄。
