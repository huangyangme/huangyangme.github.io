---
layout: post
title: "实现“三栏-中栏流动布局”[css布局]"
comments: true
description: ""
keywords: ""
---

本文介绍实现中栏流动布局的两种方法。一种是在中栏改变大小时使用负外边距定位右栏，此方法结构略显复杂，但是很巧妙并且有效。 另一种是使用 CSS3 让栏容器具有类似表格单元的行为 ，这种方法很简单易理解，但是无法在低版本IE浏览器（IE6,7）上正常使用。

## 用负外边距实现

其实现方法是添加两个div外包装，其中一个外包装包围所有三栏,另一个外包装只包围左栏和中栏。

    <div id="main_wrapper">
    <div id="threecolwrap"> /三栏外包装(包围全部三栏)/
        &lt;div id="twocolwrap"&gt; /*两栏外包装(包围左栏和中栏)*/
    
    <nav>Nav</nav>
    <article>Article</article>
      </div> /*结束两栏外包装(twocolwrap)*/
      <aside>Asides</aside> /*结束三栏外包装(threecolwrap)*/
     </div>
    </div>

**CSS：**

```css
    #main_wrapper{min-width: 600px;max-width: 1100px;margin: 0 auto}
    header{padding: 10px;background-color: #a5a5a5}
    #threecolwrap{float: left;width: 100%}
    #twocolwrap{float: left;width: 100%;margin-right: -210px} /*把右栏拉到区块外边距腾出的位置上*/
    nav{float: left;width: 150px;background-color: #f00;padding: 20px 0}    
    nav>*{margin: 0 10px} /*让子元素与栏边界保持一定距离*/    
    article{width: auto;margin-left: 150px;margin-right: 210px;/*在流动居中的栏右侧腾出空间*/background-color: #eee;padding: 20px 0}    
    article>*{margin: 0 20px} /*让子元素与栏边界保持一定距离*/    
    aside{float: left;width: 210px;background-color: #ffed53;padding: 20px 0}    
    aside>*{margin: 0 10px} /*让子元素与栏边界保持一定距离*/
```

此种方法的大致原理时：三栏中的右栏是210像素宽。为了给右栏腾出空间，中栏_article_元素有一个210像素的右外边距。当然，光有这个外边距只能把右栏再向右推210像素。别急，包围左栏和中栏的两栏外包装上210像素的负右外边距，会把右栏拉回_article_元素右外边距(在两栏外包装内部右侧)创造的空间内。中栏_aticle_元素的宽度是auto，因此它仍然会力求占据浮动左栏剩余的所有空间。可是，一方面它自己的右外边距在两栏外包装内为右栏腾出了空间，另一方面两栏外包装的负右外边距又把右栏拉到了该空间内。总之，这是个很巧妙的设计。

## 用CSS3单元格实现

CSS 可以把一个 HTML 元素的 display 属性设定为 table、table-row 和 table-cell。通过这种方法可以模拟相应 HTML 元素的行为。而通过 CSS 把布局中 的栏设定为 table-cell 有三个好处：

1.  单元格(table-cell)不需要浮动就可以并排显示，而且直接为它们应用内边距也不 3 会破坏布局

2.  默认情况下一行中的所有单元格高度相同

3.  任何没有明确设定宽度的栏都是流动的。

但是，凡是都有一个但是，CSS3的这一属性在IE7-上并不支持。然而IE6、7的使用者正在减少，相信过不了多久就再也不用考虑[IE6](http://huangyang.me/say-about-ie6.html)了。所以，如果你跟我一样鄙视这个让人碎蛋的IE6，那我非常建议你使用这一方法。只需要列出三个标记：

```html
    <nav>Nav content</nav>
    <article>Article content</article>
    <aside>Asides content</aside>
```

和以下CSS：

```css
nav{display: table-cell; width: 250px;padding: 10px;background-color: #dcd9c0}
article{display: table-cell;padding: 10px 20px;background-color: #ffed53}
aside{display: table-cell;width: 210px;padding: 10px;background-color: #3f7ccf}
```

一个三栏-中栏流动布局就这样搞定了！