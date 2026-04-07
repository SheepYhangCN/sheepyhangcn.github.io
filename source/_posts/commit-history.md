---
title: 新增了修订历史
date: 2026-04-08 05:59:55
updated: 2026-04-08 05:59:55
categories: [博客更新]
repo: SheepYhangCN/sheepyhangcn.github.io
tags: 
 - Blog
---

在 Github Copilot 的帮助下给博客文章新增了修订历史功能
翻到最下面就能看到了
技术上来说是通过 `git log` 命令获取记录

也就 花了大概六个小时不到 嗯
被 Copilot 蠢笑了只能说 还贼慢

灵感来源是[这篇文章](https://2x.nz/posts/posts-diff/)
不过技术上应该是除了都用到 git 以外毫无关联了
我这里用的方法是直接在构建的时候就生成每个文章的 JSON 并放到构建结果中
然后进行解析来生成修订历史
所以就不需要用到难以除错的 Actions 了

以及，因为涉及到比较底层的修改，所以把 Stellar 换成了 submodule，方便修改
顺便也把非文章的某些页面去除了建立时间，只保留最后更新时间

下一步是把文章硬编码的最后更新时间去掉
改成直接从修订历史拿
但是写这个 post 的时候我这里已经是凌晨六点了
还不睡觉会死的哥 撤了 起床再说