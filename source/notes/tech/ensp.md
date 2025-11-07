---
title: 自用 eNSP 学习笔记
date: 2025-11-07 18:01:10
updated: 2025-11-07 18:01:10
notebook: notes
tags:
 - eNSP
---
`undo info-center enable` (`un in en`) 关闭消息日志
`sysname <设备名>` 设备命名
`system-view` 进入系统视图
`quit` 返回上一级视图
`return` 直接返回到用户视图
`save` 保存

同时操作多个接口视图
`port-group group-member <接口类型> x/y/z to <接口类型> x/y/z`

# Telnet
`user-interface vty <id> <id>` 进入 vty 视图（0 4 为 5 个）
`user-interface maximum-vty` 最大同时登录数量
## 参观/监控级
`authentication-mode password` 密码认证模式
`user privilege level <0/1>` 预设 0 参观级，1 监控级

## 管理级
`authentication-mode aaa` aaa 认证模式
`aaa` 进入 aaa 视图
`local-user <用户名> password <cipher/plain> <密码> privilege level <权限等级>` 配置用户账户和权限等级
`local-user <用户名> service-type telnet` 服务类型 Telnet

# Vlan
`vlan batch` 创建 vlan，后面跟所有需要创建的 vlanid ，空格分割
## 配置接口
`interface <接口>` 进入接口视图
`port link-type <access/trunk/hybrid>` 配置端口类型
### Access
`port default vlan <vlanid>` 配置 pvid
### Trunk
`port trunk pvid vlan <vlanid>` 配置 pvid
`port trunk allow-pass vlan` 配置放行 vlan，后面跟所有需要放行的 vlanid，空格分割
{% note 提示 若题目要求中没有放行 vlan 1，则需要手动 `undo port trunk allow-pass vlan 1` %}
### Hybrid
`port hybrid pvid vlan <vlanid>` 配置 pvid
`port hybrid untagged vlan` 配置剥离 vlan tag，后面跟所有需要放行的 vlanid，空格分割
`port hybrid tagged vlan` 配置添加 vlan tag，后面跟所有需要放行的 vlanid，空格分割
{% note 提示 untagged 的朝向 PC，tagged 朝向交换机 %}

# IP 位址
`interface <接口>` 进入接口视图
`ip address <IP 位址> <掩码>`

# STP
`stp mode <模式>` STP 模式
 - `mstp` 默认
 - `stp`
 - `rstp`

`stp instance <实例 ID> cost <开销值>` 链路堵塞 开销
`stp bpdu-protection` BPDU 保护
`stp priority <优先级>` 设置优先级
`stp root <优先根/备份根>` 设置根桥
- `primary` 优先
- `secondary` 备份
## 边缘端口和保护
`interface <接口>` 进入接口视图
`stp edged-port enable` 边缘端口
`stp root-protection` 根保护
`stp loop-protection` 环路保护

# Vlan 间路由
## 单臂路由
`interface <接口>.<子接口编号>` 进入子接口视图
[配置 IP 位址](#IP-位址)
`dot1q termination vid <vlanid>` 封装并承载 Vlan 流量
`arp broadcast enable` 开启 ARP 广播
## 三层交换
`interface vlanif <vlanid>` 进入 Vlanif 视图
[配置 IP 位址](#IP-位址)

# 链路聚合
`interface eth-trunk <ID>` 进入 Eth-Trunk 视图
`mode <模式>` 制定聚合模式
- `manual load-balance` 手动负载分担模式
- `lacp-static` 静态 LACP 模式（[LACP](LACP)）

## 配置成员接口
`interface <接口>` 进入接口视图
`eth-trunk <ID>` 加入聚合组

{% note 提示 修改 IP 位址时，如果是路由器设备，<br>遇到 `Error: The layer-3 interface can not add into a layer-2 trunk`<br>则需要 `undo portswitch` 关闭二层模式<br>需要在设置模式和配置成员接口前执行，如果已经执行了需要 `undo <命令>` %}

## LACP
### 系统视图下
`lacp priority <优先级>` LACP 系统优先级
`max active-linknumber <最大活动接口数>` 活动接口上线阈值
### 接口视图下
`lacp priority <优先级>` LACP 接口优先级
`lacp preempt` LACP 抢占
- `enable` 开启
- `delay <时间>` 抢占延迟

# 静态路由
`ip route-static <目标网段> <掩码> <下一跳> preference <优先级>` 静态路由
- `<目标网段 = 0.0.0.0> <掩码 = 0>` 默认路由

# 动态路由
## RIP
`rip <进程号>` 创建 RIP 进程
`undo summary` 关闭自动汇总
`version <版本号>` 指定 RIP 版本
`network <网段>` 宣告网段
`import-route <来源>` 在 RIP 网络中引入路由
- `direct` 直连路由
- `static` 静态路由
- `ospf <进程号> cost <开销值>` OSPF 路由

`default-route originate` 发布默认路由

## OSPF
`ospf <进程号> [router-id <ID>]` 创建 OSPF 进程，配置 Router-ID（进入进程视图）
`router id <ID>` 设置 Router-ID
`area <区域ID>` 创建区域（进入区域视图）
### 进程视图下
`import-route rip <进程号>` 引入 RIP 路由
`default-route-advertise [always]` 配置 OSPF 发布默认路由
`silent-interface <接口>` 配置 OSPF 被动静态接口
`preference <优先级>` 设置优先级
### 区域视图下
`network <网络地址> <反掩码>` 宣告网段
`authentication-mode` 区域认证模式
- `simple <plain/cipher> <口令>` 简单模式
- `md5 <验证字标识符 ID> <口令>` MD5 模式
### 接口视图下
`ospf dr-priority <优先级>` 设置优先级，数字越大优先级越高
`ospf authentication-mode` 链路认证模式
- `simple <plain/cipher> <口令>` 简单模式
- `md5 <验证字标识符 ID> <口令>` MD5 模式

# VRRP
`interface <接口>` 进入接口视图
`vrrp vrid <备份组ID>`
- `virtual-ip <虚拟 IP>` 创建备份组
- `priority <优先级>` 设置优先级，数字越大优先级越高
- `track interface <监视接口号> reduced <裁剪优先级值>` 接口监视
- `authentication-mode <认证方式> <密码>` 认证

# ACL
`acl <ACL 编号>` 创建 ACL
{% note 提示 题目要求只允许 = 要先 `rule <规则 ID> deny all` %}
## 基本 ACL（编号范围 2000-2999）
`rule <规则 ID> <permit/deny> source <源位址> <反掩码>` 定义 ACL 规则
## 高级 ACL（编号范围 3000-3999）
`rule <规则 ID> <permit/deny> <协议> source <源位址> <反掩码> destination <目的位址> <反掩码> detination-port <操作符> <端口号/服务名>` 定义 ACL 规则
## 调用 ACL
### 接口下调用
`interface <接口>` 进入接口视图
`traffic-filter <inbound/outbound> acl <ACL 编号>` 调用 ACL
### vty 下调用
`user-interface vty <id> <id>` 进入 vty 视图
`acl <ACL 编号> <inbound/outbound>` 调用 ACL

# PPP 认证
## 认证方
`interface <接口>` 进入接口视图
`link-protocol ppp` 指定协议为 PPP
`ppp authentication-mode <pap/chap>` 指定认证方式
`quit` 退出视图
`aaa` 进入 aaa 视图
`local-user <用户名> password <cipher/plain> <密码>` 配置用户账户
`local-user <用户名> service-type ppp` 服务类型 PPP
## 被认证方
### PAP
`interface <接口>` 进入接口视图
`link-protocol ppp` 指定协议为 PPP
`ppp pap local-user <用户名> password <cipher/plain> <密码>`
### CHAP
`interface <接口>` 进入接口视图
`ppp chap user <用户名>`
`ppp chap password <cipher/plain> <密码>`

# DHCP
`dhcp enable` 开启设备 DHCP
## 接口 DHCP
`interface <接口>` 进入接口视图
`dhcp select interface` 启用接口 DHCP
`dhcp server lease day <天数> hour <小时> minute <分钟>` 租期
`dhcp server gateway-list <IP 位址>` 网关位址
`dhcp server excluded-ip-address <开始 IP> <结束 IP>` IP 位址排除范围
`dhcp server dns-list <IP 位址>` DNS 位址
## 全局 DHCP
`ip pool <名称>` 创建 IP 池
`network <网段> mask <掩码>` 可分配网段
`lease day <天数> hour <小时> minute <分钟>` 租期
`gateway-list <IP 位址>` 网关位址
`excluded-ip-address <开始 IP> <结束 IP>` IP 位址排除范围
`dns-list <IP 位址>` DNS 位址
`quit` 退出视图
`interface <接口>` 进入接口视图
`dhcp select global` 启用全局 DHCP
# IPv6
`ipv6` 开启 IPv6 功能
`interface <接口>` 进入接口视图
`ipv6 enable` 开启 IPv6
## 自动生成链路
`ipv6 address auto link-local`
## 手动配置位址
`ipv6 address <IPv6 位址> <前缀长度>`
## EUI-64 方式配置位址
`ipv6 address <位址前缀> <前缀长度> eui-64`

# NAT
## 静态 NAT
`interface <接口>` 进入接口视图
`nat static global <公有 IP> inside <私有 IP>`
## 动态 NAT / NAPT
`nat address-group <地址池编号> <开始 IP> <结束 IP>` 创建位址池
`acl <ACL 编号>` 创建 ACL
`rule <规则 ID> <permit/deny> source <源位址> <反掩码>` 配置 ACL
`interface <接口>` 进入接口视图
`nat outbound <ACL 编号> address-group <位址池编号> [no-pat]` 关联 ACL 与位址池
## Easy-IP
`acl <ACL 编号>` 创建 ACL
`rule <规则 ID> <permit/deny> source <源位址> <反掩码>` 配置 ACL
`interface <接口>` 进入接口视图
`nat outbound <ACL 编号>` 配置 Easy-IP
## NAT Server
`interface <接口>` 进入接口视图
`nat server protocol <协议> global <公有 IP> <服务端口号/关键字> inside <私有 IP> <服务端口号/关键字>`
`quit` 退出视图
`nat alg <服务> enable` 开启 NAT ALG

# 检验检查命令
`display this` 显示当前视图下配置
`display interface eth-trunk <ID>` 显示链路聚合配置
`display eth-trunk <ID>` 显示链路聚合配置
`display port vlan` 显示 VLAN 配置
`display ip interface brief` 显示 IP 位址配置摘要
`display ip routing-table` 显示路由表
`display stp` 显示 STP 配置
`display stp brief` 显示 STP 配置摘要
`display stp interface <接口>` 显示接口 STP 配置
`display ospf interface` 显示 OSPF 配置
`display ospf peer [brief]` 显示 OSPF 邻居状态
`display ip routing-table protocol ospf` 显示 OSPF 路由表
`display vrrp` 显示 VRRP 配置
`display vrrp brief` 显示 VRRP 配置摘要
`display vrrp interface <接口> [brief]` 查看 VRRP 工作状态
`display acl all` 查看所有 ACL 规则
`display ip pool` 查看 DHCP 位址池
`display ipv6 interface <接口>` 查看 IPv6 接口配置
`display ipv6 interface brief` 查看 IPv6 配置
`display nat static` 查看静态 NAT 配置
`display nat outbound` 查看 NAT Outbound 配置
`display nat server` 查看 NAT Server 配置