---
layout: post
title: "在一台 CentOS 云服务器上配置两（多）个 Ghost"
comments: true
description: ""
keywords: "Ghost"
---

创建另一个 nginx 反向代理配置文件 `default2.conf`，并修改域名指向和端口：

```
cd /etc/nginx/conf.d/
cp default.conf default2.conf
```

esc键 + `:wq`

最后记得重启 Nginx。

`service nginx restart`

- - - - - 

扩展阅读：

- [在服务器上部署多个Ghost](https://anyinfa.com/zai-fu-wu-qi-shang-bu-shu-duo-ge-ghost/)
- [基于 CentOS 搭建 Ghost 博客](/post/writting/2018-12-05)
- [How To Proxy Multiple Ghost Blogs Through Nginx](https://www.ghostforbeginners.com/how-to-proxy-ghost-through-nginx-for-security-and-multi-blog-setup/)
- [vi/vim 基本使用](http://www.runoob.com/linux/linux-vim.html)