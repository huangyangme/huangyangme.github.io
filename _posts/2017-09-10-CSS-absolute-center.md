---
layout: post
title: "CSS 绝对定位居中定位"
comments: true
description: ""
keywords: "css"
---

```css
.Absolute-Center {
  margin: auto;
  position: absolute;
  top: 0; left: 0; bottom: 0; right: 0;
}
```css

**优点：**

1. 支持跨浏览器，包括IE8-IE10.
2. 无需其他特殊标记，CSS代码量少
3. 支持百分比%属性值和min-/max-属性
4. 只用这一个类可实现任何内容块居中
5. 不论是否设置padding都可居中（在不使用box-sizing属性的前提下）
6. 内容块可以被重绘。
7. 完美支持图片居中。

**缺点：**

1. 必须声明高度（查看可变高度Variable Height）。
2. 建议设置overflow:auto来防止内容越界溢出。（查看溢出Overflow）。
3. 在Windows Phone设备上不起作用。

**浏览器兼容性：**

Chrome,Firefox, Safari, Mobile Safari, IE8-10.

> 详细：[http://www.tuicool.com/articles/vY3UVfE](http://www.tuicool.com/articles/vY3UVfE)


##使用 Flexbox 的居中布局

```css
.vertical-container {
  height: 300px;
  display: -webkit-flex;
  display: flex;
  -webkit-align-items: center;
  align-items: center;
  -webkit-justify-content: center;
  justify-content: center;
}
```