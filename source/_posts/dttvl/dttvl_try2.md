---
title: DELTATRAVELER 汉化初尝试²
date: 2025-04-06 03:14:02
cover: /resources/images/dttvl_try2/options.png
tags: 
 - 汉化
 - DELTARUNE
 - Unity
 - DELTATRAVELER
---

经过了熬夜一个晚上的研究
算是把工作流弄好了

## 文本
用 UABEA Dump 出 MonoBehaviour 的 json
然后用[脚本](https://github.com/UTCLC/DTTVL-Scripts/blob/main/UABEAJsonTextCollect.py)来从 json 中提取所有的```m_Text``` ```lines``` ```phrases```
生成的 json 上传到 Weblate 作为英文即可
这些文本修改了基本不会出现影响游戏的情况
也就不用再加筛选算法了

用 ```ildasm.exe``` 打开 DLL 并 Dump 出 IL
然后用[脚本](https://github.com/UTCLC/DTTVL-Scripts/blob/main/ILStringsExtract.py)来从 IL 中提取所有的字符串
脚本会自动按照算法筛选分类出五类文本
```
asterisk.json：含有「* 」的，基本可以确定是正常对话文本，可以无脑翻译
slash_underline.json：含有斜杠或下划线的，基本可以确定不需要翻译
space.json：含有空格的，基本都是菜单选项或者对话的后半段，可以看着来翻译
upper.json：含有大写的
others.json：剩下的
```
把这些 json 分别上传到 Weblate 作为英文即可

### 更新文本
从 Weblate 上爬取文本
然后同理，用前面的两个脚本把文本分别封回 MonoBehaviour 和 IL 里
再用 ```ilasm.exe``` 和 UABEA 把它们封回对应的资源与 DLL 后就可以启动了

## 字体
和 UTMT 类似，需要有一个从现成的 Unity 游戏用 UABEA 拔出来的字体
所以被迫装一个 Unity
UABEA 只能导出二进制 dat 资源
用 AssetStudioMod 可以导出 ttf/otf 字体
导出来的字体文件然后再用 FontCreator 进行一个中英合并
然后导入 Unity 就行

Gamejolt 上的最新版 v3.0.10 用的是 2018.3.14f1
就用这个版本开了个[项目](https://github.com/UTCLC/DTTVL-FontsUnityProj)导入了字体
后来经过提醒发现有测试版 v3.1.0p4
这个版本用的是 Unity 6
只能说还好我手上的是国际版的 Unity Hub 不然真没辙
不过目前测试用 2018.3.14f1 生成的字体还能用
已经把项目升级到了 Unity 6
下次就直接用了

### 字体尺寸问题
Deltatraverler 是按照 Determination 字体的尺寸加载的
如果使用 DTM+其它中文字体 的融合字体方案就会导致中文字体尺寸不合而变形
![options](./resources/images/dttvl_try2/options.png)

之前参与过 UTY 项目的晓晓也帮忙尝试了重新编排一次英文使得中英尺寸相同的方案
然后结果是
不仅中文没好 英文也一起变形了
![new_font](./resources/images/dttvl_try2/new_font.png)
(注意那个百分号)

所以，嗯，目前这个问题还没解决
只能先用着前面那个英文正常的了

## 分辨异常
如果是游戏打不开或者闪退或者卡死或者弹出 Unity 的 CrashHandler
那么就是资源导炸了 需要回档重新导
如果是进入 Deltatraveler 自己的 ExceptionHandler
那么就是 DLL 出问题了 很大概率是某个资源名被翻译了

## v3.1.0p4 也干了
更新到 v3.1.0p4 之后一个莫名其妙的 Bug
如果按窗口的 X 关闭游戏而不是长按 Esc
会导致卡一个 Deltatraverler 的进程在后台
然后就无法再打开了
要用任务管理器手动杀掉
所以可能发布的时候得在游戏目录里加一个 bat 来一键杀除