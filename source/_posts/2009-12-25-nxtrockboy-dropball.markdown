---
layout: post
title: "NXTRockBoy上的第二个原创游戏——Drop Ball"
date: 2009-12-25 11:21
comments: true
categories: 
- robot
- lego
- nxt
- java
---
前一段时间工作很忙，本职工作很忙。后来，集中精力制作了第二款NXTRockBoy的游戏——Drop Ball。顺便说一下，第一款游戏在[这里]({% post_url 2009-10-25-nxtrockboy-bball %})。
<!--more-->

Drop Ball是一款什么样的游戏呢？

大家记得有一个男人系列的游戏吧。其中有一个是“是男人就下一百层”。好，Drop Ball就是这个游戏的摇摆游戏机版。只不过把那个倒霉的小男孩换成了我们的主角——小黑球。玩那个游戏按箭头按到手疼的同学们，现在可以换一种方式了。

在这个游戏中，继续发挥了NXTRockBoy的摇摆操作特性，小球仍然是受到重力的影响，而且依旧具有惯性。不要以为你把NXTRockBoy往左转，小球就往左跑，如果他之前有一个右方向的速度，他是要先进行减速的。所以，游戏一如既往地很有难度，不过可玩性提高了很多。

另外，作为第二个游戏，总要有些突破。这个游戏中增加了两个重要特性——

第一个——背景音乐支持！在不影响小球那个滴滴答答的音效的基础上增加了背景音乐支持。系统中内置了三段音乐——Final Countdown、Smooth Criminal、变形金刚动画片主题曲。如果你想要自定义，只要读读我的[帮助](http://code.google.com/p/nxtprojects/wiki/NXTUtils)中的音乐支持部分，自己写一段代码即可。

第二个——环境光支持！如果有人组装了NXTRockBoy，一定对上面安装的颜色/光传感器和超声传感器感到奇怪。因为上一个游戏压根没用到这两个东西。这一款游戏用到了颜色/光传感器（虽然超声传感器还是遭到冷遇）。游戏中平台的相关参数会根据游戏机所在地的环境光的强度改变。

游戏有两个模式——`|`模式和`-`模式。`|`模式下，光线强，则平台间距小；`-`模式下光线强，则平台宽度大。

好，看了这么多，大家一定期待着看视频了。那就如期上视频！

<embed src="http://player.youku.com/player.php/sid/XMTM5OTEwNDA0/v.swf" allowFullScreen="true" quality="high" width="480" height="400" align="middle" allowScriptAccess="always" type="application/x-shockwave-flash"></embed>

[视频网址](http://v.youku.com/v_show/id_XMTM5OTEwNDA0.html)

最后，也是最重要的：

搭建图及程序下载和说明，请看这里：

<http://code.google.com/p/nxtprojects/wiki/DropBall>

{% include links %}