---
layout: post
title: "Retina屏移动设备实现真正1px的线"
comments: true
description: ""
keywords: ""
---

##1、0.5px

iOS 8 和 OS X Yosemite 即将支持 0.5px 的边框。


<pre><code class="language-css">
/*Standard border syntax:*/
div {
    border: 1px solid black;
}
/*Retina hairline border syntax*/
@media (-webkit-min-device-pixel-ratio: 2) {
    div {
        border-width: 0.5px;
    }
}
</code></pre>

问题：包括 iOS 7 和之前版本、OS X Mavericks 和之前版本、Android 设备可能不支持0.5px。

解决方案是通过 JavaScript 检测浏览器能否处理 0.5px 的边框，如果可以，给`<html>`元素添加个class。

    if (window.devicePixelRatio && devicePixelRatio >= 2) {
      var testElem = document.createElement('div');
      testElem.style.border = '.5px solid transparent';
      document.body.appendChild(testElem);
      if (testElem.offsetHeight == 1)
      {
        document.querySelector('html').classList.add('hairlines');
      }
      document.body.removeChild(testElem);
    }
    // 脚本应该放在 <body> 内， 如果在 <head> 里面运行，需要包装 $(document).ready(function() {   })

<pre><code class="language-css">
div {
  border: 1px solid #bbb;
}
.hairlines div {
  border-width: 0.5px;
}
</code></pre>


##2、border-image

<pre><code class="language-css">
.border-1px {
        border-width: 0px 0px 1px;
        -webkit-border-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAACCAYAAACZgbYnAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDIxIDc5LjE1NTc3MiwgMjAxNC8wMS8xMy0xOTo0NDowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTQgKE1hY2ludG9zaCkiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6QkQ0RjhCQ0JDMjhBMTFFNDhBMDFBNEVGQTY4NjBERkIiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6QkQ0RjhCQ0NDMjhBMTFFNDhBMDFBNEVGQTY4NjBERkIiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo2RkJGMDBGOEMyMzQxMUU0OEEwMUE0RUZBNjg2MERGQiIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpCRDRGOEJDQUMyOEExMUU0OEEwMUE0RUZBNjg2MERGQiIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PvuzroUAAAAVSURBVHjaYjhz5sx/hv///zMABBgAIhsGYWE6RkIAAAAASUVORK5CYII=") 2 0 stretch;
    }
</code></pre>

##3、多背景渐变

设置 1px 的渐变背景，50% 有颜色，50% 透明。

<pre><code class="language-css">
.border {
    background:
    linear-gradient(180deg, black, black 50%, transparent 50%) top    left  / 100% 1px no-repeat,
    linear-gradient(90deg,  black, black 50%, transparent 50%) top    right / 1px 100% no-repeat,
    linear-gradient(0,      black, black 50%, transparent 50%) bottom right / 100% 1px no-repeat,
    linear-gradient(-90deg, black, black 50%, transparent 50%) bottom left  / 1px 100% no-repeat;
}
</code></pre>

##4、viewport + rem

在`devicePixelRatio = 2` 时，输出 viewport

    <meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">

在`devicePixelRatio = 3` 时，输出 viewport

    <meta name="viewport" content="initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no">

同时通过设置对应 viewport 的 rem 基准值，这种方式就可以像以前一样轻松愉快的写 1px 了。

(m.taobao.com 采用此方法)

##5、伪类 + transform

把原先元素的 border 去掉，然后利用 `:before` 或者 `:after` 重做 border ，并 transform 的 scale 缩小一半，原先的元素相对定位，新做的 border 绝对定位。

**单条 border**

<pre><code class="language-css">
.hairlines li{
    position: relative;
    border:none;
}
.hairlines li:after{
    content: '';
    position: absolute;
    left: 0;
    background: #000;
    width: 100%;
    height: 1px;
    -webkit-transform: scaleY(0.5);
            transform: scaleY(0.5);
    -webkit-transform-origin: 0 0;
            transform-origin: 0 0;
}
</code></pre>

**四条 border**

<pre><code class="language-css">
.hairlines li{
    position: relative;
    margin-bottom: 20px;
    border:none;
}
.hairlines li:after{
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    border: 1px solid #000;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    width: 200%;
    height: 200%;
    -webkit-transform: scale(0.5);
    transform: scale(0.5);
    -webkit-transform-origin: left top;
    transform-origin: left top;
}
</code></pre>

样式使用的时候，结合 JS 代码，判断是否 Retina 屏：

    if(window.devicePixelRatio && devicePixelRatio >= 2){
        document.querySelector('ul').className = 'hairlines';
    }








