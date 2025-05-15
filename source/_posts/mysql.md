---
title: 自用MySQL学习笔记
date: 2025-05-15 16:23:32
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
select [distinct] */<键>,<键> from <表>
[limit 行数]
[where 条件];
[order by <键> [asc/desc], <键> [asc/desc]]
```
`distinct`：去除重复

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
`<键> ><= <值>`
`and` `or`
字符串匹配：`<键> like "匹配"`
```
匹配：
%：任意字符，不限字数
_：任意字符，限制字数
```
字符串长度：`char_length(<键>)`
字符串开头结尾：`left/right(<键>,<长度>)`