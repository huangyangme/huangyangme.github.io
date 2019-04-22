---
layout: post
title: "小程序使用 npm 安装和配置 moment.js"
comments: true
description: ""
keywords: "小程序 moment”
---

# 小程序使用 npm 安装和配置 moment.js



[monent.js](http://momentjs.cn) 是一个 JS 日期处理类库。安装在微信小程序上还是有一些坑的。

## npm 安装

```
# cd 小程序项目根目录
npm install --production moment
```

## 配置

根据[小程序官方文档关于 npm 支持](https://developers.weixin.qq.com/miniprogram/dev/devtools/npm.html)，需要「点击开发者工具中的菜单栏：工具 --> 构建 npm 」，并「勾选“使用 npm 模块”选项」。这样构建完成后才可以使用 npm 包。

`moment.locale` 方法，会从`./locale/`目录下加载语言环境包，手动从 `node_modules/moment/` 目录下，将中文语言环境包，拷贝到 `miniprogram_npm`目录下。

![屏幕快照 2019-04-20 上午9.00.24](https://ws3.sinaimg.cn/large/006tNc79ly1g28tkets3zj30u20b2tbg.jpg)

由于`构建npm`导致入口文件(`moment.js`)经过打包后更名为`index.js`。所以需要再手动将`'../moment'`统一改为`'../index'`。

```javascript
// zh-cn.js
(function(global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' && typeof require === 'function'
    ? factory(require('../moment'))
    : typeof define === 'function' && define.amd
      ? define(['../moment'], factory)
      : factory(global.moment);
})(this, function(moment) {
  'use strict';

  var zhCn = moment.defineLocale('zh-cn', {
    // ...
  });

  return zhCn;
});
```



在小程序页面测试：

```javascript
import moment from 'moment';

let sFromNowText = moment(new Date() - 360000).fromNow();
console.log(sFromNowText);

// 控制台输出："6 minutes ago"
```



感谢&参考：

[https://segmentfault.com/a/1190000016929814](https://segmentfault.com/a/1190000016929814)