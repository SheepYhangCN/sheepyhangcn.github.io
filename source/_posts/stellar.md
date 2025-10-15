---
title: 本站更换为 Stellar 主题
date: 2025-10-15 13:10:05
categories: [博客更新]
repo: SheepYhangCN/sheepyhangcn.github.io
tags: 
 - Blog
---

Nexmoe 看腻了
所以又花了两天换成了 [Stellar](https://xaoxuu.com/wiki/stellar/)
主要是这个笔记系统确实好用
然后去掉了之前为了 Nexmoe 而设置的所有文章的头图

然后 Stellar 不支援多个 rss
所以只保留了 Atom，而且把链接改为了```https://sheepyuhang.top/rss.xml```

理论上来说 Stellar 是支援多语言的
但是我实在没时间维护 + Stellar 似乎没有自带切换开关
所以还是算了 之前也没有做的说

然后因为弄主题 所以所有文章的最后编辑时间都变了
事已至此那就不管了

烦人的是 换成 Stellar 之后默认链接就自带斜杠
然后这样子本地运行没啥问题 会自动去掉斜杠
但是上传到 Github 之后带斜杠就会直接 404
现在的方案是把链接从 `post/文件名.index` 改成了 `post/文件名/index.html`
然后 pretty_url 就会自动去掉 index.html 来修复这个问题