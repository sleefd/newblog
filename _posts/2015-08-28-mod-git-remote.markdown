---
layout: post
title: "git fork工作流"
date: 2015-08-28 17:28
comments: true
categories: learn
---

项目代码库开始和gerrit关联，用gerrit做code review，然后又突然换成自建的gitlab，
然后就有了这篇文章. 

换成gitlab之后，需要以fork/pull request的方式提交代码,code review，因此
有必要了解git fork工作流.
(zz: gitlab->gitlab->gerrit-gitlab，这样换来换去有意思吗,呵呵,遇到commit问题，从来不想着方法去解决，而是更换版本控制工具也是醉了)

首先fork central repo，这样自己的gitlab projects下就多了一个central repo的副本，后续的提交都应该提交到每个人自己的repo，
而不能提交到central repo. 这也是fork工作流的好处，控制central repo的访问权限，不会有任何人的commits污染整个项目.

其次, 我的本地仓库在gerrit时代已经有了，上面也包含几个我正在开发新特性的分支，所以不可能clone我的gitlab repo重新开发。
因此第一步需要更换master branch的remote为新gitlab仓库的地址. 有两种方法:

{% highlight bash %}
# add remote and modify master branch's remote to the new remote
git remote add gitlab repo-git-url 
git config branch.master.remote  gitlab

# or change old remote url directly, suppose origin the old remote
git remote set-url origin repo-git-url
{% endhighlight %}

这样后续push可以直接到gitlab上的repo里，而remote对应的也是master分支，不会
出现your head is ahead of master xx commits这种提示.

然后后续开发还是以分支开发的形式就行，每个新特性开发对应一个branch, 单独在这个branch    
commit代码.

然后怎么code review呢,发pull request就行了，俗称pr.
gitlab是merge request，github是pull request。
选择自己项目的分支以及central对应的分支名称，@相关人员就ok了。

---------------------------
有感：怎么样的环境就有什么样的坑爹事情发生，爱咋地咋地.
