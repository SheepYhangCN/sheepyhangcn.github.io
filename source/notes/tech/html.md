---
title: 自用 HTML & CSS 学习笔记
date: 2025-09-03 18:32:32
updated: 2025-12-18 18:38:11
notebook: notes
tags:
 - HTML
 - CSS
---
# 查查表
## 转义字符
<table>
  <thead>
	<th>转义字符</th>
	<th>字符</th>
  </thead>
  <tbody>
	<tr>
	  <td>&amp;ensp;</td>
	  <td>半宽空格（&ensp;）</td>
	</tr>
	<tr>
	  <td>&amp;emsp;</td>
	  <td>全宽空格（&emsp;）</td>
	</tr>
	<tr>
	  <td>&amp;nbsp;</td>
	  <td>不折行空格（&nbsp;）</td>
	</tr>
	<tr>
	  <td>&amp;lt;</td>
	  <td>&lt;</td>
	</tr>
	<tr>
	  <td>&amp;gt;</td>
	  <td>&gt;</td>
	</tr>
	<tr>
	  <td>&amp;amp;</td>
	  <td>&amp;</td>
	</tr>
	<tr>
	  <td>&amp;quot;</td>
	  <td>&quot;</td>
	</tr>
  </tbody>
</table>

## 长度单位
<table>
  <thead>
	<th>类型</th>
	<th>单位</th>
	<th>解释</th>
	<th>换算</th>
  </thead>
  <tbody>
	<tr>
	  <td rowspan="7">绝对长度<br>（固定不变）</td>
	  <td>px</td>
	  <td>像素</td>
	  <td>N/A</td>
	</tr>
	<tr>
	  <td>cm</td>
	  <td>厘米</td>
	  <td>37.8px</td>
	</tr>
	<tr>
	  <td>mm</td>
	  <td>毫米</td>
	  <td>3.78px</td>
	</tr>
	<tr>
	  <td>Q</td>
	  <td>四分之一毫米</td>
	  <td>0.945px</td>
	</tr>
	<tr>
	  <td>in</td>
	  <td>英寸</td>
	  <td>96px</td>
	</tr>
	<tr>
	  <td>pc</td>
	  <td>派卡</td>
	  <td>16px</td>
	</tr>
	<tr>
	  <td>pt</td>
	  <td>磅</td>
	  <td>4/3px</td>
	</tr>
	<tr>
	  <td rowspan="7">相对长度<br>（取决于其它因素变化）</td>
	  <td>em</td>
	  <td>字符（字体大小）</td>
	  <td rowspan="7">N/A</td>
	</tr>
	<tr>
	  <td>rem</td>
	  <td>字符（相对于根元素的字体大小）</td>
	</tr>
	<tr>
	  <td>%</td>
	  <td>相对于父元素的百分比</td>
	</tr>
	<tr>
	  <td>vw</td>
	  <td>视图宽度 的百分比</td>
	</tr>
	<tr>
	  <td>vh</td>
	  <td>视图高度 的百分比</td>
	</tr>
	<tr>
	  <td>vmin</td>
	  <td>视图宽度与视图高度中<br>更小的那个的百分比</td>
	</tr>
	<tr>
	  <td>vmax</td>
	  <td>视图宽度与视图高度中<br>更大的那个的百分比</td>
	</tr>
  </tbody>
</table>

# HTML 标签

## 结构
html ( style (CSS样式) head (网页头) body (网页内容) )

`<!doctype html>` 首行必写（单标签）
`<html>` 必写（双标签）
- `lang` 语言（见 [RFC 5464](https://www.rfc-editor.org/rfc/rfc5646.html) 与 [IANA 语言子标签注册中心](https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry)）
  - `zh-Hans-CN` 简体中文
  - `zh` 中文

## `<style>` 内嵌样式
[CSS 参数](#CSS-参数)

## `<head>` 网页头
`<link rel="stylesheet" type="text/css" href="<CSS路径>">` 引入 CSS（单标签）
`<meta charset="utf-8">` 网页代码页（单标签）
`<title>` 标题（双标签）

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
- `style`
  - `background-color` 颜色
  - `border-style` 样式

`<br>` 换行

`<img>` 图片
- `src` 图片路径
- `alt` 图片显示不出时显示的文本
- `width` 宽度
- `height` 高度

`<input>` 输入表单
- `type` 类型
  - `text` 文本，默认
  - `button` 按钮
  - `checkbox` 复选框
  - `color` 颜色选择
  - `date` 日期
  - `datetime-local` 日期时间，不包含时区
  - `time` 时间
  - `month` 年和月
  - `week` 年和周
  - `file` 接受档案
  - `email` 电子信箱
  - `image` 图像
  - `password` 密码
  - `radio` 单选选项
  - `range` 拉动条组件
  - `search` 搜索
  - `tel` 电话
  - `url` URL
  - `submit` 提交按钮
  - `reset` 重设表单为预设
- `pattern` 检验用的表达式
- `checked` 是否被选中
- `value` 值
- `placeholder` 没有值时的背景内容
- `required` 必须
- `readonly` 只读
- `multiple` 允许多个值
- `min/max` 最小值/最大值

### 双标签
`<script>` 引入脚本，标签内写入 JS 代码，通常写在 body 的最后
- `type` JavaScript 使用 `text/javascript`
- `src` 外部 JS 档案，会覆盖标签内写的 JS 代码

`<span>` 文本容器 用于同一行中应用不同样式

`<b>` 加粗
`<i>` 斜体

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

`<figcaption>` 图片下方注释

`<form>` 表单
- `method` 提交方式 默认`get` 接受`post`
- `action` 提交的 URL

`select` 下拉选项框
`option` 下拉选项框的选项
- `value` 值

`textarea` 多行输入区域

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
- `hidden` 无，覆盖其它边框
- `dotted` 点画线
- `dashed` 虚线
- `solid` 实线
- `double` 双层
- `groove` 3D 沟槽
- `ridge` 3D 脊
- `inset` 3D 嵌入
- `outset` 3D 突出

`border-color` 边框[颜色](#颜色) 接受1-4个值 上右下左
`border-image` 边框图像
`border-image-slice` 九宫格分割

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
`margin` 外边距 接受1-4个值 上右下左 接受`auto`
`padding` 内边距 接受1-4个值 上右下左 接受`auto`

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

#### 术语描述列表
`<dl>` 列表
`<dt>` 术语
`<dd>` 描述

# CSS 参数

格式
```
选择器 { 参数: 值; 参数: 值; }
```
[style 样式参数](#style-样式参数)

优先级：`id 选择器 > 类选择器 > 标签选择器`

## 引入其它 CSS
`@import url('<CSS路径>');`

## 选择器

### 标签选择器
直接使用标签 `body` `h1` `p` 等作为选择器

### 类选择器
使用`.类名`（例如`.text`）作为选择器
在标签中加上`class="text"`来应用样式

### id 选择器
使用`#id名`（例如`#text`）作为选择器
标签中加上`id="text"`来应用样式

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
- `<数字>`
- `even` 偶数
- `odd` 奇数

### 属性匹配
选择器\[属性=值\]

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

`text-decoration-line` 文本装饰线
- `underline` 下划线
- `overline` 上划线
- `line-through`删除线

`text-decoration-style` 文本装饰线样式
- `dotted` 点画线
- `dashed` 虚线
- `solid` 实线
- `double` 双层
- `wavy` 波浪线

`text-decoration-color` 文本装饰线的颜色
`text-decoration-thinkness` 文本装饰线的粗细
`text-underline-offset` 文本下划线 距离

`text-indent` 段落首缩进
`line-height` 行间距
`letter-spacing` 字间距
`word-spacing` 单词间距

`opacity` 透明度
`box-shadow` 阴影
- 接收顺序 `x偏移` `y偏移` `阴影模糊半径` `阴影扩散半径` `阴影颜色`

`object-fit` 图片填充
- `none` 保持不变
- `fill` 拉伸 预设值
- `contain` 保持比例拉伸留白
- `cover` 保持比例拉伸裁切
- `scale-down` 自动选择 `none` 或 `contain` 中较小的

`object-position` 图片填充后的偏移位置
- 参数接受 `<X偏移> <Y偏移>`，0% - 100%

`border-radius` 圆角 接收上右下左四个值

`border-collapse` 是否合并相邻单元格的边框
- `seperate` 不合并
- `collapse` 合并

`display` 显示类型
- `block` 块级元素 开新行
- `inline` 行内元素 不开新行
- `inline-block` 行内块级元素 不开新行
- `flex` 块级元素 按照[弹性盒模型](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_flexible_box_layout)布局
- `inline-flex` 同上 行内元素 不开新行
- `grid` 块级元素 按照[网格模型](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_grid_layout)布局
- `inline-grid` 同上 行内元素 不开新行
- `flow-root` 块级元素 新建[区块格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_display/Block_formatting_context)

`float` 元素浮动
- `none` 预设值 无
- `left` 左
- `right` 右

`list-style-type` 列表符号样式
- `<ol>` 有序列表
  - `decimal` 数字
  - `lower-alpha` 小写英文字母
  - `upper-alpha` 大写英文字母
  - `lower-roman` 小写罗马数字
  - `upper-roman` 大写罗马数字
- `<ul>` 无序列表
  - `dise` 实心圆
  - `circle` 空心圆
  - `square` 实心矩形

`list-style-image` 列表符号图像
`list-style-position` 列表符号位置
- `outside` 在后方文本块之外
- `inside` 在后方文本块之内

`overflow` 内容溢出时行为
- `visible` 不裁剪允许出框，默认
- `hidden` 隐藏出框部分
- `clip` 同上，但完全禁用滚动
- `scroll` 始终添加滚动条
- `auto` 不溢出时与`visible`相同，溢出时与`scroll`相同
- `overlay` 同上，但滚动条处于内容上方而非空间内

`column-count` 分列，列数
`column-gap` 分列的间隔
`column-width` 列宽
`column-span` 元素横跨多少列
- `<数字>`
- `all` 横跨所有列

`column-rule` 列边框
- `column-rule-width` 宽度
- `column-rule-color` [颜色](#颜色)
- `column-rule-style` 类型
  - `none` 无
  - `hidden` 无，覆盖其它边框
  - `dotted` 点画线
  - `dashed` 虚线
  - `solid` 实线
  - `double` 双层
  - `groove` 3D 沟槽
  - `ridge` 3D 脊
  - `inset` 3D 嵌入
  - `outset` 3D 突出

### 颜色
详见 [CSS Color Module Level 3](https://www.w3.org/TR/css-color-3)

#### 渐变
线性渐变 `linear-gradient`
径向渐变 `radial-gradient`
锥形渐变 `conic-gradient`
重复线性渐变 `repeating-linear-gradient`
重复径向渐变 `repeating-radial-gradient`
重复锥形渐变 `repeating-conic-gradient`
详见 [使用 CSS 渐变](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_images/Using_CSS_gradients)
**实际返回值是图片 所以需要用于`-image`而不是`-color`的参数**

### 解法速查

#### 去除`<a>`超链接的特殊效果
```
a {
  color: inherit;
  text-decoration: none;
}
```

#### 让`<li>`里的`a`实现填充背景
```
a {
  display: block;
}
```

#### 居中元素
**（改为块然后设置左右margin为auto）**
```
<选择器> {
  display: block;
  margin: 0 auto;
}
```