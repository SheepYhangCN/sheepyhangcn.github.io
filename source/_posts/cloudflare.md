---
title: Namesilo域名+CDN加速
date: 2024-01-18 01:10:32
tags:
 - Blog
---

之前是一直不想搞域名和CDN的
毕竟不管怎么样域名还是要钱的
而且我懒得折腾 就一直没搞
然后我昨天看到说Namesilo的域名价格很便宜
我就上去看了下
诶我草 .top域名仅1.88美刀/年
然后就 顺手把Cloudflare也整了 嗯

## Namesilo域名
在Namesilo注册好账号
然后买了个```sheepyuhang.top```
用1美刀券减成了1美刀首年（我也不知道为啥实际减了0.88美刀
嗯 就很值
一开始用Paypal付款 付了三次
每次都是在Paypal按了付款 然后转了好几分钟
然后窗口自己关掉 没有任何反应
然后我用支付宝就支付成功了
难绷

因为要用Cloudflare弄CDN
所以Namesilo这边不用改DNS
不过为了测试 我还是弄了下
打开Domain Manage
点开我的域名 点开```DNS Records```
设置一个CNAME类型 填```www```与我Blog本来的github.io域名
再把自带的空白hostname的A类型的地址改成原来github.io域名ping出来的ip即可

在Github仓库的Settings→Pages
把Custom Domain填上就可以了
嘛 理论上是这样 但是当时不管用
因为我要弄CF 所以就没管

## Cloudflare CDN
打开Cloudflare 新建一个站点
设置好域名
DNS和Namesilo那个一样
但是这次 空白的A类型可以换成CNAME类型
内容同样填成github.io那个域名 名称填@即可
设置好后 把给出的名称服务器填进Namesilo的NameServers里就可以了
接着就是等待Cloudflare接管域名

嘛 实际后续过程还是挺挫折的
Github那边不知道发什么巅
一直把我github.io的域名跳转到我自己的域名
即使我把Custom Domain删掉 仍然是这样

其实只要```等```就可以了
过了一会我再去测试
哦豁 ERR_TOO_MANY_REDIRECTS
不过这个Stack Overflow上就有解
在Cloudflare把```SSL/TLS 加密模式```改成```完全```即可

## 结束
然后，就没了
并不难搞其实

然而我去测试了下
上了CDN的速度甚至不如直连速度
![Cloudflare](./resources/images/cloudflare/cloudflare.png)
Cloudflare
![直连](./resources/images/cloudflare/github.png)
github.io直连

那我费那么大劲弄CF干嘛
还不如上阿里云CDN 麻了

## 后续
2024-1-28 我又换成了阿里云CDN
但是目前测试阿里云CDN仍然没有直连的速度好
太奇怪了
2024-6-30 因为闲的所以又换回Cloudflare CDN了