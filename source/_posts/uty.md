---
title: Undertale Yellow 汉化技术问题记录【1】
date: 2023-12-11 19:15:15
tags: 
 - Undertale
 - Undertale Yellow
 - 汉化
---

昨天开始做Undertale Yellow的汉化，这个项目里面是真的重量级啊

## 安装字体

字体倒是没什么好说的，项目里字体不多
不会像之前某些项目 好几个一模一样的字体不同名字
直接覆盖上字体就行
但是这次的 Determination Sans+方正像素 空格超级小
一个英文字符对应三个空格
so为了美观 只能把*后一个空格后面换成两个空格 换行后两个空格换成五个空格
不仅是空格超级小 间距也超级小
所以中文不加空格超级不好看（

## 修改打字机
然后就是喜闻乐见的猜打字机名字时间
我搜了typer和writer都没搜到
然后我决定先从有打字的地方开始下手
比如开始的故事intro
我搜索了 story和intro
intro有了结果 obj_intro
然后我打开绘制事件一看
我测 为什么打字逻辑写在这
好在这套打字逻辑不复杂 而且不需要改
我这时还以为只有intro是这样有自己的打字逻辑

然后我决定找找战斗打字机
当我搜出dialogue_battle的时候
重量级出现了
![dialogue_battle](./resources/images/uty/dialogue_battle.png)
是的 每一处打字机都是一个物体
然后我又搜索了dialogue
更加印证了我的想法
我真是【Swearing Blocked】了
不是 你猜猜「初始化代码/Creation Code」有什么用

不过幸运的是 打字机并没有太大问题 打字机使用的是整体draw
所以字距会由GM调用字体的字距 目前来说没啥要改的
唯一一个有问题的是战斗框打字机 它是for文本的每一个字符 固定offset单字绘制
而那么多的dialogue_battle其实都只指向四个函数
所以修改了这四个函数 让固定offset改为根据当前字符的width来设定 就弄好了

## 开整
首先是开头的故事intro
我找到了文本 翻译好了 一切正常
![intro](./resources/images/uty/intro.png)
纸鸢提供了那张寻人启事的图
然后就是logo下方的\[PRESS Z OR ENTER\]
很难绷的就是 我翻译好后发现 这个文本 是手动设置的偏移
它没有使用GM自带的居中对齐绘制
所以我的决定是 直接修改draw函数
byd谁闲着给你一点一点调位置
![原来的](./resources/images/uty/z_enter_original.png)
↓改完之后↓
![改进的](./resources/images/uty/z_enter_improved.png)

然后就是menu和instruction
翻译了menu后 我发现找不到instruction的文本在哪
此时我还调侃说不会是个贴图吧
然后我还就真找到了
![instruction](./resources/images/uty/instruction.png)
不是 你没病吧 你和RickyG学的是吧
对此 我的选择是
直接给你字删了然后手动代码绘制一份
![最终效果](./resources/images/uty/menu.png)

## Overworld
然后就是ow的测试
首先是测试了有无空格的差别
![无空格](./resources/images/uty/no_space.png)
![有空格](./resources/images/uty/with_space.png)
显而易见了 所以就要求必须打空格

接着就是文本的上色
前面说过 为了美观 把空格的数量改了
然后uty的上色 居然是一种颜色用一个打字机
就是说 你加一个颜色 就要新开一个打字机 然后在上色词语的前面加与主打字机一样字符数量的空格
![炸了](./resources/images/uty/color.png)
↑比如这里一共是三个打字机↑
而且这个东西的打字速度还是一样的
就是说有中文的时候 上色会跟不上或者是太快
这个问题还没修好 目前可能的修复方法是找一个显示空白但是占地和中文一样的字符写进去

## 战斗
纸鸢提供了战斗的按钮贴图
没啥问题 接着就是战斗框打字机
前面说过 战斗打字机得改
然后这里有一张打字机没改时的珍贵录像（
![battle](./resources/images/uty/battle.png)
anyways，把打字机改好之后和ow一样的处理方法即可

## 待续
嗯，目前就是这样
后续有新玩意我再发篇post（