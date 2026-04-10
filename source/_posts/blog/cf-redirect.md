---
title: 基于单域名 + CF SaaS 的 CF CDN 优选
date: 2026-04-10 13:54:02
updated: 2026-04-10 13:54:02
categories: [博客更新]
repo: SheepYhangCN/sheepyhangcn.github.io
tags: 
 - Blog
---

详细更多方式见[这篇文章](https://2x.nz/posts/cf-fastip/)
现在这篇是我目前实际设置的流程 供环境类似的参考

## 前置环境
网页搭建于 Github Pages
正在使用 Cloudflare DNS
需要 Cloudflare 绑定支付方式以激活 SaaS（不需要付费，100 个域名内是免费的）

若使用单个域名则不可把启用优选的域名设在根域名上（SaaS 不允许）
多个域名则没有这个问题
因为我只有一个域名所以把博客换到了 [https://blog.sheepyuhang.top](https://blog.sheepyuhang.top) 子域上
但是后面也会设置根域名和 `www` 的重定向 不怕相容性问题

文中的 `<域名>` 就替换为自己的二级域名就行，本站的即是 `sheepyuhang.top`
后文中所有子域命名任意，没有限制，自己能认出就行
`blog` 即为最终用于访问的子域

## 设置 DNS 记录
`cdn` 指向 Cloudflare 优选域名，可以前往[https://cf.090227.xyz](https://cf.090227.xyz)查看，亦或是直接把这个域名填进去 CNAME 也行，关闭 Proxy
`direct` 指向网页源站，也就是 `<用户名>.github.io`，打开 Proxy
`blog` 指向 `cdn.<域名>`，也就是上文设置的优选域名，关闭 Proxy
`saas` 指向无任何要求，只需要打开 Proxy
以下记录用于重定向
`@`（跟域名）指向无任何要求，只需要打开 Proxy
`www` 同上，打开 Proxy

## 设置 SaaS
打开「SSL/TLS」下的「自定义主机名」
设置回退源为 `saas.<域名>`，保存
点击「添加自定义主机名」
「自定义主机名」为 `blog.<域名>`
「证书验证方法」为「HTTP 验证」
「自定义源服务器」为 `direct.<域名>`

## 已起效
现在优选已起效
可以通过 [DNS 查询](https://www.itdog.cn/dns/) 选择 CNAME 测试查看是否映射到了 `cdn.<域名>` 上

## 设置重定向
为了保证以前的链接不出问题，就得加重定向
一共三个，It just works 所以就别挑啥了
均为 301 重定向

### `www` 转接 `blog`
「请求 URL」`https://www.*`
「目标 URL」`https://blog.${1}`

### 根 转接 `blog`
两个
「请求 URL」`https://<域名>*`
「目标 URL」`https://blog.<域名>${1}`
另一个是没有后缀时的转接，用自定义运算式
「完整 URL」「等于」`https://<域名>`
「静态」`https://blog.<域名>`