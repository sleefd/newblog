---
layout: post
title: "tomcat远程调试"
date: 2015-09-19 22:39
comments: true
categories: learn
---
若tomcat服务器安装在远程主机，web应用出现了问题，通常需要远程调试来解决。

远程调试不用在本地模拟生产环境，只需要远程服务器开启debug模式，本地连上
服务器的监听端口即可调试，且需具备与远程服务器相同的代码。

连接上远程服务器后，直接在本地代码断点，即可达到与本地断点调试相同的效果。

tomcat远程调试非常简单，将start.sh最后一行启动代码改为<code>exec "$PRGDIR"/"$EXECUTABLE" jpda start "$@"</code>即可，
默认会开启8000端口监听连接或直接命令行执行<code>catalina.sh jpda start</code>。

端口定义在catalina.sh中，可通过修改JPDA\_ADDRESS来修改监听端口号。

使用intellij idea，可Edit configuration，然后选择remote或tomcat server remote配置项，
接着在Host和Port中分别填上ip地址与监听端口号，即可开始连接远程服务器进行调试。

