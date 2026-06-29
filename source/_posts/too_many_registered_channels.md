---
title: 修复游玩服务器时遇到的 Too Many Registered Channels
date: 2026-06-08 15:04:30
updated: 2025-06-29 17:39:29
repo: SheepYhangCN/FixTooManyRegisteredChannels
tags: 
 - Minecraft
---

我有一个专门的原版优化 Modpack，时不时跟着原版更新
目前有两百多个 Mod
然而这些 Mod 在加入多人游戏时都会注册自己的通道用于传输数据
而某些服务器核心会限制玩家注册的通道数量
就会报出 `Too Many Registered Channels` 错误
{% image https://cdn.modrinth.com/data/X1YbXgv4/images/cebd62adbd569e4d542ab70ffee3a7e8b9f7698f.png  ratio:829/400 %}

技术上来说，这个问题的解决不算麻烦
只要在游玩多人游戏时禁止所有 Mod 的注册通道即可，毕竟本身也是 Vanilla-Compatiable 的 Modpack

然而我是一点 Java 也不会
所以只能在 LLM 的帮助下做出了这个 Mod
以 YACL 与 Mod Menu 作为前置，可以从 Mod Menu 打开配置选单，随时开关

{% button Modrinth https://modrinth.com/mod/fix-too-many-registered-channels %}
{% button MCMOD https://www.mcmod.cn/class/27601.html %}

## 技术细节
主要就是两个 Mixin
一个是 Inject `registerChannels` 与 `registerChannel`
查看通道的 namespace，不是 `minecraft:` 就阻止
删掉 `EntrySet`，返回 false

另一个是 Inject `registerGlobalReceiver`
这个是 Fabric API 的注册
只要在多人游戏下就阻挡，返回 false