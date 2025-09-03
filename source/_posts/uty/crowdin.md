---
title: Undertale Yellow 汉化技术问题记录【2】
date: 2023-12-13 19:00:54
cover: /resources/images/crowdin/candy_spare.png
tags: 
 - UNDERTALE
 - Undertale Yellow
 - 汉化
 - Crowdin
 - GameMaker
---

因为金山文档问题太多 而且太卡了
所以决定把翻译平台搬到Crowdin上
顺便打算先把带星号的句子提出来翻译

## 仅处理星号开头的文本
修改Undertale Mod Tool的脚本
使它只导出开头为```* ```的字符串
不难做，加上if即可

<details>
<summary>ExportAllStringsAsterisk.csx</summary>
```
using System.Text;
using System;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using UndertaleModLib.Util;

EnsureDataLoaded();

string exportFolder = PromptChooseDirectory();
if (exportFolder is null)
    throw new ScriptException("The export folder was not set.");

string stringsPath = Path.Combine(exportFolder, "strings.txt");
//Overwrite Check One
if (File.Exists(stringsPath))
{
    bool overwriteCheckOne = ScriptQuestion(@"A 'strings.txt' file already exists.
Would you like to overwrite it?");
    if (!overwriteCheckOne)
    {
        ScriptError("A 'strings.txt' file already exists. Please remove it and try again.", "Error: Export already exists.");
        return;
    }
    File.Delete(stringsPath);
}

using (StreamWriter writer = new StreamWriter(stringsPath))
{
    foreach (var str in Data.Strings)
    {
        if (str.Content.Contains("\n") || str.Content.Contains("\r"))
            continue;
        if (str.Content.StartsWith("* "))
            writer.WriteLine(str.Content);
    }
}
```
</details>

不过 导入就有点麻烦了
utmt自带的脚本我属实看不明白
所以我的选择是，用基于Export改一个Import

<details>
<summary>ImportAllStringsAsterisk.csx</summary>
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

using (StreamReader reader = new StreamReader(stringsPath))
{
    var a=0;
    foreach (var str in Data.Strings)
    {
        if (str.Content.Contains("\n") || str.Content.Contains("\r"))
            continue;
        if (str.Content.StartsWith("* "))
            str.Content=reader.ReadLine();
        a+=1;
    }
}
```

</details>

这样就可以导出导入所有星号开头的字符串
可以开始上传Crowdin了

## Crowdin？Crowd1ck！
创建一个私人项目 然后我就发现一个很难绷的东西
免费账户可以创建一个私人项目
但是免费账户不能拉人
就是说免费账户开私人项目只能自娱自乐
你这给了和不给有啥区别
而且最便宜的Pro等级还非球贵 50美元/月

因为没钱 所以开了个public项目
上传英文data导出的strings
再给简体中文上传汉化过的data导出的strings

然后我才发现 它居然自动给我分句了
关键是 它只分了英文 没分中文
然后就对不上位置了
然后还不止分句问题
它还吞了不少翻译
![imported](./resources/images/crowdin/imported.png)
（为了避免剧透 所以调成糊的了 红色是未翻译 蓝色是已翻译 很明显参差不齐+有些句子分开了前后）

然后 我就删了文件重新导入
修改了纯文本的分句方式再导入
这次终于正常了
虽然还是吞了不少翻译

然后就在我们开始从文档那边搬译文过来补的时候
似乎发现了什么弱智情况
![小 石 蛙](./resources/images/crowdin/frog_pebble.png)
对 译文乱了
![小 石 弹](./resources/images/crowdin/pebble_flint.png)
《小 石 弹》
![饶 恕 糖](./resources/images/crowdin/candy_spare.png)
《饶 恕 糖》
![柴 片](./resources/images/crowdin/chisp_matches.png)
《柴 片》

嗯 然后就是花了点时间找到所有乱了的译文 一个个改回去
然后顺便从文档那边把译文补过来 然后再顺便问候问候Crowdin的木琴
好在导出txt和导入data没什么问题出现

## 没了
嗯 就这样平稳地过渡到了Crowdin
without any trouble, of course.

目前光是星号开头的对话文本就有将近一万行
虽然里面包含了很多废弃的修改记录
但是量也很大了
更何况之后还有战斗敌人对话的文本 还没数过有多少
到时候再说吧