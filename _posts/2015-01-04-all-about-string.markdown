---
layout: post
title: "StringBuffer,StringBuilder与线程安全"
date: 2015-01-04 23:56
comments: true
categories: java 
---
java里，String是不可变的序列，immutable，每write一次String，实际上创建了一个新String实例。

而StringBuffer与StringBuilder存储可变的字符序列，mutable,可在原对象上append, remove, instert字符串。
改变字符串的操作较多时，应该用这两个类代替String类。二者提供的操作函数一致。

根据api文档， 二者的区别：

* StringBuffer是线程安全的。意味着如果在多线程环境下对它的实例就行操作，不会产生不确定的行为。
StringBuilder不是线程安全的。
* 由于StringBuilder不是线程安全的，StringBuilder效率更高。
