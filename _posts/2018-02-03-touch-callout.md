---
layout: post
title: "禁用 Safari Mobile Web 的默认交互行为"
comments: true
description: ""
keywords: ""
---


`-webkit-touch-callout: none;`

当你触摸并按住触摸目标时候，禁止或显示系统默认菜单。在iOS上，当你触摸并按住触摸的目标，比如一个链接，Safari浏览器将显示链接有关的系统默认菜单。这个属性可以让你禁用系统默认菜单。

具体查看：[http://www.css88.com/webkit/-webkit-touch-callout/](http://www.css88.com/webkit/-webkit-touch-callout/)

`-webkit-tap-highlight-color`

当用户点击iOS的Safari浏览器中的链接或JavaScript的可点击的元素时，覆盖显示的高亮颜色。

该属性可以只设置透明度。如果未设置透明度，iOS Safari使用默认的透明度。当透明度设为0，则会禁用此属性；当透明度设为1，元素在点击时不可见。

具体查看：[http://www.css88.com/webkit/-webkit-tap-highlight-color/](http://www.css88.com/webkit/-webkit-tap-highlight-color/)


当你touch和hold一个触控对象时，例如链接，Safari会显示一个包含链接信息的弹出框。该属性允许你来禁用这个弹出框。 

`-webkit-user-select: none;`

可选值： 

- `auto`: 用户可以选择元素内的内容
- `none`: 用户不能选择任何内容
- `text`: 用户只能选择元素内的文本



**禁用整个页面的用户选择和链接弹出框，可页面样式表中添加如下样式规则**

```css
    body
    {
        -webkit-touch-callout:none ;
        -webkit-user-select:none ;
    }
```

**只允许Form表单域执行文本的剪切板操作，添加如下规则**

```css
    *:not(input,textarea) {
        -webkit-touch-callout: none;
        -webkit-user-select: none; 
    }
```

**禁用某个链接的长按弹出框,可在链接添加内联样式规则如下**

`<a href="#" style="-webkit-touch-callout:none">`

**以编程方式动态的向加载页面添加样式来达到同样的效果**

实现UIWebviewDelegate协议，在webViewDidFinishLoad:方法中添加以下代码 

```
    - (void)webViewDidFinishLoad:(UIWebView *)webView {
       // 禁用用户选择
       [webView stringByEvaluatingJavaScriptFromString:@"document.documentElement.style.webkitUserSelect='none';"];
        
       // 禁用长按弹出框
       [webView stringByEvaluatingJavaScriptFromString:@"document.documentElement.style.webkitTouchCallout='none';"];
    }
```


关键词：禁止选择,禁止选中,禁止复制