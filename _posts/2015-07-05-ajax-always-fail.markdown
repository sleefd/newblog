---
layout: post
title: "ajax总是调用error handle"
date: 2015-07-05 13:04
comments: true
categories: front-end
---

做进度条展示状态遇到一个问题:发一个ajax请求，轮询后台状态，如果后台
出现异常，在error回调中clearTimeout,结果error总是被调用，轮询总是
被终止。

浏览器调试，network请求显示成功，服务端spring controller也没出现任何
错误或异常，但前端error总是被call。查stackoverflow发现是dataType问题。

ajax期望返回的是json数据，但spring controller返回void，response什么
也没有，response与期望返回类型不一致导致always error，ajax 去掉dataType
设置，让浏览器自行推断类型，就没有问题了。


