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
昨天恢复了一个[旧博客](../../../../2010/11/15/tiny-countdown/)，里面包含了一个数据表格。 

但[Octopress][]的默认表格是不具有边框的，在看数据表格时会很难看。

于是，对[Octopress][]做了一番剖析，追加了针对数据表格的CSS格式，并允许在博客的内容文件中选择是否使用数据表格。
<!--more-->

要做到这一点，首先得先找到原始CSS的所在，在Firefox中[Firebug][]的帮助下，很顺利地找到了位于`source/stylesheets`的`screen.css`文件。

打开一看，傻眼了。居然是紧凑型CSS，都堆在一起了。于是上网找了个CSS格式化工具——<http://www.cssportal.com/online-css-editor/>。

当CSS格式化后，查看了一下里面对`table`、`th`、`td`的格式定义。发现确实将`border-width`设置成了`0`。

看来只有覆盖它们了。于是写了一个`data-table.css`，同样放在`source/stylesheets`下面。

{% gist 1993032 data-table.css %}

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

{% include links %}