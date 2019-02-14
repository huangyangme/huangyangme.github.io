---
layout: post
title: "让 Footer 黏在页面底部"
comments: true
description: ""
keywords: "css"
---

## Make the Footer Stick to the Bottom of a Page

HTML:

```html
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
```

CSS:

```css
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
```

DEMO: [http://ryanfait.com/html5-sticky-footer/](http://ryanfait.com/html5-sticky-footer/)
