---
layout: post
title: "Mac OS X Terminal［Mac］"
comments: true
description: ""
keywords: ""
---


**man**

命令指南，如：`man ls`

`pwd` 的含义是“print working directory”，会显示当前目录的绝对路径。
`ls` 的含义是“list directory contents”，它会列出当前目录的内容。这个命令还有其他参数可选。
`cd` 的含义是“change directory”，它会改变当前目录到你指定的目录。如果你不指定，则会返回你的 home folder。


**mkdir**

“make diretory”的缩写，用来创建文件夹，语法为mkdir后接新文件夹的目录。可以用-p选项，来一起创建路径中不存在的文件夹（这样你就不用挨层创建了）。

**显示隐藏文件**

显示：

```
    defaults write com.apple.finder AppleShowAllFiles -bool true
    KillAll Finder
```

隐藏：

```
    defaults write com.apple.finder AppleShowAllFiles -bool false
    KillAll Finder
```

**清除 DNS 缓存**

Mac OS X 10.10

```
    dscacheutil -flushcache sudo discoveryutil udnsflushcaches
```

Mac OS X 10.7 & 10.8 & 10.9

```
    sudo killall -HUP mDNSResponder
```

Mac OS X 10.5 & 10.6

```
    dscacheutil -flushcache
```

Mac OS X 10.4

```
    lookupd -flushcache
```

其他：[http://www.renfei.org/blog/mac-os-x-terminal-101.html](http://www.renfei.org/blog/mac-os-x-terminal-101.html)