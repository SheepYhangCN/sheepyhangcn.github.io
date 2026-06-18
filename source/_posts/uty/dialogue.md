---
title: Undertale Yellow 汉化技术问题记录【4】
date: 2024-02-11 22:10:43
updated: 2024-02-11 22:10:43
categories: [研究记录]
tags:
 - UNDERTALE
 - Undertale Yellow
 - 汉化
 - GameMaker
---

之前说过uty的彩色打字机非常弱智
是多个打字机叠在一起
然而打字进度对不上这个问题还没解决 又出来了新问题

## 细说
so，说是多个打字机，其实也不尽然
其实是一个主打字机和多个子打字机
子打字机的打字进度和字符数量和主打字机共享
没错，发现问题了吗
一旦子打字机的字符数量超过主打字机
那么子打字机超出去的部分就不会被打出来
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/cut.png  ratio:640/478 %}

## 解决？
uty这个打字机真的看得我小脑肿大 大脑萎缩 脑干打结
打字机本身的文本是一维数组 而颜色打字机的文本是二维数组（因为要分颜色 不同颜色不是同一个打字机）
然后就因为分数组这个问题 导致我不能轻易地把字符数量改为max函数取打字机的最大值

最后是非常弱智的解决方法：直接把主打字机的打字进度转换为百分比给颜色打字机用
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/resolve.png  ratio:735/33 %}
当然这样的后果是非常大概率会导致打字机速度不统一
但是既然之前都不同步了 就不管了
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/resolved.png  ratio:640/478 %}
嗯 就这样罢

## 解决
然后 嗯 bug了
疑似是utmt反编译时吞了码 然后编译少了码导致的
首先是choice的灵魂持续出现在底下
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/soul.png  ratio:148/39 %}

然后还有choice出现过后choice的选项不会消失 一直显示
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/choice.png  ratio:657/519 %}

最后解决是 在data里找到了一个空的if
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/if.png  ratio:344/63 %}
而且后面正好跟着的就是choice相关的代码
所以就补了个else上去
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/else.png  ratio:363/92 %}

然后就 真修好了 嗯

## 题外话
之前在翻data的时候发现了些震撼人心的东西
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/r1.png  ratio:1069/679 %}{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/r2.png  ratio:1123/838 %}{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/r3.png  ratio:1024/825 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/rr.png  ratio:293/152 %}
非常弱智的随机对话系统
Powered with ```else if```™
他们甚至不肯用```switch```

但是
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/uty/dialogue/rrrr.png  ratio:1062/366 %}
你这不是会用```asset_get_index```吗
那你前面怎么不用
只能说跨越7年的项目 就这个b样子了
uty团队的程序员实力也参差不齐
代码看得我脑壳疼