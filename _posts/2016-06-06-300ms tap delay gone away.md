---
layout: post
title: "300 毫秒延迟，浏览器开发商提供的解决方案"
comments: true
description: ""
keywords: ""
---

### 禁用缩放

```html
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
```

Chrome 开发团队不久前宣布，在 Chrome 32 这一版中，他们将在包含 `width=device-width` 或者置为比 `viewport` 值更小的页面上禁用双击缩放。当然，没有双击缩放就没有 300 毫秒点击延迟。

### 指针事件 (Pointer Events)

```css
    a[href], button {
        -ms-touch-action: none;
        touch-action: none;
    }
```

[http://caniuse.com/#search=touch-action](http://caniuse.com/#search=touch-action)

- [300 毫秒点击延迟的来龙去脉](http://thx.github.io/mobile/300ms-click-delay/)
- [300ms tap delay,gone away](http://updates.html5rocks.com/2013/12/300ms-tap-delay-gone-away)

## 当前 Polyfill

- [tap.js](https://github.com/alexgibson/tap.js)
- [fastclick](https://github.com/ftlabs/fastclick)