---
title: 利用 Docker 在 Windows 本地部署 Weblate
date: 2026-01-08 01:37:30
categories: [研究记录]
tags: 
 - Docker
 - Weblate
---

一个记录
因为需要一个译文处理的平台
所以决定研究一下本地部署 Weblate
byd给到我手上的译文是txt一行一条没有key
这我翻个毛

## 安装 Docker Desktop
需要提前开启 Hyper-V 并安装 WSL 2
从[官网](https://www.docker.com/products/docker-desktop/)下载并安装 Docker Desktop
然后对应的 CLI 与 docker-compose 也会自动安装好

然后因为 GFW 的缘故，内地网路访问 Docker Hub 坠机了
所以需要进行一个镜像的设置
打开 Docker Desktop 的设置
选择「Docker Engine」
在最后一项的下一行加上
```
  "registry-mirrors": [
    "<镜像站域名1>",
    "<镜像站域名2>"
  ]
```
就行
加前最后一项最后的逗号别漏了
镜像站见[这里](https://sheepyuhang.top/links/#%E9%95%9C%E5%83%8F%E7%AB%99)

## 拉取镜像并部署
从 https://github.com/WeblateOrg/docker-compose 拉取 Weblate 的官方镜像
然后直接命令行 cd 到拉取下来的仓库目录里
运行 `docker-compose up`
就会自动拉取补全需要的镜像并部署容器
（一个 valkey 一个 postgres）
启动完成后就可以通过 `localhost:80` 进行访问了

## 修改配置
打开 Docker Desktop
选择左边的 Containers
进入「docker-compose」
将里面的三个服务都停止
然后进入 docker-compose 仓库的目录
根据[文档](https://docs.weblate.org/zh-cn/latest/admin/install/docker.html#docker-environment-variables)来修改 `docker-compose.override.yml` 里的 `environment`
完成后保存再运行 `docker-compose up` 即可