---
title: 无人深空中字英配 Mod 制作
date: 2026-04-16 03:56:43
---

作为一个坚持原版配音的玩家/观众，我非常讨厌配音绑定文字语言的设定
Unfortunately，一款我非常喜欢的游戏，无人深空就是这样的
So, Let's just fix it by ourselves.

## 免责声明
本文不可避免地涉及到通过使用第三方工具来对 *无人深空 / No Man's Sky* 的游戏档案进行资源解包，以及通过 Mod 方式对其游戏档案进行修改
本文中所给出的操作不涉及修改游戏核心代码，不改变游戏机制与数值，不涉及任何作弊、外挂或多人联机干扰功能，理论上不会破坏游戏运行或损坏存档
本文仅供个人学习与技术交流。跟随本文操作而造成的游戏运行异常、存档损坏，概不负责，操作前请自行提前备份重要的存档资料

## 准备工作
无人深空 No Man's Sky 的 PC 端游戏本体 *废话*
下载工具 [HGPAK Tool](https://github.com/monkeyman192/HGPAKtool)

## 解包
打开命令行/终端，输入
```
hgpaktool.exe -U "<NMS 目录>/GAMEDATA/PCBANKS/NMSARC.audio.pak" "<NMS 目录>/GAMEDATA/PCBANKS/NMSARC.audioBNK.pak" -O "<输出目录>"
```
等待终端出现 `Unpacked <数量> files from 2 .pak's in <耗时>s` 提示即为完成
*不要急，花个几分钟是正常的*

## 剔除不需要的档案
打开输出目录，删除 `music` 资料夹
打开 `audio/<操作系统>` 资料夹，删除除了 `<你使用的文字语言>` `<你想使用的配音语言>` `media`以外的所有资料夹
我想替换简繁中为英配，所以保留了 `chinese(prc)` `chinese(taiwan)` `english(us)`，分别对应
打开 `media` 资料夹，删除 `<你使用的文字语言>` `<你想使用的配音语言>` 以外的所有资料夹，同上

## 替换资源
打开 `audio/<操作系统>` 资料夹
将 `<你想使用的配音语言>` 里的所有档案复制到 `<你使用的文字语言>` 资料夹中，覆写原有档案
然后删除 `<你想使用的配音语言>` 资料夹

打开 `media` 资料夹
清空 `<你使用的文字语言>` 资料夹，将 `<你想使用的配音语言>` 里的所有档案复制到 `<你使用的文字语言>` 资料夹中
然后删除 `<你想使用的配音语言>` 资料夹

## 使用 Mod
将前面操作完的资料夹移动到 `<NMS 目录>/GAMEDATA/MODS` 下
注意此时的文件目录应为这样的结构：`<NMS 目录>/GAMEDATA/MODS/<输出目录的资料夹名>/audio/<操作系统>`

然后用任意文本编辑器打开 `<NMS 目录>/Binaries/SETTINGS/GCMODSETTINGS.MXML`
将其中 `<Property name="DisableAllMods" value="true" />` 的 `true` 改为 `false` 即可

## 结束
好了之后进入游戏应该会显示一个启用 Mod 的警告，按下任意键即可
未涉及语音更新的版本通常不会使 Mod 失效，失效时只需根据本文内容再次操作即可