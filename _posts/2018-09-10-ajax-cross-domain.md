---
layout: post
title: "跨域"
comments: true
description: ""
keywords: ""
---

jQuery对于Ajax的跨域请求有两类解决方案，不过都是只支持get方式。分别是JQuery的 jquery.ajax jsonp格式和jquery.getScript方式。

什么是jsonp格式呢？API原文：如果获取的数据文件存放在远程服务器上（域名不同，也就是跨域获取数据），则需要使用jsonp类型。使用这种类型的话，会创建一个查询字符串参数 callback=? ，这个参数会加在请求的URL后面。服务器端应当在JSON数据前加上回调函数名，以便完成一个有效的JSONP请求。意思就是远程服务端需要对返回的数据做下处理，根据客户端提交的callback的参数，返回一个callback(json)的数据，而客户端将会用script的方式处理返回数据，来对json数据做处理。JQuery.getJSON也同样支持jsonp的数据方式调用。

客户端JQuery.ajax的调用代码示例：

```javascript
    $.ajax({
        type : "get",
        async:false,
        url : "http://www.xxx.com/ajax.do",
        dataType : "jsonp",
        jsonp: "callbackparam",//服务端用于接收callback调用的function名的参数
        jsonpCallback:"success_jsonpCallback",//callback的function名称
        success : function(json){
            alert(json);
            alert(json[0].name);
        },
        error:function(){
            alert('fail');
        }
    });
```

服务端返回数据的示例代码：

```
    public void ProcessRequest (HttpContext context) {
        context.Response.ContentType = "text/plain";
        String callbackFunName = context.Request["callbackparam"];
        context.Response.Write(callbackFunName + "([ { name:\"John\"}])");
    }
```

---

## JSONP跨域的原理

在同源策略下，在某个服务器下的页面是无法获取到该服务器以外的数据的，但img、iframe、script等标签是个例外，这些标签可以通过src属性请求到其他服务器上的数据。利用script标签的开放策略，我们可以实现跨域请求数据，当然，也需要服务端的配合。当我们正常地请求一个JSON数据的时候，服务端返回的是一串JSON类型的数据，而我们使用JSONP模式来请求数据的时候，服务端返回的是一段可执行的JavaScript代码。

举个例子，假如需要从服务器（http://www.a.com/user?id=123）获取的数据如下：

    `{"id": 123, "name" : 张三, "age": 17}`

那么，使用JSONP方式请求（http://www.a.com/user?id=123?callback=foo）的数据将会是如下：

    `foo({"id": 123, "name" : 张三, "age": 17});`

如果服务端考虑得更加充分，返回的数据可能如下：

    `try{foo({"id": 123, "name" : 张三, "age": 17});}catch(e){}`

这时候我们只要定义一个foo()函数，并动态地创建一个script标签，使其的src属性为http://www.a.com/user?id=123?callback=foo，便可以使用foo函数来调用返回的数据了。 

在jQuery中如何通过JSONP来跨域获取数据

第一种方法是在ajax函数中设置dataType为'jsonp'： 

```
    $.ajax({
            dataType: 'jsonp',
            url: 'http://www.a.com/user?id=123',
            success: function(data){
                    //处理data数据
            }
    });
```

第二种方法是利用getJSON来实现，只要在地址中加上callback=?参数即可： 
   
```
    $.getJSON('http://www.a.com/user?id=123&callback=?', function(data){
            //处理data数据
    });
```

也可以简单地使用getScript方法：
   
```
    //此时也可以在函数外定义foo方法
    function foo(data){
            //处理data数据
    }
    $.getJSON('http://www.a.com/user?id=123&callback=foo');
```


---

```
    function jsonp(url, callback) {
        var callbackName = 'jsonp_callback_' + Math.round(100000 * Math.random());
        window[callbackName] = function(data) {
            delete window[callbackName];
            document.body.removeChild(script);
            callback(data);
        };
    
        var script = document.createElement('script');
        script.src = url + (url.indexOf('?') >= 0 ? '&' : '?') + 'callback=' + callbackName;
        document.body.appendChild(script);
    }
    
    jsonp('https://api.meetup.com/2/open_events.json?zip=12233&page=30&category=34&time=,1w&key=1719487a4a3c39b3e241e181837529', function(data) {
        console.log(data);
    });
```