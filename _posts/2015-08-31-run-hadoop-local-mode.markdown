---
layout: post
title: "win下local模式运行hadoop作业"
date: 2015-08-31 14:24
comments: true
categories: bigdata
---
之前测试mr算法，都是通过写MRUnit来测试map或reduce逻辑完成(通过MiniYarnCluster或MiniMRYarnCluster来做测试更可靠，MRUnit测试非常有限)。然后在单元测试没问题后，
整个算法打fatjar放到集群上运行来保证正确性。

但fatjar较大，打一个fatjar包需花费大量时间，并且每次修改都要重新打fatjar，而且还要上传到集群测试非常麻烦，所以
考虑以hadoop local模式运行mr算法, 无需打jar包，直接运行本地小量数据，测试算法正确性，非常方便。

要想在win下以local模式运行hadoop作业，必须在win下重新编译native hadoop, 否则直接下载的官方安装包，在运行过程中会
因为缺少winutils.exe、dll、lib等native libraries报错。出现以下错误都是由于缺少这些包：

{% highlight java %}
ERROR [main] util.Shell (Shell.java:getWinUtilsPath(303)) - Failed to locate the winutils binary in the hadoop binary path
java.io.IOException: Could not locate executable null\bin\winutils.exe in the Hadoop binaries.

Exception in thread "main" java.lang.NullPointerException   at
java.lang.ProcessBuilder.start(ProcessBuilder.java:1012)    at
org.apache.hadoop.util.Shell.runCommand(Shell.java:404)     at
{% endhighlight %}

因此第一步是win下编译hadoop，可参见hadoop官方文档，有说明如何基于win平台编译hadoop。
编译成功后，相比于官方提供的安装包，bin目录下会多了winuitls.exe, hadoop.dll等文件，
这就是我们所需要的。

第二步配置环境变量HADOOP\_HOME为hadoop位置。

第三步直接运行算法，会启动hadoop local\_job。
可在工程中加入log4j配置，开启debug模式，这样在作业运行过程中就可以看到hadoop
输出的一些日志信息便于调试。（注：在工程中加入log4j配置文件，是调试开源项目非常好的手段，
有时候没有这些日志完全不知道错误出在哪里）。

local模式下job的输入输出绝对路径是以工程所在磁盘为根目录，相对路径是以项目位置为根目录。

比如绝对路径/user/root/test对应为disk:/user/root/test,相对路径root/text则对应disk:/yourproject/root/test,
其中disk为项目所在磁盘。






