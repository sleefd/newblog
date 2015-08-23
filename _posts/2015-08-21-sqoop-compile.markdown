---
layout: post
title: "win下sqoop1.99.6编译"
date: 2015-08-21 12:47
comments: true
categories: bigdata 
---

sqoop使用maven进行项目构建，所以按照maven的构建方式编译源码即可，
官网也有说明编译步骤。

编译过程中遇到一些小问题，一并记录在次。

###下载源码
clone github上sqoop的源码，可以看到很多分支，现在working的sqoop分支是sqoop2，
因为要编译1.99.6，所以直接checkout branch-1.99.6

{% highlight bash %}
git clone https://github.com/apache/sqoop.git
git branch -a # show all the branches
git checkout branch-1.99.6
{% endhighlight %}

###编译源码

{% highlight bash %}
mvn compile -DskipTests # compile, skip tests, fast compile speed
{% endhighlight %}

编译到common包，执行saveVersion.sh遇到问题"invalid escape character \\".

saveVersion.sh生成package-info.java文件中的注解。
打开生成的package-info.java一开，发现user字符串的内容为"slee\sleefd",
显然在java中是非法字符串，可以改为<code>\\</code>或直接改为<code>/</code>即可,继续
编译ok。

###打包发布
{% highlight bash %}
mvn package -Dbinary -DskipTests
{% endhighlight %}

###安装
安装包在dist/target目录中.具体安装方式可参考官方安装文档，但win下编译的代码在linux下使用会遇到问题：

tar.gz包传到linux上后解压，配置完毕，运行bin目录下的shell脚本，terminal提示
**no such file or direcotory** 。

这是由于这些shell脚本都是dos格式，不是unix格式，可vim打开后，通过 :set ff，
查看文件格式，把这些shell脚本转为unix格式，问题解决。

可执行命令dos2unix，完成转换. linux下需要安装了该命令才能使用，win下git的
bash自带该命令，估计cygwin也有这个命令。解决该问题后可成功启动sqoop2-server.
