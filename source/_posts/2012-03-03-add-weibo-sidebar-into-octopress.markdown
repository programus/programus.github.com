---
layout: post
title: "为Octopress博客追加新浪微博侧栏"
date: 2012-03-03 21:43
comments: true
categories: 
- octopress
- jekyll
- computer
- weibo
---
配置好了基于Octopress的博客后，发现其右侧边栏（位置自然根据主题会有不同）上有[Twitter][]等内容。因为[墙][]的原因，[Twitter][]始终使用起来不够方便，所以还是[新浪微博][]用的更多。于是，就想在博客里弄一个[微博][]的侧边栏。
<!--more-->

{% img left /images/posts/octopress-weibo.png  %}
### 实现设想 ###
[微博][]本身提供了一个嵌入侧边栏的工具，叫[微博秀][]。于是便想是不是简单嵌入即可呢？

### 其他尝试 ###
但对稍有点费劲的工作，当然首先是上网搜索一下是否有现成方案。果然找到了一篇文章——《[新浪微博侧栏widget定制](http://blog.tingkun.com/blog/2011/11/05/xin-lang-wei-bo-ce-lan-widgetding-zhi-octopress/)》。

仔细查看了一下代码，发现作者好粗心，代码里居然没有用变量，而是把自己的用户名直接硬写进去了。另外，貌似只实现了`微博名片`和`关注按钮`。而且因为使用了[微博][]的JavaScript API，所以需要一个APP ID。

### 回归 ###
我是懒人，不想搞得那么复杂。于是继续回到自己最初的方案——嵌入[微博秀][]。

先到[微博秀][]里面生成自己的微博秀嵌入代码。

```
<iframe 
	width="100%" 
	height="550" 
	class="share_self"  
	frameborder="0" 
	scrolling="no" 
	src="http://widget.weibo.com/weiboshow/index.php?
		width=0&
		height=550&
		fansRow=1&
		ptype=1&
		speed=0&
		skin=2&
		isTitle=0&
		noborder=1&
		isWeibo=1&
		isFans=1&
		uid=1098907490&
		verifier=abd54ad9&
		dpc=1">
</iframe>
```

仔细一看，其实就是一个大URL，尝试一下各种参数。最终写了一个`weibo.html`，放到了`source/_includes/asides`下面。貌似在`.theme/<所用的主题名>/source/_includes/asides`下面放着更好，因为切换主题时不会丢失。

{% gist 1966517 weibo.html %}

同时，在`_config.yml`中加入相关设定——

{% gist 1966517 _config.yml %}

其中的`weibo_uid`和`weibo_verifier`是从[微博秀][]生成的代码中取得的，其它则是显示设定。

至此，搞定了微博的嵌入。


[微博秀]:	http://weibo.com/tool/weiboshow		"微博秀"

{% include links %}
