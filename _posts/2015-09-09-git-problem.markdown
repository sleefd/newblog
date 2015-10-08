---
layout: post
title: "git问题again"
date: 2015-09-09 19:46
comments: true
categories: learn
---
最近又遇到两个git相关的问题，记录在此。

问题1，amend commit之后无法再次push到remote

{% highlight bash%}
git commit
git push remote branch
### modify something
git commit --amend	
git push remote branch # failed
{% endhighlight %}

push失败是由于同一个commit但内容不同造成的，
如果这个repo只有你一个人开发，可以强制push:
<code>git -f push remote branch</code>.
否则可采取比较dirty的做法，删掉remote branch,
重新push上去.
{% highlight bash%}
git push remote branch :branch #delete remote branch
git push remote branch  # recreate remote branch
{% endhighlight %}

问题2：如何获取remote新增的branch

情况是这样的，fork了一个remote repo，开始只有一个branch，
然后remote新增了一个branch，如何获取这个新增的branch，
可以先fetch，然后checkout新branch，再push到自己的remote就可以了。

{% highlight bash %}
git fetch upstream # get new added branch
git remote -v # upstream/newbranch
git checkout newbranch  # get the fetched newbrach
git push yourremote newbranch 
{% endhighlight %}
