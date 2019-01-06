---
layout: post
title: "基于 CentOS 搭建 Ghost 博客"
comments: true
description: ""
keywords: "Ghost, CentOS"
---


## 安装 Node.js

```
yum update -y
yum groupinstall -y "Development Tools"
curl --silent --location https://rpm.nodesource.com/setup_6.x | sudo bash -
yum -y install nodejs
npm config set registry https://registry.npm.taobao.org
npm i -g cnpm
```

安装成功后通过运行 `node -v` 及 `npm -v` 出现版本号即可表示安装成功。

## 安装 Ghost Client （ghost-cli）

`npm i -g ghost-cli`

安装成功后通过运行 `ghost -v`，出现版本号即可表示安装成功。

## 安装 Ghost

### 添加 Ghost 运行用户并创建目录

```
adduser ghost
mkdir /var/www
mkdir /var/www/ghost
chown ghost /var/www/ghost
```

### 安装 Ghost

以 SQLite3 作为 Ghost 的数据库。

```
cd /var/www/ghost
ghost install local --db=sqlite3
```

![](/images/2018-12/2018-12-05-21-07-24.jpg)
安装成功！

## 安装 Nginx

### 添加 Nginx 到 yum 源

使用以下命令添加 CentOS 7 Nginx yum 资源库：

`rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`

### 安装 Nginx

`yum install -y nginx`

![](/images/2018-12/2018-12-05-21-08-30.jpg)
Nginx 完成安装在服务器中

### 启动 Nginx

刚安装的 Nginx 不会自行启动，需要通过如下命令启动

`systemctl start nginx.service`

如果一切进展顺利的话，现在你可以通过你 IP( http://<您的 CVM IP 地址>/ )来访问你的 Web 页面来预览一下 Nginx 的默认页面。

![](/images/2018-12/2018-12-05-21-10-43.jpg)

如果看到上面的页面,那么说明你的 CentOS 中的 Nginx 已经正确安装。

另外还可以通过 `systemctl enable nginx.service` 命令加入开机启动项。

## Nginx 配置反向代理

### 修改 config 文件

请确保 Ghost 已经在运行阶段方可进行如下操作。

`vi /etc/nginx/conf.d/default.conf`

运行上面的命令后，再键入 `i` 然后移动光标在约第七行修改相关文件代码：

```
location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
```

改为：

```
location / {
        proxy_pass http://127.0.0.1:2368;
        proxy_redirect default;
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
```

完成后通过按 `ESC` ，随后输入 `:wq` 回车保存。

![](/images/2018-12/2018-12-05-21-15-39.jpg)

然后运行 `nginx -s reload` 重启 Nginx。

`service nginx restart`

- - - - - 


扩展阅读：[基于 CentOS 搭建 Ghost 博客](https://cloud.tencent.com/developer/labs/lab/10281)

