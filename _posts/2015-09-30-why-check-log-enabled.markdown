---
layout: post
title: "是否需要在log信息之前checkXXXEnabled"
date: 2015-09-30 11:06
comments: true
categories: learn
---
项目中经常需要log日志，有人直接log.debug(),有人在log日志
之前会进行检查log.isDebugEnabled()，而我则习惯采用第一种做法，
因为简便。那log之前检查对应级别是否开启有必要吗？

其实是有必要的。表面上看直接log.debug只进行了一次enable检查，但若之前
加上log.isXXEnabled()则会进行两次enabled检查，是否多此一举呢？其实不然，
如果不进行enabled检查，直接log.debug，如果log的message存在复杂操作比如
字符串连接或object.toString等，那么根据参数先进行计算的原则，enable
检查之前会进行复杂不必要的字符串运算增加开销，但若之前已经进行了检查
就不会有此开销。

使用log4j1.x，log之前都需进行log.isXXXEnabled检查，但若使用log4j2.x，则
可采用log.debug("divide by {}"， num)，这种新的接口输出信息，不会在level检查
之前进行额外计算。

#### 参考

[stackoverflow](http://stackoverflow.com/questions/963492/in-log4j-does-checking-isdebugenabled-before-logging-improve-performance)

[log4j2 api](https://logging.apache.org/log4j/2.0/manual/api.html)

