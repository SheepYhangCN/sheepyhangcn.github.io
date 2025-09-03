---
title: 自用HTML+CSS学习笔记
date: 2025-09-03 18:32:32
tags:
 - 学习笔记
 - HTML
 - CSS
---
## 结构
html ( head (网页头) body (网页内容)  )

## HTML 标签
`<!doctype html>` 首行必写（单标签）
`<html>` 必写（双标签）
- `lang` 语言（见 RFC 5464）
  - `zh-Hans-CN` 简体中文
  - `zh` 中文

### Head 网页头
`<meta charset="utf-8">` 网页代码页（单标签）
`<title>` 标题（双标签）

### Body 网页内容
#### 单标签
`<hr>` 分割线
- `width` 长度
- `size` 粗细

`<br>` 换行
#### 双标签
`<p>` 段落
- `align` 对齐 
  - `left`
  - `center`
  - `right`
  
`<h*>` 标题
- `*`为1到6

`<img>` 图片
- `src` 图片路径

`<a>` 链接
- `href` 链接地址
- `target` 进入方式
  - 默认`_self`当前标签页打开
  - `_blank`新标签页打开

`<ol>` 有序列表
`<ul>` 无序列表
- `<li>` 列表的项