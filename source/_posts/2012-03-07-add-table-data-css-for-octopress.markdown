---
layout: post
title: "为Octopress追加数据表格的CSS"
date: 2012-03-07 21:56
comments: true
categories: 
- octopress
- jekyll
- computer
- css
styles: [data-table]
---
昨天恢复了一个[旧博客]({% post_url 2010-11-15-tiny-countdown %})，里面包含了一个数据表格。 

但[Octopress][]的默认表格是不具有边框的，在看数据表格时会很难看。

于是，对[Octopress][]做了一番剖析，追加了针对数据表格的CSS格式，并允许在博客的内容文件中选择是否使用数据表格。
<!--more-->

要做到这一点，首先得先找到原始CSS的所在，在Firefox中[Firebug][]的帮助下，很顺利地找到了位于`source/stylesheets`的`screen.css`文件。

打开一看，傻眼了。居然是紧凑型CSS，都堆在一起了。于是上网找了个CSS格式化工具——<http://www.cssportal.com/online-css-editor/>。

当CSS格式化后，查看了一下里面对`table`、`th`、`td`的格式定义。发现确实将`border-width`设置成了`0`。

看来只有覆盖它们了。于是写了一个`data-table.css`，同样放在`source/stylesheets`下面。

{% gist 1993032 data-table.css %}
（感谢[陈堰平][]在评论中的指正，CSS文件修正了。修正详情在文章最后。）

然后找到引入`screen.css`的文件，在其后引入`data-table.css`。这个文件是`source/_includes/head.html`。但为了保证没有数据表格的博客还能继续使用原本的表格风格，在里面加了少许条件。

{% gist 1993032 head.html %}

这样，只要在博客文件的[`yaml front matter`](https://github.com/mojombo/jekyll/wiki/yaml-front-matter)部分里面加入`styles: [data-table]`，就可以让数据表格用的表格风格生效了。

要在博客里插入表格，则可以使用如下格式。

```
左对齐表头 | 中间对齐表头 | 右对齐表头
:----------|:------------:|----------:
左对齐数据 |中间对齐数据  |右对齐数据
第二行数据 |也是第二行    |还是第二行
懒得写了...|.....         |.....
长数据，以便看出表头的对齐|长数据，以便看出表头的对齐|长数据，以便看出表头的对齐

```
就会得到如下表格：

左对齐表头 | 中间对齐表头 | 右对齐表头
:----------|:------------:|----------:
左对齐数据 |中间对齐数据  |右对齐数据
第二行数据 |也是第二行    |还是第二行
懒得写了...|.....         |.....
长数据，以便看出表头的对齐|长数据，以便看出表头的对齐|长数据，以便看出表头的对齐

其中的左右对齐，取决于表头下面一行中的冒号的位置。

----

###2012-03-19补充：###

感谢[陈堰平][]的指正，之前的CSS会连同代码框都改掉。查看了一下HTML代码，发现代码框的代码有如下特点：

* 嵌在`<div class="highlight">`或`<div class="gist-highlight">`中
* `<td>`标签有`code`或`gutter`这两个class
* `<td>`的子标签是`<pre>`

本想用以上中的某种来区分对待CSS，但CSS中不支持通过排除某class（CSS3中有了`:not()`，但CSS3未被全部浏览器支持）或指定父标签来进行选择。由于对CSS也不是很熟悉，所以重新有学习了一次CSS选择器相关的知识，其中发现一个`+`选择器，可以通过在同级出现的标签来指定。这时发现了代码框的又一个特点：

* `<table>`标签是孤独的（没有兄弟节点）

但数据表格必然是配合文章出现的，前后通常都会有些兄弟标签。（即使没有也可以自己追加一个空`<span>`或`<div>`来解决。）于是，使用`* + table`来做选择器选出非代码的表格——也就是数据表格——从而完成了数据表格特定CSS的实现。

[陈堰平]: http://chen.yanping.me/	"陈堰平的个人网站"
{% include links %}