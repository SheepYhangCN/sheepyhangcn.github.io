---
title: DELTATRAVELER 汉化初尝试
date: 2025-04-05 00:07:45
cover: /resources/images/dttvl_try/UABEA.png
tags: 
 - 汉化
 - Undertale
 - Unity
 - DELTATRAVELER
---

Undertale Yellow 的项目也算是结束了
后续只要别再出什么 Bug 应该也就不用再维护了
现在总是只想做大项目
嗯 然后就想把先尝试把 Deltatraveler 弄一下
这也是第一次研究 Unity 游戏逆向
之前最多也就是用 AssetStudioMod 拆些素材之类的

## 分散的文本
首先是 得益于 Unity 这坨系统
文本散落在的不同的地方
需要通过代码操作的文本在编译在了```DELTATRAVELER_Data\Managed\Assembly-CSharp.dll```里
静态的文本和对话散落在资源文件里面
Unity 的 level 就有点像 GameMaker 的 room
但是 level 是直接散落在游戏目录里的

## Assembly-CSharp.dll
这个 DLL 也是游戏的代码所在地
比起反编译为 C# 这种容易炸的方法
更合适的办法其实是解成IL再用工具封回去
.NET SDK中本就有这个工具
dll → IL ```"C:\Program Files (x86)\Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.8 Tools\ildasm.exe"```
打开 ildasm.exe 后选择 File→Open
打开 DLL 文件 在选择 File→Dump
就可以得到IL文件了
然后用[脚本](https://github.com/UTCLC/StringsExtract/blob/main/StringsExtract.py)爬取所有的字符串出来即可
后续再加一下和uty一样的条件限制 比如带空格之类的就可以了
汉化好后用前面的脚本把字符串塞回IL里
IL → dll ```C:\Windows\Microsoft.NET\Framework64\v4.0.30319\ilasm.exe```
用命令行把IL和res用参数赋给 ilasm.exe 就可以重新生成一个dll了
```"C:\Windows\Microsoft.NET\Framework64\v4.0.30319\ilasm.exe" /dll Assembly-CSharp.il Assembly-CSharp.res```

### 秘之 inf
解出来的IL里出现了几个 
```ldc.r4     inf```
会引起 ilasm 报错
只需要把这里的 inf 换成 0x7F800000 即可
也不知道为啥能解出来个 inf

## 资源文件
可以使用 UABEA 来打开所有的资源文件
```
当然如果你只是想改某一个地方的文本只要你能找到那是哪个文件也行
可以用 AssetStudioMod 打开整个目录后右键 Show original file
UABEA 多文件加载速度是真的慢
```
然后筛选 MonoBehaviour 类型
用 UABEA 全选并全部 Dump 为 UABEA Json 文件即可
![UABEA](./resources/images/dttlr_try/UABEA.png)
目前发现了会出现的三种类型的文本
```m_Text``` 是单个文本
```lines```与```phrases``` 是对话组
用[脚本](https://github.com/UTCLC/DTTLR-UABEAJsonTextCollect/blob/main/UABEAJsonTextCollect.py)把这些key对应的文本从json里提取出来即可
和前面的 DLL 同理，后续加一下条件限制就可以了
汉化好后用前面的脚本重新生成这些json
然后用 UABEA 导入 Dump
保存就可以了

## 字体
还没整出来
等下次吧