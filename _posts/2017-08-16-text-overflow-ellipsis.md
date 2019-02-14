# 多行文本溢出显示省略号(…)


单行文本的溢出显示省略号(…): `text-overflow:ellipsis`

<pre><code class="language-css">
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
</code></pre>

但是这个属性并不支持多行文本溢出显示省略号，这里根据应用场景介绍几个方法来实现这样的效果。

## CSS3 方案

`-webkit-line-clamp` 用来限制在一个块元素显示的文本的行数。 为了实现该效果，它需要组合其他的 WebKit 属性。

常见结合属性：

`display: -webkit-box;` 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
`-webkit-box-orient` 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。
`text-overflow: ellipsis`;，可以用来多行文本的情况下，用省略号“…”隐藏超出范围的文本 。

<pre><code class="language-css">
    overflow : hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
</code></pre>

此属性比较合适 WebKit 浏览器或移动端（绝大部分是 WebKit 内核的）浏览器 [Demo](http://www.css88.com/webkit/-webkit-line-clamp/)。

## 图片兼容方案

另一种做法就是设置相对定位的容器高度，用包含省略号(…)的元素（背景图片等）模拟实现；

<pre><code class="language-css">
    p {
        position:relative;
        line-height:1.4em;
        /* 3 times the line-height to show 3 lines */
        height:4.2em;
        overflow:hidden;
    }
    p::after {
        content:"...";
        font-weight:bold;
        position:absolute;
        bottom:0;
        right:0;
        padding:0 20px 1px 45px;
        background:url(ellipsis_bg.png) repeat-y;
    }
</code></pre>

这里注意几点：

1. height高度真好是line-height的3倍；
2. 结束的省略好用了半透明的png做了减淡的效果，或者设置背景颜色；
3. IE6-7不显示content内容，所以要兼容IE6-7可以是在内容中加入一个标签，比如用`<span class="line-clamp">...</span>`去模拟；
4. 要支持IE8，需要将::after替换成:after；

## JavaScript 方案

用js也可以根据上面的思路去模拟，实现也很简单，推荐几个做类似工作的成熟小工具：

### Clamp.js

下载及文档地址：https://github.com/josephschmitt/Clamp.js

使用也非常简单：

    var module = document.getElementById("clamp-this-module");
    $clamp(module, {clamp: 3});

### jQuery插件-jQuery.dotdotdot

这个使用起来也很方便：

    $(document).ready(function() {
        $("#wrapper").dotdotdot({
            //  configuration goes here
        });
    });