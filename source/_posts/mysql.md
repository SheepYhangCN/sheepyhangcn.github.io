﻿---
title: 自用MySQL学习笔记
date: 2025-05-15 16:23:32
tags:
 - 学习笔记
 - MySQL
---

`<>`：参数
`[]`：可选参数

## 数据库/表的创建与删除
### 创建数据库
```
create database [if not exists] <数据库>;
```
### 创建表
```
create table [if not exists] <表>
(
<键> <数据类型> [约束条件],
<键> <数据类型> [约束条件]
);
```
### 删除数据库/表
```
drop database/table [if exists] <数据库/表>;
```

## 数据类型
数字：`int` `float` `double` `decimal`
日期时间：`date` `time` `year` `datetime` `timestamp`
`varchar(<字符数>)` 可变长度字符串，字符数为最大字符数，常用值`50`
`char(<字符数>)` 固定长度字符串，常用值`50`

## 约束条件
`auto_increment` 自动增加
`primary key` 主键
`unique` 唯一
`not null` 非空
`default <值>` 默认值

## 表结构

### 获取
```
desc <表>;
```

### 修改
```
alter table <表>
<命令>;
```
命令：
添加列：`add column <键> <类型> [约束条件]`
修改列：`modify column <键> <类型> [约束条件]`
改列名：`change column <原键名> <新键名> <类型>`
删除列：`drop column <键>`
添加主键约束条件：`add primary key (<键>)`
添加外键约束条件：`add constraint key(<键>) references <表>(<键>)`
修改表名：`rename to <新表名>`

### 外键约束
```
foreign key(<键>) references <表>(<键>);
```

## 表内容
### 获取
```
select [distinct] <表达式>,<表达式> from <表>
[limit 行数]
[where 条件];
[order by <键> [asc/desc], <键> [asc/desc]]
[group by <键> [having <表达式>]]
```
表达式：`<键>` `*全选` `<统计函数>`
`distinct`：去除重复

#### 多表连接
隐式内连接：`select <表>.<键>,<表>.<键> from <表>,<表> where <条件>;`
显式内连接：`select <表>.<键>,<表>.<键> from <表> [inner] join <表> on <条件>;`
显式外连接：`select <表>.<键>,<表>.<键> from <表> left/right/full [outer] join <表> on <条件>;`
条件：
```
<表>.<键> = <表>.<键>
```

`select <表>.<键>,<表>.<键> from <表> full [outer] join <表> on <条件>;` = 
```
select <表>.<键>,<表>.<键> from <表> left [outer] join <表> on <条件>
union
select <表>.<键>,<表>.<键> from <表> right [outer] join <表> on <条件>;
```

#### 统计函数
计数：`count(<键>)`
总和：`sum(<键>)`
平均：`avg(<键>)`
最大：`max(<键>)`
最小：`min(<键>)`

### 添加行
```
insert into <表>(<键,键>)
values
(<值,值>),
(<值,值>);
```

### 删除值
```
delete from <表>
where <条件>;
```
### 修改值
```
update <表>
set <键>=<值>,<键>=<值>
where <条件>;
```
### 清空表
```
truncate table <表>;
```

### `where` 条件
`<表达式> ><= <值>`
`and` `or`
表达式：`<键>` `+-*/加减乘除` `%取余` `//取整`
#### datetime表达式
日期部分：`date(<键>)`
年部分：`year(<键>)`
月部分：`month(<键>)`
日部分：`day(<键>)`
介于时间之间：`between <时间> and <时间>`

#### 字符串匹配
`<键> like "匹配"`
```
匹配：
%：任意字符，不限字数
_：任意字符，限制字数
```
字符串长度：`char_length(<键>)`
字符串开头结尾：`left/right(<键>,<长度>)`