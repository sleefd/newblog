---
layout: post
title: "python yield、decorator 与可变参数"
date: 2015-02-07 20:52
comments: true
categories: python
---
查了点资料，学习了下yield与decorator的使用

先看yield, 使用yield的函数，在调用参数后成为一个generator对象，

每次调用generator的next函数可返回每次yield的值。函数调用不执行

函数代码,而是得到一个iterable的generator对象，调用该对象的next方法
可得到每次迭代的值。

{% highlight python %}
def fib(n): #fibnacci numbers
	a, b, c = 0,1,1 
	for i in range(n):
		yield b
		b, c = c, (b+c)
generator = fib(5)
for i in range(5):
	print generator.next()
{% endhighlight %}

decorator装饰器，接受一个函数作为参数，返回一个经过修改的函数.

如下面代码所示，定义了一个decorator, 该decorator对函数f做改变后返回一个新函数.

{% highlight python %}
def decorator(f):
	def anotherF():
		//do something with f
	return anotherF

@decorator #annotate
def f():  #f now is a new function decorated by decorator
    pass

{% endhighlight %}
decorator涉及到的概念包括：

* 函数作用域
* 变量生命周期
* 高阶函数
* 闭包

定义多变参数：
{% highlight python %}
def f(*args):
	print type(args) #tuple
	print args
f(2, 3, 4)

def g(**args):
	print type(args) #dict
	print args
g(k1=1, k2=2, k3=3)
{% endhighlight %}

参考 

[http://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/index.html](http://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/index.html)  

[http://simeonfranklin.com/blog/2012/jul/1/python-decorators-in-12-steps/](http://simeonfranklin.com/blog/2012/jul/1/python-decorators-in-12-steps/)



