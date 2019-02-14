# 让 Footer 黏在页面底部

## Make the Footer Stick to the Bottom of a Page

HTML:

    <html>
        <head>
            <link rel="stylesheet" href="layout.css" ... />
        </head>
        <body>
            <div class="wrapper">
                <p>Your website content here.</p>
                <div class="push"></div>
            </div>
            <div class="footer">
                <p>Copyright (c) 2016</p>
            </div>
        </body>
    </html>

CSS:

<pre><code class="language-css">
* {
    margin: 0;
}
html, body {
    height: 100%;
}
.wrapper {
    min-height: 100%;
    margin: 0 auto -155px; /* the bottom margin is the negative value of the footer's height */
}
footer, .push {
    height: 155px; /* '.push' must be the same height as 'footer' */
}
</code></pre>

DEMO: [http://ryanfait.com/html5-sticky-footer/](http://ryanfait.com/html5-sticky-footer/)
