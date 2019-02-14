---
layout: post
title: "Retina 屏幕下，网页图片的显示兼容"
comments: true
description: ""
keywords: ""
---

> 原文链接：http://mutian.info/tech/1386

## 1. <img> 标签引用的图片

我们以一张 300*200px 的照片为例：

`<img src="./photo.jpg" style="width:300px;height:200px;" />`

如果想让这张图片在 Retina 屏幕下达到应有的显示分辨率，只需使用该照片的源文件导出一张清晰的 600*400px 的图片，我们将其命名为 photo@2x.jpg，替换现有的图片即可：

`<img src="./photo@2x.jpg" style="width:300px;height:200px;" />`

换成 @2X 图片，在 Retina 屏幕下的显示就清晰多了，可谓细节毕现。不过在普通屏幕下，图片的显示需要经过浏览器的压缩，一些老版本浏览器如 IE6、7 会显示得非常失真，同时大尺寸的图片会占用更多的带宽，增加页面加载的时间，降低用户体验。通过 JS 的辅助，可以让图片在普通屏幕和 Retina 屏幕下做到自动适配：

```
    <img class="photo" src="./photo.jpg" style="width:300px;height:200px;" />
     
    <script type="text/javascript">
    $(document).ready(function () {
        if (window.devicePixelRatio > 1) {
            var images = $("img.photo");
            images.each(function(i) {
                var x1 = $(this).attr('src');
                var x2 = x1.replace(/(.*)(\.\w+)/, "$1@2x$2");
                $(this).attr('src', x2);
            });
        }
    });
    </script>
```


[Retina.js](http://retinajs.com/) 提供了更加完善的解决方案，自动匹配屏幕分辨率的同时，还可以检测服务器上是否存有当前图片的 @2X 版本，以决定是否替换。

优点：

1. 操作简单
2. 普通屏幕下不会加载 @2X 的大尺寸图片，节约带宽

缺点：

1. Retina 屏幕下，标准图片和 @2X 图片都会被加载
2. 图片在显示过程中会被重绘
3. 有些老版本浏览器下存在兼容问题

## 2. CSS 背景图片

### 2.1 Meta Queries + background-size

我们以一张 logo 的背景图为例，首先我们定义 logo 的尺寸为 100*40px，然后为 #logo 设定一个 100*40px 的背景图片 logo.png，

```css
#logo {
    width: 100px;
    height: 40px;
    background: url(./logo.png) 0 0 no-repeat;
}
```

接下来通过 Meta Queries 判断设备的最小显示像素比，如果大于等于1.5的话，为 #logo 设定一张 200*80px 的背景图片 logo@2x.png，然后通过设置 background-size 让背景图显示为 logo 该有的尺寸。这里的显示像素比我们选择 1.5 作为阈值，是为兼容除苹果以外的高分辨率设备，比如三星的 Android Pad。

```css
    @media only screen and (-webkit-min-device-pixel-ratio: 1.5),
           only screen and (min--moz-device-pixel-ratio: 1.5), /* 注意这里的写法比较特殊 */
           only screen and (-o-min-device-pixel-ratio: 3/2),
           only screen and (min-device-pixel-ratio: 1.5) {
        #logo {
            background-image: url(./logo@2x.png);
            background-size: 100px auto;
        }
    }
```

这样，对于普通的显示设备或是不支持 Meta Queries 的浏览器，会显示标准的背景图，对于支持 Meta Queries 的 Retina 设备，会显示 @2X 的背景图。（ IE6、7、8 均不支持 Meta Queries 和 background-size ）

如果仅是为了兼容当前的苹果 Retina 显示屏，也可以直接判断设备的显示像素比是否等于2：

```css
    @media only screen and (-webkit-device-pixel-ratio: 2),
           only screen and (-moz-device-pixel-ratio: 2),
           only screen and (-o-device-pixel-ratio: 2/1),
           only screen and (device-pixel-ratio: 2) {
        ...
    }
```

优点：

1. 只会加载匹配当前设备的最适图片
2. 跨浏览器兼容

缺点：

1. 如果背景图片很多的话，需要编写非常冗长的代码


### 2.2 image-set

我们同样以之前的 logo 为例，实现方式如下：

```css
    #logo {
        background: url(./logo.png) 0 0 no-repeat;
        background-image: -webkit-image-set(url(./logo.png) 1x, url(./logo@2x.png) 2x);
    }
```

优点：

1. 让图片的链接地址在 CSS 中更加集中，代码的维护成本降低
2. 可以让浏览器获取多种尺寸的图片文件，根据屏幕分辨率或是其他一些因素选择适当的图片进行展示，在图片的匹配上可以做到更加智能

缺点：

1. image-set 现在只是 CSS4 的一个草案，目前只有 Webkit 内核的 Safari 6+ 和 Chrome 21+ 支持该属性
最后的这条缺点很致命，期待 W3C 早日将 image-set 写入标准之中，让更多的浏览器支持这种写法。


## 3. 使用 SVG 可缩放矢量图形

与只能记录像素信息的位图相比，矢量图在不同分辨率下的兼容有着先天的优势，目前大多数现代浏览器都已经支持基于 XML 的 SVG 格式图形的显示，网页中一些线条简单的 Logo 、图标或特殊字形，如果转成矢量的 SVG 格式来显示，在 Retina 下的兼容也就搞定了。

制作 SVG 格式图片，可以使用 Adobe Illustrator 或免费的替代软件 inkscape 。

使用 SVG 格式图片，可以像我们使用其他格式的图片一样，用 HTML 的 <img> 标签引用，或用 CSS 的 background-image 、 content:url() 属性，需要注意的是，无论用哪种形式，最好定义一下图片的尺寸。


`<img src="example.svg" width="300" height="200" />`

```css
    /* Using background-image */
    .image {
        background-image: url(example.svg);
        background-size: 300px 200px;
        width: 300px;
        height: 200px;
    }
     
    /* Using content:url() */
    .image-container:before {
        content: url(example.svg);
        /* width and height do not work with content:url() */
    }
```

如果需要兼容 IE6、7、8 或是其他一些不支持 SVG 的浏览器，需要额外用到 PNG 格式的图片副本（关于 PNG 格式 Alpha 通道的兼容问题这里不做讨论）。

### 3.1 <img> 标签引用 SVG 格式图片

在 HTML 的 <img> 标签中，增加一个 PNG 格式图片的自定义属性

    <img src="example.svg" data-png-fallback="example.png" />

然后引入 jQuery 和 Modernizr 判断当前浏览器是否支持 SVG ，不支持的话使用 PNG 替换 SVG 。

```
    $(document).ready(function(){
        if(!Modernizr.svg) {
            var images = $('img[data-png-fallback]');
            images.each(function(i) {
                $(this).attr('src', $(this).data('png-fallback'));
            });
        }
    });
```

### 3.2 CSS 背景引用 SVG 格式图片

引入 Modernizr ，将 CSS 改写成以下形式即可：

```css
    .image {
        background-image: url(example.png);
        background-size: 30p0x 200px;
    }
     
    .svg {
        .image {
            background-image: url(example.svg);
        }
    }
```

为了获得最佳的跨浏览器兼容效果，避免在 Firefox 和 Opera 下出现光栅问题，制作的 SVG 图片最小要达到父容器的尺寸。

优点：

1. 可兼容所有设备的分辨率
2. 维护成本较低
3. 矢量图可以无限伸缩，更加面向未来

缺点：

1. 不适合复杂的图形，复杂的矢量图形可能会导致文件过大
2. 不同的抗锯齿算法，可能会带来不同的浏览感受
3. IE6、7、8，早期的 Android 浏览器，及其他一些较老的浏览器无法提供对 SVG 的原生支持，使用 <img> 标签的方式可能会导致浏览器下载 SVG 文件


## 4. Favicon

Favicon 的 Retina 兼容比较容易，或许你的现在 Favicon 在 Retina 下就已经显示得非常清晰，如果不是这样，使用 ico 编辑工具，创建一个包含 16*16 和 32*32 两种内建图像的 ico 文件，替换现有的 Favicon 即可，浏览器会根据分辨率的大小自动匹配内建图像的尺寸。

至于 ico 编辑工具，Windows 下推荐使用 IconXP ，Mac 下推荐 Apple’s Icon Composer（[Graphic Tools in Xcode](https://developer.apple.com/downloads/) 中）。