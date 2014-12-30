---
layout: post
title:  "octopress博客迁移到新电脑"
date:   2014-12-28 20:35:27
categories: jekyll update
---

本来是用octopress写博客，但是上班换了新电脑，需要在新电脑上重新部署octopress。

首先clone下source分支的代码,然后clone master分支的代码到\_deploy文件夹中。master分支存放的是渲染后的博客，source分支存放的是octopress代码以及主题等。github访问xx.github.com，访问的就是该repo下的master分支代码。

重新部署的时候遇到下面几个问题：

* source分支的代码从来没有上传过， github上只保存有最新的master分支的文件。
* rake new_post，然后rake deploy之后，\_deploy文件夹被清空，然后重新生成，导致原来的所有post遗失。
* 所有post的markdown文件遗失，无法重新生成所有的html文件。如果源markdown没有遗失，啥事都没有，哎。

尝试过这样一些解决方法：

* 修改Rakefile，修改push task，每次push都不删除deploy文件，不行。因为new_post会产生新的index.html和sitemap，虽然保留有旧的post html，但没法从index.html访问到。
* 复制deploy中的文件到source文件夹中，因为source文件夹缺少一些资源，还是不行。rake generate的时候，没有更新index.html。

稍后有时间再试一试方法2，可以先把资源拷贝到source里面，然后修改index，保有原来的post链接，新链接可添加在index里面。如果还是不行的话，就只能写个脚本调用pandoc将html转化成markdown，然后再把所有post的markdown丢到soure的post文件夹下面了。

这次重新deploy帮助review了好多东西，首先是github的基本概念，然后是branch的基本操作，remote的基本操作,怎样阅读diff文件等等。决定抽时间看git pro，系统的学习一下git的操作。