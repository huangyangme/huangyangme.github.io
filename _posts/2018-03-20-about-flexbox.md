---
layout: post
title: "最直观的方式学习flexbox属性"
comments: true
keywords: "flex, flexbox"
---

## BASICS
在我们开始之前先来定下规则，我们把父容器称为flex container，它的直接子元素称为flex items。

![](/images/2018-10/2018-10-18-14-17-21.jpg)

上面的盒子中，你可以看到用来描述flex container 和 它的子元素的属性与术语。如果你需要查看更多浏览器兼容性，你可以看[这里](http://caniuse.com/flexbox)。

## USAGE

使用flexbox布局，你需要在父元素上设置display属性

```CSS
.flex-container {
  display: flex;
}
```
或者你可以把它当行内元素使用

```CSS
.flex-container {
  display: inline-flex;
}
```

注意：你给父容器设置了这个属性后，它的子元素都会自动变成flex items。

有很多种给flexbox属性分组的方式，到目前为止，我认为最简单，并且最容易理解的方式是按照flex container和flex items分成两组。下面，我们来解释各个元素是如何影响布局效果的。

### Flexbox Container 属性

#### flex-direction
这个属性指定了flex items在flex container中是如何布局的。通过设置flex container的主轴的方向，它们会按照两个方向布局，水平的行或者垂直的列。

例子：
```css
.flex-container {
  flex-direction:         row;
}
```
设置为行，那么在ltr上下文环境下，所有flex items会按照从左到右的顺序排成一行。

![](/images/2018-10/2018-10-18-14-20-40.jpg)

```css
.flex-container {
  flex-direction:         row-reverse;
}
```

使用`row-reverse`属性，那么在ltr上下文环境下，子元素则会按照从右到左的顺序排成一行。

![](/images/2018-10/2018-10-18-14-21-25.jpg)

```css
.flex-container {
  flex-direction:         column;
}
```

使用`column`属性，flex items会按照从上到下的方式排列。

![](/images/2018-10/2018-10-18-14-21-57.jpg)

```css
.flex-container {
  flex-direction:         column-reverse;
}
```
使用`column-reverse`，则会放过来。

![](/images/2018-10/2018-10-18-14-22-32.jpg)

#### flex-wrap

默认的flexbox概念是把所有子元素都放在一行里面，你可以通过flex-wrap属性来控制flex container是否将子元素分多行处理，以及新增行的方向。

例子：

```css
.flex-container {
  flex-wrap:         nowrap;
}
```

Flex items会被置在一行里面，并且默认它们会被压缩来适应容器的宽度。

![](/images/2018-10/2018-10-18-14-23-19.jpg)

```css
.flex-container {
  flex-wrap: wrap;
}
```
wrap, Flex items会被按照从上到下从左到右的顺序分配到多行。

![](/images/2018-10/2018-10-18-14-23-55.jpg)

```css
.flex-container {
  flex-wrap: wrap-reverse;
}
```

wrap-reverse, Flex itms会被按照从左到右从下到上的顺序在多行中显示。
![](/images/2018-10/2018-10-18-14-24-29.jpg)


#### flex-flow

这个属性是flex-direction和flex-wrap属性的缩写。
例子：

```css
.flex-container {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

默认值：row nowrap

#### justify-content

这个属性会根据当前容器的主轴来排列子元素。它可以在所有flex items都在同一行并且不可伸缩，或是可伸缩但是达到它们的最大尺寸时候，分配剩余空间。

例子：

```css
.flex-container {
  justify-content: flex-start;
}
```

Flex items在`lrt`上下文中会向左边靠拢

![](/images/2018-10/2018-10-18-14-25-37.jpg)

```css
.flex-container {
  justify-content: flex-end;
}
```
Flex items在ltr上下文中向右靠拢

![](/images/2018-10/2018-10-18-14-26-08.jpg)

```css
.flex-container {
  justify-content: center;
}
```
Flex items会居中

![](/images/2018-10/2018-10-18-14-26-37.jpg)

```css
.flex-container {
  justify-content: space-between;
}
```

除了第一个和最后一个，Flex items会有相同的间隔。

![](/images/2018-10/2018-10-18-14-27-04.jpg)

```css
.flex-container {
  justify-content: space-around;
}
```

所有Flex item 都会有相同间隔

![](/images/2018-10/2018-10-18-14-27-31.jpg)

默认值： flex-start

#### align-items

Flex items 会根据当前容器的主轴线对齐，跟justify-content很相似，但是方向垂直。
这个属性设置所有flex items默认对齐方式。
例子：

```css
.flex-container {
  align-items: stretch;
}
```
Flex items会占满flex container的高度，从cross start到cross end
![](/images/2018-10/2018-10-18-14-28-14.jpg)

```css
.flex-container {
  align-items: flex-start;
}
```
Flex items会与flex container 的交叉轴起始(cross start)线对齐。

![](/images/2018-10/2018-10-18-14-28-37.jpg)

```css
.flex-container {
  align-items: flex-end;
}
```

Flex items会与flex container 的交叉轴结尾（cross end）线对齐。

![](/images/2018-10/2018-10-18-14-29-04.jpg)

```css
.flex-container {
  align-items: center;
}
```
Flex items会在交叉轴（cross axis）上对齐

![](/images/2018-10/2018-10-18-14-29-33.jpg)

```css
.flex-container {
  align-items: baseline;
}
```

Flex items会按照它们的基线（baselines）对齐

![](/images/2018-10/2018-10-18-14-29-58.jpg)

默认值：stretch

#### align-content
align-content属性对齐flex container上的行，控制交叉轴的多余间隔，跟主轴上的justify-content属性很相似。

例子：

```css
.flex-container {
  align-content:         stretch;
}
```
每一行Flex items后面都会有区分开的间隔。

![](/images/2018-10/2018-10-18-14-30-40.jpg)


```css
.flex-container {
  align-content: flex-start;
}
```
Flex items会向交叉轴起始位置靠拢

![](/images/2018-10/2018-10-18-14-31-59.jpg)


```css
.flex-container {
  align-content: flex-end;
}
```

Flex items 会向交叉轴的结束位置靠拢


![](/images/2018-10/2018-10-18-14-32-25.jpg)

```css
.flex-container {
  align-content: center;
}
```

flex items的行会在交叉轴上居中显示

![](/images/2018-10/2018-10-18-14-32-58.jpg)

```css
.flex-container {
  align-content: space-between;
}
```

除了起始和结尾行，flex items的其他行都会有相同的间隔。

![](/images/2018-10/2018-10-18-14-33-23.jpg)


```css
.flex-container {
  align-content: space-around;
}
```

每一行都会有相同上下间隔

![](/images/2018-10/2018-10-18-14-34-38.jpg)


默认值：stretch
注：这个属性只会在flex container有多行的时候才会有效，如果只有一行那么这个属性就不会有效果。

#### 关于flex containers
所有的column-*属性在flex container上都不会有效果
::first-line 与 ::first-letter伪类在flex container上不会被应用。

### FlexBox Item 属性

#### Order

order属性定义子元素在父容器中的顺序，默认它们会被最加到后面
例子：

```css
.flex-item {
  order: <integer>;
}
```

不需要调整HTML，就能修改显示顺序

![](/images/2018-10/2018-10-18-14-40-02.jpg)

默认值:0

#### flex-grow
这个属性是一个flex拉伸因子，决定了flex items会相对于父容器剩余空间增长多少。

```css
.flex-item {
  flex-grow: <number>;
}
```

如果所有flex items都有相同的flex-grow，那么他们都会在父容器中有相同的尺寸。

![](/images/2018-10/2018-10-18-14-42-24.jpg)

调整一下，看看有什么区别

![](/images/2018-10/2018-10-18-14-42-41.jpg)

默认值：0
注：负数是不合法的

#### flex-shrink
flex-shrink是一个flex收缩因子，它决定了当父容器体积不足时，flex items收缩的相对比例。

```css
.flex-item {
  flex-shrink:         <number>;
}
```

默认所有flex items都是可以压缩的，但如果我们把它设为0，那么它将保持原有大小。

![](/images/2018-10/2018-10-18-14-43-11.jpg)


#### flex-basis
这个属性会设置宽度和高度，
原文为：This property takes the same values as the width and height properties
并在flex因子分割间隔之前，指定flex items的初始大小。

```css
.flex-item {
  flex-basis: auto | <width>;
}
```

flex-basis会指定第四个flex items的初始大小

![](/images/2018-10/2018-10-18-14-45-50.jpg)

#### flex
这个属性是flex-grow,flex-shrink和flex-basis属性的缩写。

```css
.flex-item {
  flex: none | auto | [ <flex-grow> <flex-shrink>? || <flex-basis> ];
}
```

#### align-self
这个属性允许为单个flex items重写对齐方式。

```css
.flex-item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

![](/images/2018-10/2018-10-18-14-46-54.jpg)


>  [原文链接](https://www.w3ctrain.com/2015/11/12/visual-guide-to-css3-flexbox-flexbox-playground/)