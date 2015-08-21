---
layout: post
title: "git相关操作"
date: 2015-08-20 16:37
comments: true
categories: learn
---

平时自用git涉及不到什么高级的用法，因为也就一个人开发。
最近工作上用gerrit提交代码, 遇到很多问题, 发现对git的有些
操作并不了解，现总结一些问题及命令如下：

### 每次提交代码都应该先pull，再git review; 或者先pull，再push 
pull保证本地代码与remote一致，再push自己本地的修改到remote,这样不会有冲突。

直接push很可能有冲突。

{% highlight bash %}
git pull origin master
### other operations
git push origin master
{% endhighlight %}

### git commit --amend, amend上一次提交，不会产生新提交
可用于重新修改comment，或修改上一次提交的文件内容，只是再次commit
时要加上--amend选项。

###  丢弃git add到暂存区的文件修改

有三种方法，一是在原文件上修改后重新add，staged的相同文件就会被覆盖;

另一种方法是reset head;

还可以重新checkout该文件，那么工作区中该文件就会回到未修改之前的状态，再在上面做修改后add.

{% highlight bash %}
git reset head filename ## unstaged filename
git reset head # unstaged all the files
git checkout -- filename # reset file in the workspace
{% endhighlight %}

### git reset, 重置到某次提交
提交多次后，想回到某次提交的状态，可以使用reset，
reset之后，该次提交之后的所有提交都木有了。

{% highlight bash %}

# reset to last second commit,
# all the modified in head return to workspace
git reset head~1 

# reset to last second commit,all the modified are discarded
# workspce is clean
git reset head~1 --hard

{% endhighlight %}

### git revert commit, revert某次commit

会产生一次新的提交，新的提交重置commit的内容

{% highlight bash %}

# revert head, produce a new commit "revert"
git revert head  

#git revert commit of SHA
git revert COMMIT_SHA

{% endhighlight %}

### git stash可缓存工作区的内容到一个栈中，让工作区恢复到clean状态

可通过git stash apply或git stash pop来恢复最后被暂存的东西（位于栈顶）。

注意untrack的文件还在workspace中。

当需要从一个分支切换到另一个分支而有不想commit当前工作内容，可以先stash，
然后切换分支。

{% highlight bash %}
		
git stash
git checkout master 
### do sth
git checkout dev
git stash apply # get back what u are doing 

git stash list # review all the stashed content
git stash apply stash@{2} #choose last second stash apply
git stash pop #apply stash and remove the stash from stash list
git stash clear #clear stashes in stash list

{% endhighlight %}

### 新特性在分支开发，开发完毕后，rebase到master，可保证分支的每次commit，都可以review

对于较复杂的feature开发，肯定会有多次commit，gerrit分支上的每次commit也会产生changeID, 分支rebase到最新的master,
然后git review，会把所有逻辑化的commit按顺序提交到gerrit进行review,
并且rebase后master的commit历史是线性的不会出现分叉。

{% highlight bash %}
git pull origin master # update master 

git checkout -b feature
## multiple times modify or add 
git add 
git commit  

git rebase master ##fast merge or fix confilcts
git checkout master
git merge feature 

{% endhighlight %}

### 冲突解决，merge、pull、rebase都可能会有冲突， 冲突解决之后，才能继续操作

出现冲突，可先用git status查看有哪些文件冲突，然后一次修改这些文件。

冲突的地方在<<<< 与 >>>>之间，中间由=====分隔开，上下分别代表不同commit的修改内容。

解决冲突可采取接纳你的代码或其他人的代码或合并的方式，解决完之后重新commit.

{% highlight bash %}
git status  ### check unmerged conflict files
### fix conflicts
git add .  ### stage modified files
git commit -m "fix conflicts" ## commit, fix conflicts over
{% endhighlight %}

### git detached head

出现detached head是由于checkout了非head的commit，
git checkout master后就没有了

### git rebase -i,交互式变基，非常强大的功能
可重新组织commit的提交顺序，修改commit，拆分commit，
目前只会简单使用。

{% highlight bash %}
git rebase -i commit ## appear a vim editor contains all the commits after the commit
### do some edit, modify pick to edit，you can edit the commit 
git rebase --continue ## continue rebase, until end the rebase
git rebase --abort ## abort rebase, all the ops are canceled
{% endhighlight %}
