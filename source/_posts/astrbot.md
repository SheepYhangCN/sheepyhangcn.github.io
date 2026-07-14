---
title: 搭建 AstrBot 并链接到 QQ Bot
date: 2026-07-14 03:03:11
updated: 2026-07-14 18:03:33
categories: [研究记录]
repo: AstrBotDevs/AstrBot
---
## 背景故事
我于 2024 年在阿里云上购置了一台 2 核 2 GiB 内存
固定带宽，包年包月的每个用户仅限一台的 99 元/年套餐
运行的是 Ubuntu，装机时是 22.04，现在已经是 26.04 了
换现在估计我会选 Rocky Linux 而不是 Debian 系（

当初是搞来搭建 TeamSpeak 语音服务器的 这个性能也跑不了游戏
甚至之前试过跑 Koishi 框架一段时间，然后整个服直接卡死得手动重启

然后不久前发现了 [AstrBot](https://astrbot.app/) 这个项目
我之前一直没有搞 OpenClaw 是因为那个项目被 Vibe 出来的屎山搞的太烂了
现在想搞是因为有时我会想要在手机上用 LLM 帮我聚合信息
以及帮我控制服务器，看情况啥的，所以这次决定搞来试试

## 安装 AstrBot
首先是安装 AstrBot
第一个尝试的方案是 1Panel 应用商店
先提前配置 Docker 镜像，然后在 1Panel 安装 AstrBot
不过安装之后测试才发觉，因为其是装在 Docker 里面
所以没办法进行操作系统里的某些操作，比如查进程之类的
因为我有控制服务器的需求，所以最后放弃了这个方案

官方的推荐方法是用 uv，但是 uv 似乎不能在 WebUI 里面升级
对我来说是个扣分点，我还是希望能和 1Panel 一样方便地更新
所以最后尝试了手动安装，使用 venv 运行

### 使用 venv
首先安装 `venv`
Debian 系确实是神秘，不是用一般的 `pip install python3-venv`，而是 `apt-get install python3-venv`
用命令拉取最新 Release 的源码
```
git clone --depth=1 --branch $(git ls-remote --tags --sort='-v:refname' https://github.com/AstrBotDevs/AstrBot.git | head -n1 | awk -F/ '{print $3}') https://github.com/AstrBotDevs/AstrBot.git
```
然后进入虚拟环境，安装依赖，并启动
```
source venv/bin/activate
python -m pip install -r requirements.txt
python main.py
```
启动后日志里就会有默认用户名和密码了
然后就可以进入 `<服务器 IP>:<端口>` 访问 WebUI，默认端口应为 `6185`
如果进不去的话最好检查一下防火墙和阿里云那边有没有放行端口

### 开机自启
运行 main.py 后理应就会自动添加 `astrbot.service`，可以在 systemctl 里控制
如果没有的话可以修改 `/etc/systemd/system/astrbot.service` 自己编写配置
```
[Unit]
Description=AstrBot Service
After=network.target network-online.target
Wants=network-online.target

[Service]
Type=simple
User=root
WorkingDirectory=<AstrBot 安装目录>
ExecStart=<AstrBot 安装目录>/venv/bin/python <AstrBot 安装目录>/main.py
Restart=always
RestartSec=5
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

## 配置 AstrBot
进入 WebUI，用设置好的账号密码登入
进入后先配置 LLM 模型，我用的是阿里云百炼的 DeepSeek V4 Flash
对于这种模型一般都是推荐用便宜快速的，所以 DS 是个很不错的选择
相同的价格的话小米 MiMo 应该也行？

### 配置 QQ 机器人
接着接入消息平台
原计划是用 Telegram 的
到了这一步才想起来我服务器在内地，连不了 API
所以只能退而求其次地选 QQ Bot 了
目前的 QQ 机器人平台拿来做这种私人助理还是够用的
而且目前 AstrBot 可以直接扫码创建

我之前研究 Koishi 框架时已经注册过了开放平台账号
主要就是要填信息，实名认证个人账户，绑定一个 QQ 作为超级管理员
然后还要注意普通的机器人创建了是要找客服才能删除的
所以不要乱创机器人 个人用户只有五个 Bot 额度（

对于 AstrBot 来说，创建好之后开放平台的其它东西就都不用填了
直接用自己的 QQ 加好友私聊即可
至少我是没有群聊使用的需求的
以前的 Bot 可以设置 QQ 群沙箱，不知道为什么现在的没了

可以在私聊里发一条消息验证一下能不能正常回复

### 配置 AstrBot
放行 Agent 访问电脑
然后进入左侧「配置文件」
运行环境设置为 Local
然后还得在「平台配置」设置一下管理员
在聊天会话输入 `/sid` 即可获取
当然直接在「AI 配置」里关闭「需要 AstrBot 管理员权限」也行就是了

然后下面的「显示思考内容」和「流式输出」也可以开一下
QQ Bot 是可以用流式输出的
毕竟之前特地支援了 OpenClaw
「输出函数调用状态」和「输出函数调用返回结果」也打开

### 配置 Tavily 网页搜索
在 AstrBot 配置文件的「AI 配置」里，可以开启网页搜索
这是透过第三方服务实现的
我选的是 [Tavily](https://www.tavily.com/)
每个账户每月有 1000 Credits 的搜索额度

进入官网使用 Google 账户登录
然后复制 API Key 填入即可

## 插件
目前是研究了俩插件
俩插件都遇到坑了

### B 站视频总结
一个是 [astrbot_plugin_biliVideo](https://github.com/storyAura/astrbot_plugin_biliVideo)
用于哔哩哔哩视频总结
扫码登录和总结都没毛病
生图有个很抽象的问题就是 Markdown 语法会失效
改成纯文字就没问题

配置中有个生成合并转发聊天记录的功能
实测开了这个就发不出来了
所以目前就是一大串文字这么用

分 P 视频也用不了，只能识别 P1
不过这个可能是 B 站 API 问题就是了

### 日报
另一个是 [astrbot_plugin_zhenxunribao](https://github.com/luminacry/astrbot_plugin_zhenxunribao)
这个具体来说是我的服务器问题
2 GiB 内存的机器，在 TeamSpeak + AstrBot 的情况下就已经吃掉了一半
而这个插件需要使用 Playwright 运行浏览器，还是我们最爱的 Chromium
所以就不负众望的卡死了

所以目前实际用的是 [astrbot_plugin_dailyhub](https://github.com/AMag1c/astrbot_plugin_dailyhub)
注意要把配置里 AI 日报的 RSS 地址改成 `https://daily.juya.uk/rss.xml`

还有个悲报是众所周知不久前 Bangumi 被墙了，所以这个插件的「今日番剧」是拿不到的
而且这个插件的配置*截至目前*没法改 Bangumi 的镜像
所以这里是一个[添加配置项的 PR](https://github.com/AMag1c/astrbot_plugin_dailyhub/pull/2)

可以直接 fork 修改后的仓库
然后修改 AstrBot 目录下 `data/plugins/astrbot_plugin_dailyhub/metadata.yaml` 的 `repo`
然后去 WebUI 更换插件源到 Github，再选重新安装即可
这会保留原有配置
当然，如果 PR 合并了之后那就不用搞了（

我目前用的 Bangumi API 镜像是自己部署的
使用[这篇文章](https://catcat.blog/2026/05/bangumi-reverse-proxy)的 Cloudflare Worker 方案