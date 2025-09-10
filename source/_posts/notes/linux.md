---
title: 自用Kylin Linux学习笔记
date: 2025-03-06 19:40:50
tags:
 - 学习笔记
 - Linux
---

Ctrl+C 终止
Ctrl+Z 暂停并挂起

## 命令
`echo "<字符串> | cat >> <档案名>"` 把<字符串>写入<档案名>档案中

`<命令> | grep <内容>` 在命令输出结果中搜索内容

`shutdown <参数1> <参数2>` 关机
<table>
	<tbody>
		<tr>
			<td>参数1</td>
			<td>-h/poweroff 关机<br>-r/reboot 重启</td>
		</tr>
		<tr>
			<td>参数2</td>
			<td>now 立刻<br>+* *分钟后</td>
		</tr>
	</tbody>
</table>

帮助
```
help <命令>
man <命令>
info <命令>
<命令> --help
```

`yum [参数] <命令>` 包管理
- `-y` 自动yes
- `-q` 静默

命令
- `list` 列出已安装和可安装
- `info <包>` 详细信息
- `install <包>` 安装
- `remove <包>` 卸载
- `update <包>` 升级
- `clean` 清理缓存
- `groupinfo <包组>` 组信息
- `groupinstall <包组>` 安装组
- `grouplist` 列出组
- `groupremove` 卸载组

yum 源位置
```
/etc/yum.repos.d/kylin_x86_64.repo
```

`rpm [参数] <包>` Red Hat 包管理
- `-q` 查询
- `-qa` 列出所有
- `-qi` 查询详细
- `-ql` 列出安装详细
- `-i` 安装
- `-v` 详细
- `-e` 删除
- `-u` 升级
- `-h` \#进度条

`tar [参数]` 压缩
- `-c <目录>` 建立压缩档案
- `-cf <压缩档案名> <目录>` 建立到压缩档案
- `-x <压缩档案名>` 解压压缩档案
- `-xC <压缩档案名> <目录>` 解压到目标目录
- `-t <压缩档案名>` 查询压缩档案内容
- `-v <压缩档案名>` 列出详细信息
- `-j` bzip2
- `-z` gzip

`ps [参数]` 查看进程
- `-a` 现行终端 包括其它用户
- `-u` 以用户为主
- `-l` 详细
- `-x` 所有，不分终端

`top` 系统进程监控
`jobs [参数]` 查看后台作业
- `-p` 只显示进程号
- `-l` 同时显示进程号和作业号

`bg/fg [作业号]` 切换作业到后台/前台
`nice -<优先级> <命令>` 以指定优先级运行命令
`renice <优先级> <参数>` 重设优先级
- `-p <进程号>` 进程号
- `-u <用户名>` 用户
- `-g <GID>` 组

`kill <进程号/作业号>` 杀死作业
`killall` 杀死所有作业

`-9 SIGKILL -15 SIGTERM`

`systemctl <命令> [参数] <服务单元>` 服务操作
命令
```
enable 启用
disable 禁用
status 状态
start 启动
stop 停止
restart 重启
reload 重载
kill 结束
is-enabled 是否可用
list-unit-files 列出可用
```

`at [参数] <时间>` 定时执行命令
- `-f` 文件
- `-l` 查看序列
- `-d` 删除
时间
```
hh:mm 时分
mmddyy / mm/dd/yy / mm.dd.yy 月日年
midnight = 00:00
noon = 12:00
teatime = 16:00
now+时间（xx minute/hour/days/weeb）
```

`crontab [参数]`
- `-e` 编辑配置档案
- `-l` 显示
- `-r` 删除

配置档案格式
`分 时 日 月 周几 命令`
不使用填写`*`

### 档案管理
`ls [参数]` 列出当前档案夹的档案
- `-l` 详细内容
- `--color <颜色>` 显示颜色

`ll` = `ls -l --color=auto`

`touch [参数] <档案名>` 创建档案
`mkdir [参数] <档案夹名>` 创建档案夹
- `-p` 上层不存在就一并创建

`rmdir [参数] <档案夹名>` 删除档案夹
- `-p` 上层空了就一并删除

`rm [参数] <档案(夹)名>` 删除档案(夹)
- `-i` 先问
- `-f` 不问
- `-r` 包括子档案目录

`cp [参数] <档案(夹)名> <目标位置/档案>` 复制
- `-i` 先问
- `-r` 包括子档案目录
- `-l` 硬链接
- `-s` 符号链接

`mv [参数] <档案(夹)名> <目标位置/档案>` 移动
- `-i` 先问
- `-f` 不问直接覆盖

`ln [参数] <档案(夹)名> <目标位置/档案>` 链接
- `-s` 软链接

`cat <档案名>` 查看档案内容
`head/tail [参数] <档案名>` 查看档案内容前/后10行
- `-n <行数>` 行数，不填默认10行

`wc <档案名>` 统计档案内容
- `-c` 仅显示行数
- `-w` 仅显示字数
- `-c` 仅显示字符数

`sort <档案名>` 档案内容行排序
- `-r` 按降序
- `-n` 按数字顺序
- `-u` 去除重复行

`uniq <档案名>` 去除重复行
- `-d` 只显示重复行
- `-u` 只显示非重复行

`diff <档案名> <档案名>` 比较差异
- `-u` 以统一格式输出
- `-q` 仅显示是否不同

### vi/vim
#### 模式
`命令` `末行` `插入`
命令输入冒号(:)进入末行
命令输入`i`进入插入
插入按下`Esc`进入命令
#### 命令模式
`v` 多选
`d` 剪切
`yy` 复制一行
`y` 复制
`p` 粘贴
`x` 删除一个字符
`y` 撤销
`dd` 删除一行
`*yy` 复制往下\*行
`G`(Shift+G) 移动到末行行首
`*G` 第\*行行首
`/<内容>` 搜索（`n` 下一个）
`<行1>,<行2>s/<内容1>,<内容2>/gc` 在行1与行2之间 批量替换内容1为内容2（`g`为全局 `c`为要询问）
- `%`/`1,$` 1到最后一行，`$`为最后一行

#### 末行模式
`:w` 保存
`:w >> <档案名>` 追加到档案末尾
`:wq`/`:x` 保存并退出
`:q!` 强制退出 不保存
`:n` 新建档案
`:set nu` 开启行号
`:set nonu` 关闭行号
`:<行号>` 跳转行号

### 用户/组管理
#### 用户
`useradd [参数] <用户名>` 添加用户
- `-d <家目录>` 设置家目录
- `-g <组>` 设置主组
- `-G <组>` 设置附加组
- `-u <UID>` 设置 UID
- `-s <Shell>` 设置 Shell

`passwd [参数] <用户名>` 设置密码
- `-d` 删除
- `-l` 锁定
- `-u` 解锁

`chage <用户名>` 强制要求用户修改密码
`usermod [参数] <用户名>` 修改用户
- `-p <密码>` 密码
- `-d <目录>` 家目录
- `-g <组名>` 主组
- `-G <组名>` 附加组
- `-l <登录名>` 登录名

`userdel [参数] <用户名>` 删除用户
- `-r` 一并删除家目录

`su [参数] <用户名>` 切换用户身份
- `-l` 重载环境变量

`sudo [参数] <命令>` 以指定身份用户运行命令，默认 root
- `-u <用户名>` 指定用户名，不指定时默认为 root

#### 组
`groupadd [参数] <组名>` 添加组
- `-g <GID>` 设置 GID

`groupmod [参数] <组名>` 修改组
- `-n <组名>` 组名
- `-g <GID>` GID

`gpasswd [参数] <组名>` 管理组内用户
- `-a <用户名>` 添加
- `-d <用户名>` 移除

`groupdel <组名>` 删除组

### 磁盘管理
常用设备类型
```
/dev/hd[a-d] IDE 设备
/dev/sd[a-d] SCSI 设备
/dev/fd[0-1] 软盘设备
/dev/cdrom 光驱
/dev/mouse 鼠标
```

分区流程
```
fdisk > mkfs > mount > /etc/fstab
```

`fdisk [参数]` 更改分区表
- `-l` 列出分区表

`mkfs [参数] <设备>` 创建档案系统
- `-t <档案系统>` 指定档案系统

`mount [参数] <设备> <目录>` 挂载设备到目录
- `-t <档案系统>` 指定档案系统

`df [参数]` 查询挂载
- `-h` 易读形式
- `-H` 以 1000 为进制
- `-T` 显示档案系统
- `-t <档案系统>` 只显示指定档案系统

`umount [参数] <挂载点/设备>` 卸载
- `-f` 强制

`xfs_quota -x -c "<命令>" <挂载点/设备>` XFS 配额
命令
```
report [-ugp] 信息
state [-ugp] 状态
enable/disable [-ugp] 启用/禁用
off [-ugp] 关闭
timer -u|-g|-p <值> 宽限时间
limit -u|-g|-p [bsoft/bhard/isoft/ihard]=<值> 限制
```

#### 开机自动挂载（/etc/fstab）
编辑 `/etc/fstab`
格式：
```
设备
挂载点
档案系统类型
挂载选项（defaults usrquota grpquota）
是否需要 dump（0 不要 / 1 要）
是否需要检查档案系统（0 不要 / 1 要 / 2 跳过）
```

### 网络配置
`ifconfig [接口名] [参数]`
- `netmask <掩码>` 设置子网掩码
- `broadcast <位址>` 广播位址
- `up/down` 开/关

`nmcli <对象> <命令> <参数>` Network Manager
<table>
	<thead>
		<th>对象</th>
		<th>命令</th>
	</thead>
	<tbody>
		<tr>
			<td>device</td>
			<td>connect disconnect monitor set status delete help modify reapply show wifi</td>
		</tr>
		<tr>
			<td>connection</td>
			<td>add delete edit help load monitor show clone modify reload up down</td>
		</tr>
	</tbody>
</table>
参数
```
ipv4.addresses IPv4 位址（x.x.x.x/xx 格式）
ipv4.dns IPv4 DNS
ipv4.gateway IPv4 网关
ipv4.method IPv4 连接方法（auto/manual）
ifname 网卡名
con-name 链接名
connection.autoconnect yes/no 是/否自动连接
```


网络配置文件
```
/etc/sysconfig/network-scripts/ifcfg-*
```
参数
```
DEVICE 设备名
NAME 链接名
UUID 唯一标识号
TYPE 类型（ethernet）
BOOTPROTO 协议（dncp/static/none）
ONBOOT 开机启动
IPADDR IP 位址
PREFIX 子网长度
GATEWAY 网关
DNS(1/2/3) DNS
```

### 权限管理
`chmod [参数] <权限> <档案(夹)名>` 修改档案权限
- `-R` 包括子档案目录

[八进制数形式档案权限](#八进制数形式档案权限)
或者`<身份>=<长形式权限>`的形式 使用逗号分隔
所有者(u) 所有者所在组(g) 其它用户(o) a=ugo
例如 `chmod 777 <档案>` == `chmod a=rwx <档案>` == `chmod u=rwx,g=rwx,o=rwx <档案>`
也可以使用 `<身份>+/-<长形式权限>` 来添加/去除权限

`chown <所有者>[:组] <档案>` 修改档案所有者/组
`chgrp <组> <档案>` 修改档案所有组
`chown root:root <档案>` == `chown root <档案>` + `chgrp root <档案>`

`umask [权限掩码]` 查看/修改权限掩码
[档案权限掩码](#档案权限掩码)

## 权限
### 档案权限
```
-rwxrwxrwx(长形式) 或 777(八进制数形式)
```

#### 长形式档案权限
第一位
`-` 普通档案
`l` 链接
`d` 档案夹
剩余三位为一个身份 共三个
分别为 所有者(u) 所有者所在组(g) 其它用户(o)
[长形式](#长形式)
#### 八进制数形式档案权限
三个数分别为 所有者(u) 所有者所在组(g) 其它用户(o)
[八进制数形式](#八进制数形式)

### 档案权限掩码
第一个数
`4`(SUID, Set User ID) 以所有者权限运行
`2`(SGID, Set Group ID) 以所有者所在组权限运行
`1`(t, sticky) 只有所有者才能删除
后三个数分别为 所有者(u) 所有者所在组(g) 其它用户(o)
[权限掩码](#权限掩码)

### 权限形式
#### 长形式
```
rwx
```
`-` 无
`r` 读取
`w` 写入
`x` 执行

#### 八进制数形式
```
7
```
`4` 读取
`2` 写入
`1` 执行
直接相加
无则为`0`

### 权限掩码
```
0
```
档案创建时默认权限为 `满权限 - 权限掩码`
档案满权限：`rw-`/`6`
目录满权限：`rwx`/`7`