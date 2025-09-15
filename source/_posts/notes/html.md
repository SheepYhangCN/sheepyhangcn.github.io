---
title: 自用HTML+CSS学习笔记
date: 2025-09-03 18:32:32
tags:
 - 学习笔记
 - HTML
 - CSS
---
`&nbsp;` non-breaking space 不折行空格

## 结构
html ( head (网页头) body (网页内容)  )

# HTML 标签
`<!doctype html>` 首行必写（单标签）
`<html>` 必写（双标签）
- `lang` 语言（见 RFC 5464）
  - `zh-Hans-CN` 简体中文
  - `zh` 中文

## `<head>` 网页头
`<meta charset="utf-8">` 网页代码页（单标签）
`<title>` 标题（双标签）

### `<style>` 内嵌样式
格式
```
选择器 { 参数: 值; 参数: 值; }
```
[style 样式参数](#style-样式参数)

优先级：`id 选择器 > 类选择器 > 标签选择器`

#### 标签选择器
直接使用标签 `body` `h1` `p` 等作为选择器

#### 类选择器
使用`.类名`（例如`.text`）作为选择器
在标签中加上`class="text"`来应用样式

#### id 选择器
使用`#id名`（例如`#text`）作为选择器
标签中加上`id="text"`来应用样式

## `<body>` 网页内容
### style 参数 行内样式
格式
```
<body style="<参数>: <值>; <参数>: <值>;">
```
[style 样式参数](#style-样式参数)

### 单标签
`<hr>` 分割线
- `width` 长度
- `size` 粗细

`<br>` 换行

`<img>` 图片
- `src` 图片路径
- `alt` 图片显示不出时显示的文本
- `width` 宽度
- `height` 高度

### 双标签
`<span>` 文本容器 用于同一行中应用不同样式

`<p>` 段落
- `align` 对齐 
  - `left`
  - `center`
  - `right`

`<h*>` 标题
- `*`为1到6

`<a>` 链接
- `href` 链接地址
- `target` 进入方式
  - 默认`_self`当前标签页打开
  - `_blank`新标签页打开

#### `<div>`
`width` 宽度
`height` 高度
`color` 文字[颜色](#颜色)

##### 边框
`border-width` 边框粗细 接受1-4个值 上右下左
- `thin/medium/think` 细到粗
- `数值` 直接设置

`border-style` 边框样式 接受1-4个值 上右下左
- `none` 无
- `dotted` 点画线
- `dashed` 虚线
- `solid` 实线
- `double` 双层
- `groove` 3D 沟槽
- `ridge` 3D 脊
- `inset` 3D 嵌入
- `outset` 3D 突出

`border-color"` 边框[颜色](#颜色) 接受1-4个值 上右下左

##### 背景
`background-color` 背景[颜色](#颜色)
`background-image` 背景图像
`background-position` 背景图像位置 默认为左上角
`background-repeat` 背景图像平铺方式 默认为`repeat`
- `repeat` 重复
- `repeat-x/y` 仅在水平/垂直方向重复
- `no-repeat` 不重复
- `inherit` 从父元素继承

##### 边距
`margin` 外边距 接受1-4个值 上右下左
`padding` 内边距 接受1-4个值 上右下左

#### 列表
`<ol>` 有序列表
- `type` 类型
  - `1` 数字
  - `a` 小写英文字母
  - `A` 大写英文字母
  - `I` 罗马数字

`<ul>` 无序列表
- `type` 类型 默认为`dise`
  - `dise` 实心圆
  - `circle` 空心圆
  - `square` 实心矩形

`<li>` 列表的项

#### 表格
##### 结构
```
table ( thead ( th th ) tbody ( tr ( td td ) tr ( td td ) ) )
```

`<table>` 表格
- `border` 外边框粗细，`0`时无边框
- `cellspacing` 单元格间距
- `cellpadding` 单元格内边距
- `width` 总宽度
- `height` 总高度

`<caption>` 表格外标题
`<thead>` 表格标题行
`<tbody>` 表格内容
`<th>` 表格列标题
- `width` 列宽度

`<tr>` 表格行
- `height` 行高度

`<td>` 格内容
- `width` 宽度
- `height` 高度
- `align` 对齐 
  - `left`
  - `center`
  - `right`
- `valign` 上下对齐 
  - `top`
  - `middle`
  - `down`
- `colspan` 水平跨度（跨过多列）
- `rowspan` 垂直跨度（跨过多行）

## style 样式参数
`color` [颜色](#颜色)
`background-color` 背景[颜色](#颜色)
`font-family` 字体 逗号分隔备选字体
`font-size` 字体大小
`font-style` 字体差分
- `normal` 正常
- `italic` 斜体
- `oblique` 倾斜的文字

`font-weight` 字体加粗
- `normal` 正常
- `bold` 粗体
- `数值` 直接加粗

`text-align` 文本对齐
- `left`
- `center`
- `right`
- `justify` 两端对齐
- `inherit` 从父元素继承

`line-height` 行间距
`letter-spacing` 字间距
`word-spacing` 单词间距

### 颜色
十六进制数 或 下列之一
```
black white red green blue fuchsia yellow cyan whitesmoke
```

### 伪类
选择器:伪类
`link` 未访问的链接
`visited` 已访问的链接
`hover` 鼠标悬停
`active` 选中的链接

`input:focus` 输入框获得焦点

`first-child` 父元素的第一个子元素
`last-child` 父元素的最后一个子元素
`nth-child(*)` 父元素的第\*个子元素


## 去除`<a>`超链接的特殊效果
```
<style>
a {
  color: inherit;
  text-decoration: none;
}
</style>
```