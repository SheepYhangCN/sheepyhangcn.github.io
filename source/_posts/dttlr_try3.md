---
title: DELTATRAVELER 汉化初尝试³
date: 2025-04-18 21:02:06
tags: 
 - 汉化
 - Undertale
 - Unity
 - DELTATRAVELER
---

byd这还能有3的

## bytearray
首先是解出来的 IL 里缺失了一些文本
搜了一下发现被弄成 bytearray 了
![before](./resources/images/dttlr_try3/before.png)
这些文本的共同点就是都含有 ```\b```
咱也不知道为啥要有这东西 意义不明
但是有了这玩意之后 ildasm 就会解成 bytearray
但是反过来 把解出来的 bytearray 改写成字符串是可以正常重新封成 DLL 的
就 很抽象

## DeepSeek 助我
这种带换行带缩进带注释插中间的玩意
不是不能写 但是太麻烦了
所以我决定进行一个科技改变生活
让 DeepSeek 给我写一个
嗯，然后就这样，[脚本](https://github.com/UTCLC/ILBytearraysConvert/blob/master/ILBytearraysConvert.py)
![after](./resources/images/dttlr_try3/after.png)

## 更新翻译文件
之前说过，翻译文件的 json 是使用行数作为 key 的
然后这样一改 就飞了
虽然目前还没有翻译
但是为了避免以后需要修改 dll
还是得想一个解决方法

然后，嗯，又是 ds，[脚本](https://github.com/UTCLC/DTTLR-ILStringExtract/blob/main/UpdateLineAfterUpdated.py)
我太不想进步了（x

不过要更新还是很麻烦
因为 weblate 的机制
不能删除原本的原文
就只能删掉部件重新添加
然后再把生成的译文重新塞回去
然后 contributors 就没了
毕竟是跟 key 的
所以还是希望用不上

## 繁体
之前 uty 就想做繁体的
但是因为 gm 那个字体系统加字库太恶心了
所以就没做

这次是 ttf 字体
所以大概率是会做的
👍