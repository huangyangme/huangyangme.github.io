---
layout: post
title: "Javascript 选择器"
comments: true
description: ""
keywords: "js"
---

`querySelector` 选择的是 `querySelectorAll` 的第一个：

    document.querySelector('.foo') == document.querySelectorAll('.foo')[0];

**class（类）选择器**

    document.getElementsByClassName('foo');

**tag（标签）选择器**

    document.getElementsByTagName('a');

**ID 选择器**

    document.getElementById('foo');

---

## DOM 遍历操作

**父节点**

`el.parentNode`

    var el = document.querySelector('div');
    var parent = el.parentNode;

**子节点**

`el.childNodes;`  //所有子节点
`el.firstChild;` Or `el.children[0]` //第一个子节点
`el.lastChild;` Or `el.lastElementChild` //最后一个子节点

**兄弟节点**

`el.parentNode`

`el.nextSibling`

`el.previousSibling`

**get all siblings**

    function getAllSiblings(el, filter) {
        var siblings = [];
        el = el.parentNode.firstChild;
        do {
            if (!filter || filter(el)) siblings.push(el);
        } while (el = el.nextSibling)
        return siblings;
    }


**get all previous siblings**

    function getPreviousSiblings(el, filter) {
        var siblings = [];
        while (el = el.previousSibling) { if (!filter || filter(el)) siblings.push(el); }
        return siblings;
    }


**get all next siblings**

    function getNextSiblings(el, filter) {
        var siblings = [];
        while (el= el.nextSibling) { if (!filter || filter(el)) siblings.push(el); }
        return siblings;
    }

---

## 操作DOM元素

**创建新元素**

`createElement()`

    var el = document.createElement('div');
    el.innerHTML = '<p>Hello World!</p>';


**替换元素**

`replaceChild()`

    // select the element that will be replaced
    var el = document.querySelector('div');
    // <a href="/javascript/manipulation/creating-a-dom-element-51/">create a new element</a> that will take the place of "el"
    var newEl = document.createElement('p');
    newEl.innerHTML = '<b>Hello World!</b>';
    // replace el with newEL
    el.parentNode.replaceChild(newEl, el);

**清空元素内容**

    el.innerHTML = '';

**清除一个元素**

    var el = document.querySelector('div');
    el.parentNode.removeChild(el);

**在元素之前或之后插入一个新元素**

*在之后插入*

    function insertAfter(el, referenceNode) {
        referenceNode.parentNode.insertBefore(el, referenceNode.nextSibling);
    }
    
    // example
    var newEl = document.createElement('div');
    newEl.innerHTML = '<p>Hello World!</p>';
    
    var ref = document.querySelector('div.before');
    
    insertAfter(newEl, ref);

*在之前插入*

    function insertBefore(el, referenceNode) {
        referenceNode.parentNode.insertBefore(el, referenceNode);
    }
    
    // example
    var newEl = document.createElement('div');
    newEl.innerHTML = '<p>Hello World!</p>';
    
    var ref = document.querySelector('div.before');
    
    insertBefore(newEl, ref);

**获取元素的内容(text)**

`el.textContent` (IE8: `el.innerText`)

    var el = document.querySelector('div');
    text = el.textContent || el.innerText;
    console.log(text);

**获取或修改整个元素的 html**

`el.innerHTML`

    var el = document.querySelector('div');
    
    // get HTML content
    console.log(el.innerHTML);
    // set HTML content
    el.innerHTML = '<p>Hello World!</p>'

**在元素内部的前面或后面插入新元素**

    var el = document.querySelector('div');
    
    // get element content as string
    console.log(el.innerHTML)
    // append to the element's content
    el.innerHTML += '<p>Hello World!</p>'; 
    // prepend to the element's content
    el.innerHTML = '<p>Hello World!</p>' + el.innerHTML;

**在元素外层包上一个新元素作为他的父元素**

    function wrap(el, wrapper) {
        el.parentNode.insertBefore(wrapper, el);
        wrapper.appendChild(el);
    }
    
    // example: wrapping an anchor with class "wrap_me" into a new div element
    wrap(document.querySelector('a.wrap_me'), document.createElement('div'));

**复制一个已存在的元素**

'el.cloneNode(true);'

    var el = document.querySelector('div');
    var foo = el.cloneNode(true);

---


## 元素属性

**属性操作 set, get, remove**

    el.attr = 'val'
    var el = document.querySelector('a');
    el.title = 'foo';
    
    var inp = document.querySelector('input[type="text"]');
    inp.value = 'Hello World!';
    delete inp.value;


**类操作 addClass, removeClass, hasClass**

    el.className = 'hasJS';
    el.className += ' hasJS';

函数：

    function hasClass(el, className) {
        return el.classList ? el.classList.contains(className) : new RegExp('\\b'+ className+'\\b').test(el.className);
    }

    function addClass(el, className) {
        if (el.classList) el.classList.add(className);
        else if (!hasClass(el, className)) el.className += ' ' + className;
    }

    function removeClass(el, className) {
        if (el.classList) el.classList.remove(className);
        else el.className = el.className.replace(new RegExp('\\b'+ className+'\\b', 'g'), '');
    }

使用：

    var el = document.querySelector('div');
    if (!hasClass(el, 'foo')) addClass(el, 'foo');

**设置、获取和删除属性**

`setAttribute('attr', 'val')`, `getAttribute('attr')`, `removeAttribute('attr')`

    // set the alt attribute of an element
    var el = document.querySelector('img');
    el.setAttribute('alt', 'Hello World!');
    
    // read the title of an element
    var el = document.querySelector('img');
    console.log(el.getAttribute('title'));
    
    // remove the alt attribute of an element
    el.removeAttribute('alt');

**HTML5 的 classList API**

    if ("classList" in document.documentElement) { //IE10+
        // Adding a class
        element.classList.add("bar");
        // Removing a class
        element.classList.remove("foo");
        // Checking if has a class
        element.classList.contains("foo");
        // Toggle a class
        element.classList.toggle("active");
    }

---

## Style

[http://plainjs.com/javascript/styles/](http://plainjs.com/javascript/styles/)


--- 

## Ajax

###Ajax

    var xmlhttp=new XMLHttpRequest();
    xmlhttp.open(method,url,async); //规定请求的类型、URL 以及是否异步处理请求。
    setRequestHeader(header,value); //添加 HTTP 头
    xmlhttp.send(string); //string：仅用于 POST 请求
    
    var xmlhttp=new XMLHttpRequest();
    xmlhttp.onreadystatechange=function() {
        if (xmlhttp.readyState==4 && xmlhttp.status==200) { //当 readyState 等于 4 且状态为 200 时，表示响应已就绪
            console.log(xmlhttp.responseText); //responseText: 获得字符串形式的响应数据。
        }
    }
    xmlhttp.open("POST","ajax_test.json",true);
    xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
    xmlhttp.send("fname=Bill&lname=Gates");


###Ajax GET

    function getAjax(url, success) {
        var xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP');
        xhr.open('GET', url);
        xhr.onreadystatechange = function() {
            // if (xhr.readyState>3 && xhr.status==200) success(xhr.responseText);
            if (this.readyState === 4){
                if (this.status >= 200 && this.status < 400){
                  // Success!
                  resp = this.responseText;
                } else {
                  // Error :(
            }
        };
        xhr.setRequestHeader('X-Requested-With', 'XMLHttpRequest');
        xhr.send();
        return xhr;
    }
    
    // example request
    getAjax('http://foo.bar/?p1=1&p2=Hello+World', function(data){
        console.log(data);
    });
    //Or get JSON
    getAjax('http://foo.bar/?p1=1&p2=Hello+World', function(data){
        var json = JSON.parse(data);
        console.log(json); 
    });


###Ajax POST

    function postAjax(url, data, success) {
        var params = typeof data == 'string' ? data : Object.keys(data).map(
                function(k){ return encodeURIComponent(k) + '=' + encodeURIComponent(data[k]) }
            ).join('&');
    
        var xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP");
        xhr.open('POST', url);
        xhr.onreadystatechange = function() {
            if (xhr.readyState>3 && xhr.status==200) { success(xhr.responseText); }
        };
        xhr.setRequestHeader('X-Requested-With', 'XMLHttpRequest');
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xhr.send(params);
        return xhr;
    }
    
    // example request
    postAjax('http://foo.bar/', 'p1=1&p2=Hello+World', function(data){ console.log(data); });
    
    // example request with data object
    postAjax('http://foo.bar/', { p1: 1, p2: 'Hello World' }, function(data){ console.log(data); });

###Get JSON

    request = new XMLHttpRequest();
    request.open('GET', 'URL', true);
     
    request.onreadystatechange = function() {
      if (this.readyState === 4){
        if (this.status >= 200 && this.status < 400){
          // Success!
          data = JSON.parse(this.responseText);
        } else {
          // Error :(
        }
      }
    }
     
    request.send();
    request = null;

###使用一个表单元素初始化FormData对象

    window.addEventListener("load", function () {
      function sendData() {
        var XHR = new XMLHttpRequest();
    
        // We bind the FormData object and the form element
        var FD  = new FormData(form);
    
        // We define what will happen if the data are successfully sent
        XHR.addEventListener("load", function(event) {
          alert(event.target.responseText);
        });
    
        // We define what will happen if case of error
        XHR.addEventListener("error", function(event) {
          alert('Oups! Something goes wrong.');
        });
    
        // We setup our request
        XHR.open("POST", "http://ucommbieber.unl.edu/CORS/cors.php");
    
        // The data send are the one the user provide in the form
        XHR.send(FD);
      }
     
      // We need to access the form element
      var form = document.getElementById("myForm");
    
      // to takeover its submit event.
      form.addEventListener("submit", function (event) {
        event.preventDefault();
    
        sendData();
      });
    });
    
    <form id="myForm">
      <label for="myName">Send me your name:</label>
      <input id="myName" name="name" value="John">
      <input type="submit" value="Send Me!">
    </form>

###异步加载js

    var script = document.createElement('script'),
        scripts = document.getElementsByTagName('script')[0];
    script.src = url;
    scripts.parentNode.insertBefore(script, scripts);
    
    <script src="https://platform.twitter.com/widgets.js" async defer></script>


--- 


## Javascript 事件

###Document Ready 事件，DOM加载完毕后执行

    document.addEventListener('DOMContentLoaded', function(){
        // do something
    });

兼容IE8及以下：

    function run() {
        // do something
    }
    
    // in case the document is already rendered
    if (document.readyState!='loading') run();
    // modern browsers
    else if (document.addEventListener) document.addEventListener('DOMContentLoaded', run);
    // IE <= 8
    else document.attachEvent('onreadystatechange', function(){
        if (document.readyState=='complete') run();
    });

###事件监听器

    element.addEventListener("click", function() {
      alert("You clicked");
    }, false);

为了让页面的所有元素都实现这个功能，我们必须依次重复每个元素且给他们添加事件监听器：

    // Select all links
    var links = document.querySelectorAll("a");
    
    // For each link element
    [].forEach.call(links, function(el) {
    
      // Add event listener
      el.addEventListener("click", function(event) {
        event.preventDefault();
        alert("You clicked");
      }, false);
    
    });