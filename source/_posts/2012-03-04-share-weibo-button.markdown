---
layout: post
title: "为Octopress追加[分享到微博]按钮"
date: 2012-03-04 20:11
comments: true
categories: 
- octopress
- jekyll
- computer
- weibo
---
昨天加上了[微博][]的侧边栏。今天发现每篇微博下面还有一个`Tweet`按钮。还是那句话，在[墙][]后面，[Twitter][]用不到，所以[微博][]的分享按钮不可缺少。

<!--more-->
有了昨天的经验，今天也就没去网上查什么资料。直接去[新浪微博][]找到[分享按钮][]的页面，生成一个按钮代码。

{% gist 1972564 %}


仔细看看生成的按钮，虽然上面写了一大堆JavaScript，但实际有用的就是最后一行`document.write('...')`里面的HTML代码。但这里HTML的主要部分都是用JavaScript生成的。于是祭出网页分析神器——[Firebug][]，直接分析[分享按钮][]页面里例子的HTML代码。从中提取出如下URL（为了便于阅读，格式做了调整）——

```
http://hits.sinajs.cn/A1/weiboshare.html?
  url=http%3A%2F%2Fopen.weibo.com%2Fsharebutton&
  appkey=&
  type=6&
  ralateUid=1098907490&
  language=zh_cn
```

其中各个参数的意思，参看生成的JavaScript的注释，基本一目了然了。下一步，嵌入代码到博客下面的分享栏里。

经过一番查找，分享栏的代码存储在`source/_includes/post/sharing.html`中。果断加入代码：

{% gist 1972657 sharing.html %}

其中最后一行是自己调整了一下看着顺眼的格式。

同时要在`_config.yml`文件中加入`weibo_share`的设定。

{% gist 1972657 _config.yml %}

最后，为了更换主题时不会出问题，对`.theme/<所用的主题名>/source/_includes/post/sharing.html`也做了相应的修改。

最后，生成网页、启动服务器测试——

```
rake generate
rake preview
```

查看预览网页<http://localhost:4000>……**按钮加入成功**！

[分享按钮]:	http://open.weibo.com/sharebutton	"新浪微博分享按钮"

{% include links %}