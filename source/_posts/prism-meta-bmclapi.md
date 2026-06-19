---
title: 让 Prism Launcher 使用 BMCLAPI
date: 2026-05-22 22:26:26
updated: 2026-06-20 02:23:36
repo: SheepYhangCN/prism-meta-bmclapi
tags: 
 - Minecraft
 - Cloudflare
---

[Prism Launcher](https://prismlauncher.org/)（以及其的前世 PolyMC 与前前世 MultiMC）是我很喜欢，且一直都在使用的 Minecraft 启动器
在多实例，有着一大堆 Modpack 的情况下非常好用
不过毕竟是国外制作的启动器，内嵌 [BMCLAPI](https://bmclapidoc.bangbang93.com/) 就不大可能了
好在，当前版本的 Prism Launcher 允许自定义 Metadata 与 Assets 源

## Assets
在 [Prism Launcher 10.0.0](https://prismlauncher.org/news/release-10/)（[PrismLauncher/PrismLauncher#3875](https://github.com/PrismLauncher/PrismLauncher/pull/3875)）时其增加了自定义 Assets 源的功能
默认情况下为 `https://resources.download.minecraft.net/`
直接填写 `https://bmclapi2.bangbang93.com/assets/` 进去即可

## Metadata
剩下的资源，则全部都在 Metadata 中，也就是默认情况下的 `https://meta.prismlauncher.org/v1/`
这个源本质上是一个 Github Pages 静态站点，其中存储了所有资源的源地址
所以，最简单的方法，就是直接设置一个反代，从 Prism Launcher 官方源获取内容后，替换所有源为 BMCLAPI 域名

### 散列值校验
默认情况下，这个 Meta 源的索引里包含了版本 Json 的散列值，供 Prism Launcher 下载时校验
Worker 替换后散列值必然发生变化，所以必须进行处理
解决方法是获取到 Json 后解析，然后删除掉 `sha256` 与 `sha1` 字段
然后重新转换为字符串返回

### 刷新缓存
删除 Prism Launcher 目录下的 `meta` 文件夹以刷新缓存

## Cloudflare Workers
[worker.js](https://github.com/SheepYhangCN/prism-meta-bmclapi/blob/main/worker.js)
在 AI 的协助下写出的一个简单的 JavaScript，用于部署一个 Cloudflare Worker
Worker 域名直接对应的就是 `https://meta.prismlauncher.org/v1/`
将 Worker 域名填入 Prism Launcher 的 Metadata 源中即可