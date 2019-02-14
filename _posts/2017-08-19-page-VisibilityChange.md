---
layout: post
title: "使用页面可见性API"
comments: true
description: ""
keywords: ""
---

```javascript
    //startSimulation和pauseSimulation函数定义在其他地方
    function handleVisibilityChange() {
        if (document.hidden) {
            pauseSimulation();
        } else  {
           startSimulation();
        }
    }
    document.addEventListener("visibilitychange", handleVisibilityChange, false);
```

该API一个很好的用处就是能够在页面切换到不可见状态时暂停执行一些不必要的操作,以减少资源的浪费.

在一个多标签的浏览器中,某个网页所在的标签页很有可能被切换到后台,这时,该网页是用户不可见的. 一些网站很有可能希望在此时做出一些动作. 比如:

- 某网站有个图像幻灯片页面,自动播放图片.如果该页面被切换到了不可见状态,图片的播放操作应该暂停,直到该页面重新对用户可见时,幻灯片才会继续自动播放.
- 某web应用程序每隔一段时间会自动访问服务器更新页面内的及时信息.在页面不可见时,应该停止这种请求动作.
- 某页面想要检测自己是否被预渲染,这样可以获得更准确的页面访问量.