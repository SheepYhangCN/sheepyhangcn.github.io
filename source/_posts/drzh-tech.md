---
title: DELTARUNE 汉化技术解析
date: 2026-06-18 22:44:00
repo: gm3dr/DeltaruneChinese
tags: 
 - 汉化
 - DELTARUNE
 - GameMaker
---

全文初版完成于 2026 年 6 月 9 日前，且原本撰写作为视频台本
本文内容很可能仅适用于 6 月 9 日前的源码，具体最新情况请查阅仓库 README
本文内容基于 260401 之后的开发中版本补丁
具体为 [2026 年 6 月 4 日 15:28 的提交](https://github.com/gm3dr/DeltaruneChinese/tree/7d0021bb0fa57be4018ab24595ba42f103764646)（UTC + 8 / 北京时间）
本文内容基于 v2.4.2 与 v2.5.0 之间的开发中版本安装器
具体为 [2026 年 6 月 6 日 14:41 的提交](https://github.com/gm3dr/DeltaruneChinesePatcher/tree/cbf05b8cf791d9faf3a3547f9b121e4cc5c60b92)（UTC + 8 / 北京时间）
{% folding 本文视频版 %}
{% video bilibili:BV188Ef6aEu5 %}
{% endfolding %}
{% button 好人汉化组 https://space.bilibili.com/3546893244172492 %}
{% button 补丁源码仓库 https://github.com/gm3dr/DeltaruneChinese %}
{% button 安装器源码仓库 https://github.com/gm3dr/DeltaruneChinesePatcher %}

# P1 技术铺垫 & 补丁源码仓库
首先，DELTARUNE 中文化项目分为补丁与安装器
所有的源码自从补丁发布以来就是开源的了
这两个仓库都有对应的开源协议，安装器完全使用 [MIT 协议](https://github.com/gm3dr/DeltaruneChinesePatcher/blob/master/LICENSE)
而补丁由于各种因素限制，请查阅 [README](https://github.com/gm3dr/DeltaruneChinese/blob/main/README.md#%E5%8D%8F%E8%AE%AE-license) 了解具体的开源协议

先从补丁开始，补丁方面的技术主要是 yig 与 ws3917 负责，在此感谢他们的付出
打开源码的 Github 仓库
选择 Code → Download ZIP 即可下载到本地
你可以阅读下方的 README 来了解源码结构

## 技术铺垫
在开始讲内容之前，得先来点技术铺垫
DELTARUNE 是使用 GameMaker 制作的游戏，所以游戏的所有资源与代码都存在一个叫做 data.win 的文件中，一直到一二章 demo
{% note 在不同操作系统中，这个文件不一样 例如苹果系都是 game.ios，安卓是 game.droid %}
如果有人拆过一二章 demo 的 data 就会发现，很明显一二章是分别开发的
因为有很多东西有两份，比如一二章的打字机是分开的两个物体，甚至功能上还有差别
于是 Toby 的屎山代码顶不住了，他就联系了 GameMaker 的制作方 Yoyogames
于是 yyg 就为了 Toby 一个人，给 GameMaker 加上了一个可以在游戏内切换到其它 data.win 的功能
{% swiper effect:coverflow %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p1/1.png width:800px ratio:1625/1103 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p1/2.png width:500px ratio:712/900 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p1/3.png width:200px ratio:336/628 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p1/4.png width:400px ratio:396/335 %}
{% endswiper %}

{% note 个人意见不代表好人组 不用跟我犟什么社群也要用，不是为了 Toby 一个人，只是巧合什么的<br>Toby 专门发了 newsletter 提了专门为了他，然后 yyg 也专门在更新注记里提了专门为了 Toby<br>这 yyg 是真的臭味相投，有人走后门加功能还这么光荣，社群报的 Bug 一个不修😅 %}
于是三四章更新后，目前的 DELTARUNE 也就有了四个章节 + 章节选择一共五个 data
所以中文化补丁也需要对这几个 data 分别修改

## 为什么不直接配布 data
如果有人使用过以前的 undertale 中文化，以及 DELTARUNE 一二章中文化的话，应该还记得
当时的补丁是直接配布 data.win，覆盖进去
为什么现在不这么做了呢
其实答案很简单：由于 GameMaker 的特性，配布 data.win 约等于配布整个游戏，只是缺少了外置的音频
只要找一个 GameMaker 版本相同导出的 exe，就可以直接启动游戏
也就是说，这是一种盗版行为，也是好人汉化组一直以来不提倡不支持的行为，同时也会有法律风险
所以，从三四章开始，我们采用了另一种方案： XDelta 二进制差分
如果有游玩过 undertale 模组的应该很熟悉，例如 Bits and Pieces 与其的中文化就是使用的这种方案
这种方案是基于原版和修改之后的文件生成它们之间的差分文件
举个例子：如果我们要把 123 变为 234，那么我们就可以生成一个 `-1+4`，这就是差分
不过由于目前的 DELTARUNE 有五个 data，如果还像那些 Mod 一样用工具手动打补丁就太麻烦了
所以我们制作了一个一键安装器，这个安装器的技术解析在[后文](#P4-安装器)

## 补丁源码仓库
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p1/5.png width:200px ratio:208/312 %}
现在我们就可以开始了
首先是比较简单的几个目录
`atlas_packer`，这是一个 Node.js 包，用于生成纹理页
我们是将所有中文化的贴图构建为纹理页
再添加到游戏文件中，这样不受大小限制比较方便，特别是第三章 Tenna 的艺术字属实是很麻烦
`bin`，这里是用于生成中文版 data 的主要程序，源码在 `src` 目录，具体细节我们之后会讲到
`misc_scripts`，这里是一些之前用到的 Python 脚本，例如比较贴图差异，替换人名翻译等
`node_modules`，这个是 Node.js 的依赖包目录，不用管
`prproj`，这里是第三章 Tenna 小视频与其中一条宣传片的 Premiere 工程，在此不赘述，详看 README
`tool`，这是构建过程中用到的工具，包括生成字体用的 bmfont，生成文件差分用的 xdelta3，生成压缩包用的 7z

## workspace
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p1/6.png width:200px ratio:231/232 %}
接下来就是重点的两个，workspace 与 `src`，我们先从 `workspace` 开始
这里就是每个章节所用到的文件，`chX` 是四个章节，`main` 是章节选择
`demo` 是一二章 demo 版，`global` 是通用的，目前只有人名翻译替换表
`result` 就是构建结果
考虑到结构相似，我们以第三章为例子

打开后有一个 `imports` 一个 `vid`
`imports` 就是主要的目录，而 `vid` 是只有第三章有的，那个 Tenna 初见视频
是的，这个视频是直接放置在游戏目录里的 mp4
有两个视频，分别是人名翻译和不译，带 `zhname` 与不带的
打开 `imports`，这里有很多目录
`atlas`，这个不用动，是在修改了贴图之后使用 `atlas_packer` 生成的纹理页
`code`，这里是所有中文版本修改过的 GML 代码，你可以在 README 里面看到所有改动的介绍
`font`，这里是字体，`bmfc` 是提前使用 bmfont 手动生成的
`pics` 则是使用打包器从 data 中导出的英文字体，主要是 `font` 这个目录
这里便是使用的所有字体，第四章是最多的，对应的字体名同样可以查看 README
`pics` 与 `pics_zhname`，这里便是中文化的贴图，分别是人名不译与翻译的
然后便是 `text_src`，这里就是文本

## 构建汉化流程
如果你想要构建汉化，那么流程如下
首先，确认当前目录没有非 ASCII 字符，就是不能有中文与特殊字符，不然会报错，因为用到的工具 bmfont 比较老
然后，把对应的原版 data 放置到对应章节目录下，和 `imports` 同级目录
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p1/7.png width:600px ratio:762/239 %}
接着，打开终端，将 `bin` 下方的 `deltarunePacker.exe` 拖进去，打个空格，再把 `workspace` 目录拖进去
最后，汉化后的 data 将生成在 `result` 目录下
将 data 覆盖到游戏目录，将 `lang` 开头的文件放置到游戏目录下 `lang` 目录中即可
不要漏了第三章的视频

# P2 GML 代码
各种硬编码到代码中的文本，以及各种位置微调，这个就不用多说了
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/1.png obj_splashscreen_Create width:600px ratio:1431/509 %}

## 打字机
首先是打字机，我们修改了打字逻辑，增加了字符宽度与字间隙
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/2.png obj_writer_Draw width:600px ratio:1920/1080 %}
如果有使用过一二章汉化的应该会发现，新的字间隙更大，看着更舒服了
但是又没有 undertale 汉化的字间隙那么大
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/3.jpg 上到下分别为：UT，第四章，旧版第二章 width:600px ratio:1920/1080 %}

另外，我们还专门修改了第一章的打字机
在默认情况下，打字机都使用 `&（与符号）` 来换行
而 GameMaker 的绘制默认使用 `\n` 换行
从第二章开始，打字机也支援了 `\n` 换行，为了方便我们处理文本，我们将这个功能带回了第一章
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/4.png obj_writer_Draw width:600px ratio:1227/545 %}

以及后续章节的打字机还有一个 `` `（反单引号）`` 保留后方特殊字符的功能
打字机有一些用到特殊字符功能，例如前面提到的&换行等
如果我们想在文本里使用这些字符，就可以用这种方式来正常打出
我们同样将这个功能带回了第一章
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/5.png obj_writer_Draw width:500px ratio:594/260 %}

## 外置文本
DELTARUNE 的多语言使用下面这六个方法来获取本地化文本
```
msgnextloc(原文, key)
stringsetloc(原文, key)
msgsetloc(msg_id, 原文, key)
msgsetsubloc(原文[, 参数], key)
msgnextsubloc(原文[, 参数], key)
stringsetsubloc(原文[, 参数], key)
```
需要输入英文的字符串与对应的 key，英文时直接把这个字符串原封返回
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/6.png 举例，obj_ch3_closet_Step width:600px ratio:904/230 %}
非英文时会调用 `scr_84_get_lang_string` 方法
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/7.png msgsetloc width:600px ratio:503/187 %}
从启动游戏时储存的 `lang_map` 中取出文本
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/8.png scr_84_get_lang_string width:500px ratio:995/307 %}
判断非英文使用的是 `is_english` 方法，我们去掉了其判断语言的部分
只保留了检测 `global.lang` 是否存在的部分
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/9.png 原版&nbsp;is_english width:600px ratio:553/73 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/10.png 修改后的&nbsp;is_english width:600px ratio:545/185 %}

## 禁用自动切换日文字体
然后是改动了 `scr_kana_check`
这个脚本会检查文本里是否有日文假名
如果有，就返回 `true`
然后各个地方的逻辑就会切换对应字体为日文
但是这个脚本的检查方式是直接检查字符码位，会匹配到汉字
所以我们把这个脚本改为了只会返回 `false`
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/11.png scr_kana_check width:400px ratio:714/454 %}

## 人名翻译切换 & 禁用日文
接着便是三四章开始，增加的游戏内切换人名翻译功能
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/12.png obj_initializer2_Create width:600px ratio:538/140 %}
首先是设置了一个 `global.names` 的变量用于存储这个设置
`0`为不译 `1`为翻译可招揽 `2`为全部翻译
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/13.png scr_84_init_localization width:400px ratio:708/294 %}
把这个设置存储到 `true_config.ini` 中

{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/14.png scr_84_init_localization width:300px ratio:891/967 %}
在日文版中，大部分贴图与字体切换都是借助这里的 `sprite_map` 与 `font_map`
所以我们也把本身有日文差分的贴图的人名翻译差分写在了这里
很可惜的是这总归是少数，只有战斗介面的三个人名用了这个系统
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/15.png scr_84_init_localization width:300px ratio:804/533 %}

同时，因为读档介面的语言按钮被人名翻译选项替换了
而且还有很多利用了日文的修改方式
所以我们决定彻底禁用日文
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/16.png scr_84_init_localization width:400px ratio:697/386 %}
无论前面加载到什么语言，都会强制被切换回英文
防止有人之前设置过日文，或是在章节选择选单里设置了日文而出 Bug
（我们没有改动章节选择选单里面的语言选项）

然后就是读档介面的修改
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/17.png 读档选单 width:400px ratio:835/153 %}
把下方这个语言按钮替换为了人名翻译的选项
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/18.png DEVICE_MENU_Draw width:400px ratio:979/789 %}
每次点击会调用 `scr_change_language`
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/19.png DEVICE_MENU_Step width:400px ratio:527/364 %}
所以我们也把这个脚本里面的内容换成了修改人名翻译设置，而非原来的切换语言
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/20.png scr_change_language width:400px ratio:637/497 %}
原本设定语言按钮的位置是放不下四个字的
所以为了放得下，我们手动修改了所有按钮与灵魂的位置
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/21.png DEVICE_MENU_Draw width:300px ratio:582/651 %}

同时增加了 `names_countdown` 变量
当 `names_countdown` 大于 `0` 时就会持续 `-1`，且会在左下角绘制文本
每次修改人名翻译的选项就会设置为 `90`
DELTARUNE 以 `30` 帧运作，也就是三秒
也就是每次点了按钮就在左下角显示三秒钟的文本
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/22.png DEVICE_MENU_Draw width:400px ratio:1096/535 %}

## 招揽伙伴选单
自从第二章开始，存档介面新增了查看招揽伙伴的功能
这个在英文版中是被横向压缩了的，因为写不下
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/23.png 伙伴介面 width:400px ratio:1280/960 %}
但是中文和日文是写的下的，所以按照日文的值来
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/24.png obj_fusionmenu_Create width:400px ratio:511/280 %}

## 地图内人名翻译
第二章与第四章的光世界地图有两处出现人名
Sans 的店铺招牌与学校里黑板上的 Toriel
{% swiper %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/25.png Sans&nbsp;的店铺招牌 ratio:1280/960 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/26.png 学校黑板上的&nbsp;Toriel ratio:1280/960 %}
{% endswiper %}
这两处因为不是物体，而是地图的一部分，所以一般情况下非常麻烦
好在这两处日文同样需要翻译，而在地图上设置了专门的层
会在日文时显示出来并盖住原有的英文
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/27.png room_torielclass width:400px ratio:1206/763 %}
所以我们就借助了这些层，直接将其的贴图替换为人名翻译的贴图
同时也修改了对应的逻辑
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/28.png obj_84_lang_helper_Create width:400px ratio:908/622 %}

## 贴图人名翻译
除此之外还有普通的贴图人名
{% box 第二章&nbsp;Pipis %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/29.png obj_pipis_enemy_Draw&nbsp;与&nbsp;obj_pipis_egg_bullet_Draw ratio:1477/462 %}
{% endbox %}
{% box 第三章&nbsp;Ramb %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/30.png obj_room_green_room_Create ratio:1322/122 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/31.png room_dw_green_room width:400px ratio:946/689 %}
{% endbox %}
{% box 第三章&nbsp;T&nbsp;级介绍 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/32.png obj_dw_ranking_t_explain_Draw width:600px ratio:620/204 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/33.png spr_dw_ranking_t_explain width:600px ratio:946/360 %}
{% endbox %}
{% box 第三章&nbsp;Rouxls&nbsp;的飞机拖旗 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/34.png obj_rouxls_biplane_flag_Draw width:600px ratio:625/218 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/35.png spr_rouxls_biplane_flag ratio:960/162 %}
{% endbox %}
{% box 第三章&nbsp;Tenna&nbsp;初见视频 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/36.png obj_ch3_couch_video_Create ratio:1133/220 %}
{% endbox %}
人名翻译切换相关的改动大概就是这些

## 单独的修改
{% box 第二章初次进入&nbsp;Cyber&nbsp;City&nbsp;时的牌子 %}
是按照当前位置，逐字亮起的
所以需要按照汉字的位置重写
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/37.png obj_welcometothecity_backinglights_Draw，右侧绿色的都是原本的代码 width:300px ratio:736/818 %}
{% endbox %}
{% box 第三章的音游歌词 %}
改为了按照日文的方式来显示字间隙
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/38.png scr_rhythmgame_lyrics ratio:625/124 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/39.png scr_rhythmgame_lyrics ratio:943/384 %}
{% endbox %}
{% box 第三章&nbsp;Tenna&nbsp;的长难句 %}
因为中文字少，所以持续时间也短
所以就加长了这个循环，加长了重复的时长
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/40.png scr_rhythmgame_lyrics width:400px ratio:885/606 %}
{% endbox %}
{% box 第三章的饮水机 %}
具体的介绍可以看[这期专栏](https://www.bilibili.com/read/cv42255231)，里面有提到这里的改动
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/41.png scr_rhythmgame_lyrics width:400px ratio:1424/687 %}
{% endbox %}
{% box 第三章&nbsp;Rouxls&nbsp;战的彩蛋 %}
这个文本在日文是贴图，英文是文本
为了方便与美观，中文也用了贴图的形式
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/42.png obj_rouxls_annyoing_dog_controller_Draw width:400px ratio:1036/617 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/43.png spr_txt_ja_annoyingdog width:400px ratio:851/528 %}
{% endbox %}
{% box 第四章的对话 %}
这里文本有一个特殊字符，而这个字符是一个贴图
Toby 的设计是，读取当前的打字机进度，到了就绘制这个贴图出来
中文译文里没有这种字符，所以就整个代码注释掉
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/44.png width:400px ratio:745/178 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/45.png obj_dw_church_intro_guei_Draw width:400px ratio:885/966 %}
{% endbox %}
{% box 第四章的&nbsp;TAKING&nbsp;TOO&nbsp;LONG %}
这里改动主要是 TAKING TOO LONG 文本的时长与顺序等
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/46.png obj_takingtoolong_Draw width:300px ratio:998/832 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/47.png obj_takingtoolong_Draw width:400px ratio:897/331 %}
{% endbox %}
{% box 第四章&nbsp;Mike&nbsp;战的麦克风选单 %}
这里会读取系统的麦克风名称
在 Patch 1.02，Toby 把这个选单的字体改为了默认直接用日文字体
毕竟日文字体含有的字符必然比英文字体多
所以我们也将这个代码回退到了旧版本，以使用中文字体显示
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p2/48.png width:400px ratio:1282/992 %}
{% endbox %}

# P3 打包器
我们不通过 UndertaleModTool 的图形化程序来打包
相反，我们使用 UndertaleModTool 的 DLL 动态链接库
透过 C# 代码来直接调用功能

这个打包器由 yig 制作，在此感谢他的付出
打包器的源码只有这四个 C# 文件
我们先从不那么重要的 `Loader` 与 `Program` 开始

## Loader & Program
`Program` 是程序入口
启动程序后，从输入的路径获取工作空间
然后用 Task 创建多线程，在线程里为每个章节运行 `Importer`
完成后再将构建好的文本复制到输出目录
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/1.png width:400px ratio:1567/738 %}
Loader 比较简单，只是提供了日志分级与加载 data 的衔接
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/2.png width:200px ratio:1430/1312 %}

## Importer & Exporter
接着便是两个主要的 `Importer` 与 `Exporter`

### Exporter 导出
#### 文本
`Exporter` 是我们一开始用于批量提取文本、资源与代码的
首先设置了正则表达式，用于从代码中寻找各个已有本地化的英文原文
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/3.png width:400px ratio:1606/601 %}
在 `ExportTexts` 方法导出文本，其调用了 `DecompileCodes` 方法，批量反编译所有代码
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/4.png width:400px ratio:985/471 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/5.png width:400px ratio:1257/532 %}
接着用正则匹配出文本，还有一些正则替换
包括 #& 都替换为真正的换行，去掉标点前的`^1`，去掉文本首尾的控制字符
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/6.png width:400px ratio:1533/351 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/7.png width:400px ratio:1278/297 %}
这些操作会方便我们的译者进行翻译，后续导入文本时再替换回去即可
这也就是为什么我们需要打字机 `\n` 换行

{% box 题外话 %}
这里去掉 `^1` 的设计是败笔
不是所有标点都有 `^1`，而且 `^` 功能只有打字机文本才有效，普通绘制会把这个绘制出来
所以当时导入游戏，自动补上 `^1`，然后，各种不使用打字机的地方出现了一大堆 `^1`
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/8.png width:400px ratio:1920/1080 %}
最后我们还是手动给所有不需要 `^1` 的文本前面打 `@` 字符来标记才解决了这个问题
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/9.png width:400px ratio:794/392 %}
{% endbox %}

#### 字体 & 代码 & 贴图
{% box ExportFont&nbsp;导出字体 %}
这个导出来主要是用于后续导入时与中文字体合并
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/10.png width:600px ratio:1533/370 %}
{% endbox %}
{% box ExportCodes&nbsp;导出代码 %}
导出所有代码，用于修改
当然这个用处不大，因为游戏更新，后续一般都是直接用 UTMT 改了复制出来
而不是全部导出来再挑出需要用的
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/11.png width:600px ratio:1532/240 %}
{% endbox %}
{% box ExportSprites&nbsp;导出贴图 %}
导出所有贴图
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/12.png width:600px ratio:1530/368 %}
{% endbox %}

### Importer 导入
最重要的 `Importer`，用于导入所有汉化修改后的文本资源代码
透过多线程运行，分别生成字体，处理文本，导入贴图与代码
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/13.png width:400px ratio:1920/1080 %}

#### ImportFont 字体
首先是 `ImportFont`，这会用到一个字库
这个字库是自动构建的，包含这些东西：
`code` 目录下的代码、`text_src` 下的中文译文、`global` 目录下的中文人名
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/14.png width:400px ratio:1920/1080 %}
然后将其以 UTF-16 编码输出到 `dump` 目录下的 `dict.txt` 中
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/15.png width:400px ratio:1790/936 %}
（别问我为什么是 UTF-16，老资历 bmfont 导致的）
接下来便会读取 bmfc 基础配置，并遍历字体对应的所有图片
然后按照这些图片的文件名，分割出`字符 ID`、`X 轴偏移`和 `X 轴 Advance`
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/16.png width:400px ratio:1392/419 %}
把这些参数追加到基础配置中，生成完整的配置
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/17.png width:400px ratio:1387/533 %}
接着便是使用 bmfont 生成字体大图与 `fnt`，完成后调用 `ImportFontData`
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/18.png width:400px ratio:1397/418 %}

从输出的 `fnt` 中读取字形参数，并设置到新建的字形资源中
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/19.png width:400px ratio:1391/633 %}
同时还有中英混排时出现字形偏移的修复，以及字形切尾的修复
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/20.png width:500px ratio:1389/421 %}
将这些修复应用到字形资源中
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/21.png width:600px ratio:1121/164 %}
完成后便创建一个新的纹理页，大小与字体纹理相同，将字体纹理放进纹理页中
然后再从 data 中找出同名字体，设置其的参数与对应的纹理
以及把前面创建的字形资源赋值给这个字体，将其覆盖
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/22.png width:300px ratio:1415/751 %}

#### ImportTexts 文本
接着是 `ImportTexts`
按照 `text_src` 下的 `raw.json` 遍历，一个个匹配
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/23.png width:300px ratio:1391/705 %}
再透过 `ReplaceItem` 方法来替换人名译文
人名译文替换时各种命名介面是例外，需要防止替换到命名彩蛋等
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/24.png width:300px ratio:1235/632 %}
人名译名的替换用到了 `Aho-Corasick` 算法，透过一个现有的 `AhoCorasickDoubleArrayTrie` 类
具体算法就不单独讲了，各位可以上网自行了解
只需要知道这是一种在同时查找大量字符串时非常快的字符串搜索算法就行
最后还需要交给 `RestoreItem` 方法来恢复导出去掉的特殊字符，然后写入到 json 中
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/25.png width:300px ratio:1379/1122 %}

#### ImportCodes 代码
`ImportCodes`，导入代码
这个比较简单，直接使用 `CodeImportGroup` 类即可
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/26.png width:400px ratio:1387/635 %}
不过这里要注意的是贴图导入时由于人名翻译切换的存在，必然会有新贴图
而不导入贴图前，代码无法匹配到这些贴图的 ID
所以代码的导入必须在贴图之后
这里是直接在代码添加到 `ImportGroup` 之后加了一个等待
贴图导入 Task 完成后再执行导入操作

#### ImportSprites 贴图
等待的贴图导入也就是 `ImportSprites` 方法
首先会遍历 `atlas` 目录下的 cfg 配置
并将其对应的纹理导入进来
新建一个纹理页，将纹理设置到纹理页中
接着解析对应的 cfg 配置，从 data 中获取对应的贴图
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/27.png width:400px ratio:1385/887 %}
会透过 `GetSprite` 方法获取贴图
如果匹配不到贴图那就尝试匹配去掉 `_zhname` 的原贴图
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/28.png width:400px ratio:1133/363 %}
若匹配到原贴图后新建一个贴图，并照抄原有贴图的参数，将其添加到 data 后再返回
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/29.png width:400px ratio:963/1025 %}
拿到贴图之后设置其的纹理并调整偏移量
如果纹理大小不一致，则会重新计算原点与边界的值
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/30.png width:400px ratio:1435/1489 %}

#### 导入结果
全部完成导入后就把 data 输出到 result 目录，这样就完成了一次构建
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p3/31.png width:600px ratio:1299/327 %}

# P4 补丁包 & 安装器
## 补丁包
补丁包本质上是一个 7-Zip 压缩包
以 `patch_语言_操作系统(_demo)_版本号` 为文件名
其中语言有 `chs` 简中 `cht` 繁中，不过繁中补丁直到现在都没出来，所以暂时是没有
操作系统包含 `windows` `macos` 与 `linux`
实际使用中 `windows` 与 `linux` 使用同样的文件，所以会写成 `windowslinux` 的形式

压缩包中包含所有需要覆盖到游戏目录的文件，例如语言 json 与第三章的视频
除此之外便是每个 data 的 xdelta，全部放置在压缩包的根目录
`main.xdelta` 是用于章节选择的，`chapterX.xdelta` 便是各个章节的
安装器运行时会先把压缩包内容覆盖到游戏目录上，再用 xdelta3 将 xdelta 文件一个个安装

## 安装器
这个安装器的主要部分由我制作，感谢并非组员的 WhatDamon 为这个安装器完善了 macOS 相关的兼容
同时也要感谢其它为该项目提交贡献的 Contributor 们，在此感谢各位的付出
<a href="https://github.com/gm3dr/DeltaruneChinesePatcher/graphs/contributors?all=1">
  <img src="https://contrib.rocks/image?repo=gm3dr/DeltaruneChinesePatcher" />
</a>
我们在[前文](#为什么不直接配布-data)说过，为了避免给每个章节的 data 一个个打补丁的麻烦，我们制作了这个安装器
这个安装器是使用 Godot Engine 制作的，使用 .NET C# 代码
至于为什么 GameMaker 的游戏要用 Godot 来做补丁安装器
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/1.jpg 因为我会（x width:200px ratio:360/346 %}

从 Godot 官网下载最新 .NET 版本的编辑器
使用和下载补丁源码一样的方法下载安装器源码
打开项目，进入里面唯一一个场景 `Main.tscn`，根节点绑定了唯一一个脚本 `Main.cs`
当初因为功能少所以就想着一个场景足矣，结果功能越加越多，导致这个场景乱七八糟的
场景的主题透过 `Assets` 目录下的 `Theme.tres` 设置，包括按钮与输入框的样式等

### 场景
先从场景基本结构开始
节点树的从下到上也是覆盖顺序的从后到前
`Background`，这里是背景图，动画播放与半透明黑色滤镜
这里与游戏内读档介面不同，图像上方并没有喷泉的特殊效果
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/2.png width:600px ratio:1081/424 %}

接着便是主介面，包含中间的文字与按钮
`CenterContainer` 的功能是将节点居中
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/3.png width:600px ratio:1920/1080 %}

`BottomContainer` 是类型为 `HBoxContainer` 的容器，用于把节点横向排列
用于左下角的安装器版本号与右下角的安装器更新按钮
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/4.png width:600px ratio:797/169 %}
这里的 `Left` 同理，是类型为 `VBoxContainer` 的容器，用于把节点纵向排列
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/5.png width:600px ratio:755/161 %}

然后便是单独的：
左上角资讯按钮，右上角语言选项，用于 macOS 的窗口标题栏
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/6.png width:600px ratio:1658/175 %}
还有一个弹出选取文件用的 `FileDialog`
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/7.png width:400px ratio:1084/580 %}

再往下就是各种窗口了，Godot 原生支援多窗口，只需要在项目设置关闭 `嵌入式子窗口` 即可
同时为了保证鼠标悬浮提示正常显示透明背景，还需要开启 `像素级透明`
窗口按照顺序分别是：
`日志显示`，`讯息弹窗`，`1225 彩蛋的讯息弹窗`，`补丁已安装时的弹窗`
`在安装器目录下读取到 README 时的显示`，`教程视频链接`，最后是`高级选项`
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/8.png width:600px ratio:1920/1080 %}

### 代码
首先，为了方便调用场景内的节点，同时避免大量 `GetNode` 带来的性能损耗
提前为所有用到的节点设置了变量存储，同时全部 `Export`
在编辑器把节点引用保存到场景中
这样这些节点的引用就会在场景加载时提前加载好
{% swiper %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/9.png 变量定义 width:100px ratio:604/1542 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/10.png 编辑器 width:100px ratio:313/1208 %}
{% endswiper %}

安装器初始化时会检查是否是用于旧设备的 Outdated 版本，如果是则弹出提示
这是通过检查引擎版本是否小于等于 4.4 来实现的
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/11.png  ratio:1405/39 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/12.png  ratio:1376/282 %}
从 Godot 4.5 开始不再支援 [Win10 以下操作系统](https://github.com/godotengine/godot/pull/106959)与[不支援 SSE 4.2 的 CPU](https://github.com/godotengine/godot/pull/108561)
所以我们从 v2.4.1 开始另外提供了基于 Godot 4.4 的 Outdated 版本

之后就会寻找补丁包，也就是遍历安装器目录下所有文件
检查以 `patch_` 开头的文件，对其的文件名基于下划线分割，读取版本号
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/13.png width:400px ratio:1231/509 %}
然后还有检查是否有 `readme` 文件，有就透过窗口显示出来
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/14.png width:400px ratio:1185/400 %}

接着便是更新 Contributor 列表，检查补丁与安装器更新
{% swiper %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/15.png 更新&nbsp;Contributor&nbsp;列表 ratio:1411/442 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/16.png 检查补丁更新 ratio:1868/489 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/17.png 检查安装器更新 ratio:1593/484 %}
{% endswiper %}
这些操作都是使用 `HttpClient`，透过 Github API 完成的
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/18.png  ratio:1666/71 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/19.png  ratio:532/43 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/20.png  ratio:1057/51 %}
补丁与安装器的更新讯息，包括版本号与链接，都会各自存储到 `patchreleases` 与 `patcherreleases` 中
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/21.png  ratio:1066/135 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/22.png  ratio:1124/121 %}

如果之前没有填写过路径，那么就会透过 `FindGamePath` 方法尝试寻找游戏路径
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/23.png  ratio:914/308 %}
其会先检查这些默认路径
```
Windows: C:/Program Files (x86)/Steam/steamapps/common/DELTARUNE*(demo)*
macOS: ~/Library/Application Support/Steam/steamapps/common/DELTARUNE*(demo)*
Linux: ~/.local/share/Steam/steamapps/common/DELTARUNE*(demo)*
```
如果搜不到，且是 Windows，那么就会读取注册表
从 `HKEY_CURRENT_USER\Software\Valve\Steam` 底下的 `SteamPath` 拿到 Steam 安装目录
接着从 Steam 目录下 `steamapps` 拿到 `libraryfolders.vdf`，使用 Gameloop.Vdf 来解析
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/25.png width:600px ratio:1521/181 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/24.png width:600px ratio:1984/861 %}
{% note 不是只有&nbsp;Windows&nbsp;会读&nbsp;libraryfolders.vdf 是只有 Windows 会透过注册表获取 Steam 安装目录<br>其它系统仍会使用默认目录来获取 `libraryfolders.vdf` %}
这个文件中记载了 Steam 安装了的所有游戏目录
从中寻找 DELTARUNE 本体或 Demo 版的安装路径
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/26.png width:400px ratio:1522/574 %}

#### 安装逻辑
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/27.png width:400px ratio:1496/860 %}
点击安装按钮，会先使用 `PathTrim` 方法来进行路径处理
包括处理正反斜杠，替换类 Unix 的 `~` 家目录，补全 macOS 包内路径
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/28.png width:400px ratio:1403/641 %}
接着便会检查是否曾经安装过汉化，这是透过检查是否存在 `backup` 目录来实现的
同时还会从 `backup` 目录下的 `version` 文件里获取补丁版本

如果没有问题那么就透过 `Patch` 方法进行安装补丁
首先是安装器内嵌的 7z 与 xdelta3 的检查
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/29.png width:400px ratio:1404/1167 %}
譬如检查当前系统中是否有能直接用的，以及 sha256 散列值检查
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/30.png  ratio:1337/223 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/31.png width:400px ratio:1926/619 %}
要用到 sha256 散列值检查是因为之前遇到过有人电脑中了病毒
然后解压安装器时自动感染了，然后顺带还损坏了程序

检查完成后便开始安装，使用 `Process` 类新建进程运行 7z
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/32.png 7z.exe&nbsp;<补丁路径>&nbsp;-o&nbsp;<安装器目录下&nbsp;ExtractTemp>&nbsp;-aoa&nbsp;-y width:400px ratio:1249/467 %}
用 7z 解压后调用 `MoveAfterExtracted` 方法将文件全部移动到 DELTARUNE 目录下
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/33.png  ratio:667/53 %}
移动前会检查是否已经存在文件，如果存在那么就把原有的文件移动到 `backup` 目录下，再移动
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/34.png width:400px ratio:1339/548 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/35.png width:400px ratio:1760/438 %}
然后再备份 data，创建多进程为每个 data 运行 xdelta3
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/36.png width:400px ratio:1277/618 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/37.png width:400px ratio:1869/682 %}
直到所有 data 完成后才会触发结束
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/38.png  ratio:1038/203 %}
若 30 秒后还没完成就杀死进程并触发 `TAKING TOO LONG` 错误
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/39.png width:400px ratio:1184/437 %}

安装结束进入 `Ending` 方法，清理所有 xdelta 文件
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/40.png width:400px ratio:855/337 %}
计算安装耗时，根据日志内容触发对应的错误到 `PatchResultHandler` 方法
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/41.png width:400px ratio:1877/749 %}
保存 log，若失败则透过 `RestoreData` 方法撤销更改
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/42.png 保存&nbsp;log width:400px ratio:1184/680 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/43.png PatchResultHandler width:400px ratio:1313/529 %}
最后显示结果弹窗，安装流程结束

卸载补丁本质上就是将 `backup` 目录里的文件覆盖到游戏文件中
按下按钮后便会检查备份存在，然后触发 `RestoreData` 方法，遍历 `backup` 目录还原
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/44.png width:400px ratio:1095/439 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/45.png width:400px ratio:757/421 %}

#### 补丁更新逻辑
点击更新补丁时，会先删除现有的补丁
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/46.png width:600px ratio:1201/334 %}
然后从 `patchreleases` 的 `assets` 获取 `name`，检查是否 demo 与操作系统
接着获取它的 `browser_download_url`
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/47.png width:600px ratio:1373/357 %}
使用 `HttpClient`，按照 4096 字节分块异步下载
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/48.png width:600px ratio:1403/1274 %}
下载中的文件会在文件名前面加上 `_downloadingtemp_`
这是为了防止在某些异常情况下没下完的文件被安装器识别为正常补丁
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/49.png  ratio:730/38 %}
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/50.png  ratio:1072/114 %}
完成后等待五秒重载场景
{% image https://cdn.jsdmirror.com/gh/SheepYhangCN/sheepyhangcn.github.io/source/assets/images/drzh-tech/p4/51.png  ratio:649/91 %}