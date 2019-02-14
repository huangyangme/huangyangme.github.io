#js延迟加载



## defer 和 async

**defer**

对于defer，我们可以认为是将外链的js放在了页面底部。js的加载不会阻塞页面的渲染和资源的加载。不过defer会按照原本的js的顺序执行，所以如果前后有依赖关系的js可以放心使用。只有 Internet Explorer 支持 defer 属性。

**async**

    <script async="async">

对于async，这个是html5中新增的属性，它的作用是能够异步的加载和执行脚本，不因为加载脚本而阻塞页面的加载。一旦加载到就会立刻执行。async 属性仅适用于外部脚本（只有在使用 src 属性时）。

- 如果 async="async"：脚本相对于页面的其余部分异步地执行（当页面继续进行解析时，脚本将被执行）
- 如果不使用 async 且 defer="defer"：脚本将在页面完成解析时执行
- 如果既不使用 async 也不使用 defer：在浏览器继续解析页面之前，立即读取并执行脚本


## Google推荐的代码。

这些代码应被放置在</body>标签前(接近HTML文件底部)。另外，我将外部JS文件名突出显示。

    <script type="text/javascript">
        // Add a script element as a child of the body
        function downloadJSAtOnload() {
            var element = document.createElement("script");
            element.src = "defer.js";
            document.body.appendChild(element);
        }
    
        // Check for browser support of event handling capability
        if (window.addEventListener)
            window.addEventListener("load", downloadJSAtOnload, false);
        else if (window.attachEvent)
                window.attachEvent("onload", downloadJSAtOnload);
        else window.onload = downloadJSAtOnload;
    </script>

这段代码意思是等到整个文档加载完后，再加载外部文件“defer.js”。


## 代码延迟加载

    window.onload = function() {
        setTimeout(function(){
        
            // reference to <head>
            var head = document.getElementsByTagName('head')[0];
        
            // a new CSS
            var css = document.createElement('link');
            css.type = "text/css";
            css.rel = "stylesheet";
            css.href = "new.css";
        
            // a new JS
            var js = document.createElement("script");
            js.type = "text/javascript";
            js.src = "new.js";
        
            // preload JS and CSS
            head.appendChild(css);
            head.appendChild(js);
        
            // preload image
            new Image().src = "new.png";
        
        }, 1000);
    };

## 代码延迟执行


    function sleep(ms) {
        var dt = new Date();
        dt.setTime(dt.getTime() + ms);
        while (new Date().getTime() < dt.getTime());
    }
    sleep(2000);
    
    document.getElementById('inner').innerHTML = "Hello World!";


---

- [http://www.cnblogs.com/youxin/p/3369666.html](http://www.cnblogs.com/youxin/p/3369666.html)
- [http://web.jobbole.com/82317/](http://web.jobbole.com/82317/)