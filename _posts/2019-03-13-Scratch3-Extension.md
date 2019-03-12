---
layout: post
title: “开发一个 Scratch3 Extension"
comments: true
description: ""
keywords: “Scratch3"
---



环境要求：

- Node
- Git

建议安装`yarn` `webpack` `webpack-dev-server`（不是必须的）

```shell
npm install -g yarn
npm install -g webpack
npm install -g webpack-dev-server
```

```shell
mkdir Scratch3
cd Scratch3
```

## 安装运行 `scratch-gui`

```shell
git clone https://github.com/LLK/scratch-gui
cd scratch-gui
npm install
```

运行`webpack-dev-server --https`，打开:`https://127.0.0.1:8601/`，可以在本地运行 Scratch 3.0 编辑器。

## 安装运行 `scratch-vm`

```shell
cd Scratch3
git clone https://github.com/LLK/scratch-vm
cd scratch-vm
yarn install
yarn link
yarn add uglifyjs-webpack-plugin
yarn run watch

# 新开一个shell
cd scratch-gui
yarn link scratch-vm
```

完成`yarn link scratch-vm`之后，scratch-gui 就会采用我们开发环境里的 scratch-vm，而不是默认的scratch-vm. 这样一来我们就可以定制 scratch-vm了。

## 编写插件

在`scratch-vm/src/extensions`目录创建`scratch3_hello_world/index.js`

这个文件目录和目录下的 `index.js` 就是插件在 scratch-vm 下的全部内容。

## 配置

将插件挂载到 scratch-gui 的插件区：编辑 `scratch-vm/src/extension-support/extension-manager.js`

增加

```javascript
const Scratch3HelloWorldBlocks = require('../extensions/scratch3_hello_world');
```

```javascript
const builtinExtensions = {
    helloWorld: Scratch3HelloWorldBlocks
};
```



编辑`scratch-gui/src/lib/libraries/extensions/index.jsx`



相关阅读：

[https://blog.just4fun.site/create-first-Scratch3-Extension.html](https://blog.just4fun.site/create-first-Scratch3-Extension.html)