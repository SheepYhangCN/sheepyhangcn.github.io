---
title: DELTATRAVELER 汉化技术问题记录【1】
date: 2025-05-18 00:43:06
cover: /resources/images/dttvl_enemyname/loadpath.png
tags:
 - 汉化
 - Undertale
 - Unity
 - DELTATRAVELER
---

终于是立项了
因为前期准备工作做的充足
所以立项后就没什么好写的了

## 首个问题
首先是敌人名字无法翻译
翻译后会在进入战斗时报错
检查代码后发现 敌人初始化中有两个变量
enemyName与fileName
![awake](./resources/images/dttvl_enemyname/awake.png)
游戏中显示的敌人名字为enemyName

进一步检查C#代码后 可以看到这两个变量也分别用于拼接敌人资源路径的目录与文件名
![loadpath](./resources/images/dttvl_enemyname/loadpath.png)
这种行为太唐了 拿敌人名当目录名
有空格有点号的 是真不怕出问题啊
而且明明可以同用一个fileName 却要分开用两个

## 解决
解决方法非常抽象
选择的方案是 把这些用到enemyName的地方 都改为fileName
然后enemyName照旧翻译

不过有一些enemyName与fileName不一致的敌人
所以需要先用[脚本](https://github.com/UTCLC/DTTVL-Scripts/blob/main/EnemyNameFileNameDiffCheck.py)确认这些敌人的enemyName与fileName
这些需要单独处理

然后就可以用[脚本](https://github.com/UTCLC/DTTVL-Scripts/blob/main/ReplaceEnemyName2FileName.py)
批量把IL中的`battle/enemies/敌人名字`拼接 从enemyName改为fileName

以及需要处理前面那些不相同的敌人
用 UABEA 提取出资源路径
只需要提取出唯一一个ResourceManager资源即可
里面包含所有资源的路径
分别搜索所有`battle/enemies/敌人名字` 把敌人名字替换为和fileName一样即可

除了有个例外
遗迹初见的Flowey的enemyName是FloweyCutscene
而fileName和遗迹结尾的Flowey战斗同为flowey
决定是把这个敌人的fileName修改为floweycutscene
所以这个需要把文件名的flowey也改为floweycutscene

ResourceManager资源改好导入回去之后
还有DLL中也会直接加载一些资源
也要手动把这些资源的路径进行修改
完成后就可以了