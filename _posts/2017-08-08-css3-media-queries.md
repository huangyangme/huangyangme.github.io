---
layout: post
title: "Media Queries with Sass"
comments: true
description: ""
keywords: "sass"
---

```css
    //定义设备宽度
    $phone-width: 320px;
    $tablet-width: 768px;
    $desktop-width: 1024px;
    
    //手机
    @mixin phone {
      @media (max-width: #{$tablet-width - 1px}) {
        @content;
      }
    }
    
    //平板
    @mixin tablet {
      @media (min-width: #{$tablet-width}) and (max-width: #{$desktop-width - 1px}) {
        @content;
      }
    }
    
    //桌面
    @mixin desktop {
      @media (min-width: #{$desktop-width}) {
        @content;
      }
    }
    
    //视网膜屏幕
    @mixin retina {
      @media
        only screen and (-webkit-min-device-pixel-ratio: 2),
        only screen and (min--moz-device-pixel-ratio: 2),
        only screen and (-o-min-device-pixel-ratio: 2/1),
        only screen and (min-device-pixel-ratio: 2),
        only screen and (min-resolution: 192dpi),
        only screen and (min-resolution: 2dppx) {
        @content;
      }
    }

    p {
      font-size: 16px;
      @include tablet {
        font-size: 18px;
      }
      @include desktop {
        font-size: 20px;
      }
    }
```

相关：[http://davidwalsh.name/write-media-queries-sass](http://davidwalsh.name/write-media-queries-sass)