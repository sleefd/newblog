---
layout: post
title: "修改jekyll默认样式"
date: 2015-06-19 19:21
comments: true
categories: 
---

jekyll默认模板代码高亮不太好看，posts显示也没有分页,可以对默认样式做一些简单的修改:

修改代码高亮, 添加分页， 改变css来个性化博客.

### 代码高亮

jekyll代码高亮默认采用github的代码高亮css,即是_sass目录下的_syntax-highlighting.scss文件，

采用scss编写。改变代码高亮需要修改该文件或者添加额外的css覆盖原样式。jekyll支持通过pygments

插件来实现代码高亮,pygment可生成不同主题的css文件或直接对某段代码实现高亮。

* 安装Pygments. pygments是一个Python package，可通过pip安装：

{% highlight bash%}
python -m pip install pygments 
{% endhighlight %}

* 生成theme对应的css文件。可通过执行pygmentize脚本来生成css文件。

win下需要把python安装目录下的scripts文件夹添加到path环境变量.

我比较喜欢monokai主题,配上黑色很好看。

{%highlight bash%}
pygmentize -S monokai -f html > monokai.css
{% endhighlight %}

然后将css文件拷贝到工程css目录并将该文件include到head.html中即可。

以后所有html文件都将应用该样式.

* _config.yml中将highligher设置为pygments即可.

### 添加分页及页面navigation

 _config.yml配置paginate为每页想展现的posts条目数，

  参照[jekyll](http://jekyllrb.com/docs/pagination/)官网的写法，将yml代码粘贴index.html中，就能实现post list分页及导航。                                                                                                           
  
### 修改样式

样式修改可以通过直接修改_sass文件夹中的scss文件或添加额外的scss或css文件，

再include到html页面中。

css文件和scss文件都需要放到css文件夹中,scss在build的时候会自动编译为对应的

css文件，都可以直接在html中include。如果将scss文件放到sass文件夹中，必须被css

目录下的scss文件import才会被编译生效。
