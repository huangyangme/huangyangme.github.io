# css实现文本竖排 - writing-mode

**语法**

    /*
    语法：writing-mode : lr-tb、tb-rl
    参数：lr-tb：从左向右，从上往下　tb-rl：从上往下，从右向左
    */

在最新的规范中「writing-mode」属性缩减为三个值：

- horizontal-tb（默认值）：自上而下，从左到右的横排书写方式。（类似IE私有值lr-tb）
- vertical-rl：从右到左，自上而下的竖排书写方式（典型的汉字竖排排版方式）。（类似IE私有值tb-rl）
- vertical-lr：从左到右，自上而下的竖排书写方式（主要用于内蒙古的蒙古语和满语。）。

**示例**

    p {-webkit-writing-mode: horizontal-tb | vertical-rl | vertical-lr}


**查看**

[http://ued.ctrip.com/blog/wp-content/webkitcss/prop/writing-mode.html](http://ued.ctrip.com/blog/wp-content/webkitcss/prop/writing-mode.html)

**浏览器支持**

IE6-10，Opera 不支持标准属性：horizontal-tb | vertical-rl | vertical-lr
IE6只支持 tb-rl | lr-tb，IE7 只支持 tb-rl | lr-tb | rl-tb | bt-rl
IE11.0.9431.0 开始支持该属性
Webkit 目前实现的最好（需加-webkit-），为了兼容旧的规范，虽然支持 lr-tb | tb-rl 属性值，但未实现效果
Opera 12.1 虽然支持 lr-tb | tb-rl 属性值，但未实现效果
Firefox 不支持 writing-mode 属性