---
layout: post
title: "CSS3 3D 实现翻牌效果"
comments: true
description: ""
keywords: "CSS3 3D”
---

```html
<div class="card">
    <!-- 卡牌正面 -->
    <div class="card-face card-front">正面</div>
    <!-- 卡牌反面 -->
    <div class="card-face card-back">反面</div>
</div>
```



```css
.card {
    position: relative;
    width: 200px;
    height: 300px;
    margin: 200px auto;
    perspective: 600px;
    transition: transform 1s;
    transform-style: preserve-3d;
  }

  .card:hover {
    transform: rotateY(180deg);
  }

  .card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    color: #fff;
    font-size: 50px;
    backface-visibility: hidden;
  }

  .card-front {
    background: #f00;
    transform: rotateY(180deg)
  }

  .card-back {
    background: #4577dc;
  }
```

[原文链接](https://juejin.im/post/5b191501e51d4506825f1480)