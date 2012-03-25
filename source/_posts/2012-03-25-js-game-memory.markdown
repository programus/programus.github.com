---
layout: post
title: "Javascript游戏——Memory"
date: 2012-03-25 17:47
comments: true
categories: 
- computer
- software
- javascript
- game
---
前段时间看了《JavaScript语言精粹》，一直想实践一下。于是利用了3、4天的时间做了一个记忆力游戏——[Memory](http://programus.appinn.me/jsgames/memory-single.html)。
<!--more-->

先在这里放一个直接可玩的版本：

<iframe id="memory-game" width="350" height="380" src="http://programus.appinn.me/jsgames/memory-single.html"></iframe>

看书的时候随随便便就看过去了，实际做的时候才总是感到书到用时方恨少。书中提到的一些常见问题还真是很常见，在实际编写程序的过程中很多都碰上了。

游戏最终在网上发布时使用的文件——[memory-single.html](http://programus.appinn.me/jsgames/memory-single.html)——是一个整合了所有内容的文件，直接转存到本地后，单独一个文件就可以展现整个游戏内容。甚至包括所有的声音和图片。

对于图片，使用了[Data URI Schema](http://en.wikipedia.org/wiki/Data_URI_scheme)的技术，将图像的二进制内容转码成Base64编码，以文本形式写在html文件中。不过，本游戏中一共就用了两个图片：作为favicon的图标和下拉列表框的自定义箭头。

对于声音，使用了Pedro Ladaria做的[代码生成声音的库](http://www.codebase.es/riffwave/)。我在[CSSer][]上放了一个[贴板](http://www.csser.com/board/4f6ab5086e9e4f0817000066#/post/4f6ab6462af57d0a17000020/more)，里面贴了声音库的使用相关链接。

最后，说一下自定义风格的下拉列表框。参考了[这篇文章](http://bavotasan.com/2011/style-select-box-using-only-css/)，使用外框的标签来切断`<select>`的内容，将右边的箭头排除到外框标签的外面并遮挡住。其中不同的是，我在这里使用的外框是`<span>`而不是`<div>`。最初使用`<span>`的时候，没有成功，后来在网上查询才知道只有`block`级的标签才有效，而`<span>`不是。但加上`display:inline-block`就可以如`block`级标签一般工作了。

说了这么多，最后附上代码。

<div title="其实，我是个按钮" style="cursor:pointer;" onclick="if($('#source-block').css('display') === 'none') {$('#source-block').css('display', 'block');$('#show-hide').text('隐藏');}else{$('#source-block').css('display','none');$('#show-hide').text('显示');}">[<span id="show-hide">显示</span>代码部分]</div>

<div style="display:none;" id="source-block">
{% gist 2192639 %}
</div>

{% include links %}
