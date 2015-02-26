---
layout: post
title: "python docstring使用"
date: 2015-02-16 15:37
comments: true
categories: python
---

docstring，文档字符串, 是出现在函数或类开头以"""包围的字符序列。
可用来描述函数作用, 函数的使用方式及对函数做单元测试。

docstring中类似运行在python交互解释器中的代码段, 可通过doctest模块
运行，基于docstring对函数做单元测试，是保证代码正确性的简单有效手段。

help(funcname)或funcname.\_\_doc\_\_  输出的就是该函数docstring的内容.

{% highlight python %}
def extract_post_head(filepath):
    """
        extract the head of a html post, head contains
        post name and post date.
        >>> extract_post_head( r"blog\\2014\\01\\20\\adapter-model\\index.html")
        ('adapter-model', '2014\\\\01\\\\20')
    """
    namepattern = r"([\w-]+)\\[\w-]+\.html$"
    filename = re.search(namepattern, filepath).group(1)

    datepattern = r"\d+\\\d+\\\d+"
    date = re.compile(datepattern).search(filepath).group()
    return filename, date
if __name__ == "__main__":
	import doctest
	doctest.testmod(verbose=True) 

{% endhighlight %}

写上面的demo学到以下两点:

1. 转义\\:  在string literal中需要4个\\，在rawstring中只需要两个\\
2. tuple中字符串的引号为单引号
