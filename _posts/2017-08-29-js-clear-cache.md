---
layout: post
title: "js清除浏览器缓存的几种方法"
comments: true
description: ""
keywords: "js"
---

##meta方法

    //不缓存
    <META HTTP-EQUIV="pragma" CONTENT="no-cache"> 
    <META HTTP-EQUIV="Cache-Control" CONTENT="no-cache, must-revalidate"> 
    <META HTTP-EQUIV="expires" CONTENT="0">

清理form表单的临时缓存（一般情况不建议清理）

    <body onLoad="javascript:document.yourFormName.reset()">

## jQuery ajax清除浏览器缓存

方式一：用ajax请求服务器最新文件，并加上请求头If-Modified-Since和Cache-Control,如下:

```
      $.ajax({
         url:'www.haorooms.com',
         dataType:'json',
         data:{},
         beforeSend :function(xmlHttp){ 
            xmlHttp.setRequestHeader("If-Modified-Since","0"); 
            xmlHttp.setRequestHeader("Cache-Control","no-cache");
         },
         success:function(response){
             //操作
         }
         async:false
      });
```

方法二，直接用cache:false,

```
      $.ajax({
         url:'www.haorooms.com',
         dataType:'json',
         data:{},
         cache:false, 
         ifModified :true ,
         success:function(response){
             //操作
         }
         async:false
      });
```

方法三：用随机数，随机数也是避免缓存的一种很不错的方法！

    URL 参数后加上 "?ran=" + Math.random();
方法四：用随机时间，和随机数一样。

    在 URL 参数后加上 "?timestamp=" + new Date().getTime(); 
