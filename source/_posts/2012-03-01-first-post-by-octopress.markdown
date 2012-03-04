---
layout: post
title: "第一篇用Octopress生成的日志"
date: 2012-03-01 21:31
comments: true
categories: 
- octopress
- jekyll
- computer
---
先是在知乎上发现了一个叫[吕坤][zhihu.lvkun]的人，顺着简介发现了[他的博客](http://lvkun.github.com/ "吕坤的github博客")。然后了解到可以使用github来写博客，觉得很好玩，就打算试一试。
<!--more-->

我不像[吕坤][zhihu.lvkun]对[Ruby][]那么大意见，所以装了运行Jekyll的一切必需品，现学[Git][]（这个早就听说了，一直没用到所以没学）、[Jekyll][]、[Markdown][]……折腾了一个星期天，终于可以显示博客了。正高兴呢，忽然发现服务器不正常了。

经过一番查找，发现是因为[Jekyll]运行时使用默认环境的地区编码信息，所以不识别UTF-8编码中的中文。从网上搜到Windows下可以使用`chcp`来改变语言编码，于是试了试`chcp 65001`，发现运行

	jekyll --pygments --safe --auto --server

之后没有任何显示。遂放弃。

继续查，发现哪怕在Windows下，只要设置`LANG`和`LC_ALL`后，便可以设置的编码运行。于是

{% gist 1972602 %}

然后再次运行`jekyll`，**正常了！**

于是才有了此篇日志。

----
本来用[Jekyll][]的日志已经做好了，但是发现无法从手机上正常浏览。又懒得去研究怎么调整HTML、CSS那一套东西。正一筹莫展之际，发现了[Octopress][]这个东西。于是毅然变节，改投[Octopress][]，于是有了现在的后半段。

[Octopress][]比较有趣，会把代码放在`source`目录下，把生成的网站放在`_deploy`目录下。（当然所有目录都可以通过设置修改。）然后会自动将`_deploy`目录作为github page的`master`分支，将包含source在内的部分作为github page的`source`分支。

对[Git][]还不是特别了解，或许领会不到如此分离的真谛，但隐约能感觉到这样做会使Repository里的结构清晰很多。


[zhihu.lvkun]: 	http://www.zhihu.com/people/lv-kun				"吕坤的知乎主页"
{% include links %}
