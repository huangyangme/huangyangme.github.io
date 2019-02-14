---
layout: post
title: "鼠标左右键控制翻页"
comments: true
description: ""
keywords: "js"
---

```javascript
    <script>
        document.onkeydown=nextpage;
        var prevpage="preurl";//上一页链接
        var nextpage="nexturl";//下一页链接
        var catalog="catalogurl";//目录页链接
        function nextpage(event) {
        event = event ? event : (window.event ?             window.event : null);
        if (event.keyCode==39) {
        location=nextpage
        }
        if (event.keyCode==37) {
        location=prevpage;
        }
        if (event.keyCode==13) {
        location=catalog
        }
        }
    </script>
```