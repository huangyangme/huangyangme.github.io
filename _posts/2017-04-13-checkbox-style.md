# CSS自定义复选框和单选按钮

有时候你会觉得默认的复选框和单选按钮看起来总是太过单调，并且不仅在不同的操作系统平台上的视觉效果不一致，就算都是在PC机上，不同的浏览器也会有自己“个性”的显示样式。如果想自定义复选框和单选按钮样式，可以通过CSS+JavaScript来实现。实现起来并不复杂。 

**结构：**

    <input type="checkbox" value="" id="check-1">
    <label for="check-1" class="">复选按钮</label>

    <input type="radio" value="" id="check" name="">
    <label for="check" class="">单选按钮</label>

**使用CSS3给其添加自定义样式：**

*wrapper*

<pre><code class="language-css">
    custom-checkbox, #custom-radio {position: relative;float: left}
</code></pre>

*input, label position*

<pre><code class="language-css">
    custom-checkbox input,#custom-radio input {
        position: absolute;
        margin:0;
        z-index: 0;
        top:3px;
        left: 1px
    }
    custom-checkbox label,#custom-radio label {
        display: block;
        position: relative;
        z-index: 1;
        width: auto !important; height: 1.8em;
        margin: 0 10px 5px 0;padding: 0 4px;
        min-width: 50px;
        font-size: 13px;
        color: #fff;
        line-height: 1.9;
        text-align: center !important;
        cursor: pointer;
        outline: none;
    }
</code></pre>

*默认状态*

<pre><code class="language-css">
    custom-checkbox label, #custom-radio label{
        color: #D54E21;
        border: 1px solid #D54E21;
        background-color:#fafafa;
        border-radius: 3px
    }
</code></pre>

*鼠标悬停和焦点状态*

<pre><code class="language-css">
    custom-checkbox label.hover, #custom-checkbox label.focus, #custom-radio label.hover,#custom-radio label.focus {
        background-color: #e4916a
    }
</code></pre>

*选中状态*

<pre><code class="language-css">
    custom-checkbox label.checked,#custom-radio label.checked {
        background-color: #D54E21;color: #fff;text-shadow: 0 -1px 0 rgba(0,0,0,0.25)
    }

    custom-checkbox label.checked:before {
        content: '\e819';font-size: 10px;font-family: "fontello";speak: none;display: inline-block;text-decoration: inherit;width: 1em;margin-right: .2em;text-align: center;font-variant: normal;text-transform: none;line-height: 1em;margin-left: .2em;
    }

    custom-checkbox label.checkedHover,#custom-checkbox label.checkedFocus {
        background-position: -10px -314px
    }

    custom-checkbox label.focus,#custom-radio label.focus {
        outline: 1px dotted #ccc
    }

    .control-group .span1 label{
        width: 80px
    }
</code></pre>

**引入jQuery和 [customInput.jquery.js](http://huangyang.me/demo/checkbox/checkbox.js) 插件：**

（略）

**最后调用插件：**

    $('input').customInput();

**[演示](http://huangyang.me/demo/checkbox/)**

这样的功能自然会有一个强大的jQuery插件来实现。iCheck就是这么一款插件。其开源项目地址：[https://github.com/fronteed/iCheck](https://github.com/fronteed/iCheck)

**插件的特色：**

*   小巧（gzip只有1kb
*   支持触摸设备和键盘导
*   表现一
*   方便定
*   最重要，支持所有浏览器（包括坑爹的IE6+）

**使用：**

假设你已经[下载](http://pan.baidu.com/share/link?shareid=2515038423&amp;uk=219570419)好了此插件：

引入jQuery、插件js、插件CSS及图片

    <link href="your-path/square/color-scheme.css" rel="stylesheet">
    <script src="your-path/jquery.js"></script>
    <script src="your-path/jquery.icheck.js"></script>

HTML

    <input type="checkbox">
    <input type="checkbox" checked>
    <input type="radio" name="iCheck">
    <input type="radio" name="iCheck" checked>

启用JS

    <script>
    $(document).ready(function(){
      $('input').iCheck({
        checkboxClass: 'icheckbox_square',
        radioClass: 'iradio_square',
        increaseArea: '20%' // optional
      });
    });
    </script>

在css文件里提供许多空间样式和颜色方案可选，你可可以自己用PS制作图片使用。

相关内容：

*   [iCheck中文介绍及下载](http://www.bootcss.com/p/icheck/)
*   [iCheck github开源下载](https://github.com/fronteed/iCheck)