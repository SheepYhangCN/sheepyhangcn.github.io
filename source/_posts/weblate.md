---
title: Undertale Yellow 汉化技术问题记录【3】
date: 2023-12-25 19:06:24
cover: /resources/images/weblate/account_paused.png
tags: 
 - Undertale
 - Undertale Yellow
 - 汉化
 - Crowdin
 - Weblate
 - GameMaker
---

2023年12月22日 我的Crowdin账户因为超过限额而被暂停
![account_paused](./resources/images/weblate/account_paused.png)
![project_paused](./resources/images/weblate/project_paused.png)
Crowdin规定免费账户最多可以托管60000个单词
而Undertale: Yellow光星号开头的句子就包含超过70000个单词
所以，嗯，再次搬迁

## Weblate架设
我本想用阿里云自己架设一个Weblate
等我刚装好宝塔面板的时候
天机已经把他手上本来有的Weblate移植过来用了
所以 本来这里能写不少内容的
乐
也不麻烦其实 Weblate文档写的很齐全
用宝塔面板安装Docker 走Docker的流程
或者直接装Ubuntu 走Ubuntu的流程也可以

## 移植strings
首先是 Weblate只支持分段格式
![weblate_format](./resources/images/weblate/weblate_format.png)
这很好解决 导出时手动给每个换行后面多加个换行
（直接Ctrl+F把换行替换为换行换行）
导入就手动修改一个脚本
把我之前写的ImportAllStringsAsterisk改一改即可
虽然实际方法非常弱智

<details>
<summary>WeblateImportAllStringsAsterisk.csx</summary>
```
using System.Text;
using System;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using UndertaleModLib.Util;

EnsureDataLoaded();

if (Data.ToolInfo.ProfileMode)
{
    ScriptMessage("This script will not modify your existing edited GML code registered in your profile. Please use GML editing for text editing, or a script like FindAndReplace, for editing strings within these code entries.");
}
else
{
    if (!(ScriptQuestion("This script will recompile all code entries in your profile (if they exist) to the default decompiled output. Continue?")))
        return;
    foreach (UndertaleCode c in Data.Code)
        NukeProfileGML(c.Name.Content);
}

string importFolder = PromptChooseDirectory();
if (importFolder is null)
    throw new ScriptException("The import folder was not set.");

// Overwrite Check One
string stringsPath = Path.Combine(importFolder, "strings.txt");
if (!File.Exists(stringsPath))
{
    ScriptError("No 'strings.txt' file exists!", "Error");
    return;
}

var file = File.ReadAllText(stringsPath);
file=file.Replace("\r\n\r\n", "\r\n");
file=file.Replace("\r\r", "\r");
file=file.Replace("\n\n", "\n");
var file_edited=File.CreateText(stringsPath.Replace("strings.txt", "strings_edited.txt"));
file_edited.Write(file);
file_edited.Close();

using (StreamReader reader = new StreamReader(stringsPath.Replace("strings.txt", "strings_edited.txt")))
{
    foreach (var str in Data.Strings)
    {
        if (str.Content.Contains("\n") || str.Content.Contains("\r"))
            continue;
        if (str.Content.StartsWith("*  "))
            str.Content=reader.ReadLine();
    }
}

```
</details>

本来天机写的是在```str.Content=reader.ReadLine()```之后再单独执行一次```reader.ReadLine()```
but don't know why, it just won't work, and it will break ```data.win```
所以我就用了一个非常简单粗暴且弱智的方法
直接修改原来的txt保存为另一个txt 然后读取保存出来的txt
不管它有多弱智，it just works
能跑就行 嗯

## 导入原文与译文
接着就是 我从Crowdin下载译文与原文下来
然后手动加上分段换行 再导入Weblate
英文导入没问题
但是Weblate不支持txt对照 译文导入没效果
但是这里又是天机处理的 我把分好段对应好的原文译文txt给了天机
天机再把译文进行处理再上传Weblate
所以这里又没东西写了（

## 没了
对 Weblate问题真的不算多
不像SbCrowdin 导入的时候问题一堆 还卡 还贵
应该是不会再出问题了（

## 多灾多难の汉化项目
2023/12/10 立项目
2023/12/13 金山文档转Crowdin
2023/12/24 Crowdin转Weblate
不能说是多灾多难，只能说是命途多舛
希望是别再给我整这么多事（

## 圣诞快乐
哦对了，祝各位圣诞快乐
Merry Christmas.