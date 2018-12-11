---
layout: post
title: "如何在 Ubuntu 系统上安装 Ghost"
comments: true
keywords: "Ghost"
author: HuangYang
---

在 Ubuntu 16.04 或 18.04 服务器上安装、配置并运行 Ghost 用于生产环境的完整指南。

官方推荐的生产环境需要具备以下软件栈：

- Ubuntu 16.04 或 Ubuntu 18.04
- NGINX (最低版本 1.9.5 并支持 SSL)
- 被支持 的 Node.js
- MySQL 5.5、5.6 或 5.7 (不要 >= 8.0)
- Systemd
- 最少 1GB 内存的服务器
- 一个域名

在开始之前，您应该为将要使用的域名添加 A 记录 指向服务器的 IP 地址，并确保解析正确。此工作务必提前完成，以便在安装过程中正确配置 SSL。

## 服务器设置

此部分说明是为了确保满足安装 Ghost-CLI 的先决条件。

### 创建一个新用户 👋

打开终端窗口并以 root 用户登录服务器：

```
# 通过 SSH 登录
ssh root@your_server_ip

# 创建一个新用户并按提示操作
adduser <user>
```

>  注意：不要使用 `ghost` 作为用户名，否则会与 Ghost-CLI 冲突，因此务必为新用户选择其他名称。

```
# 将用户添加到超级用户组以赋予管理权限
usermod -aG sudo <user>

# 然后以新用户名登录
su - <user>
```

### 更新软件包

确保软件包列表和已安装的软件包是最新版本。

```
# 更新软件包列表
sudo apt-get update

# 更新已安装的软件包
sudo apt-get upgrade
```

按照提示输入刚才在前一步（创建用户时）设置的密码。

### 安装 NGINX

Ghost 依赖 NGINX 服务，并且 SSL 配置需要 NGINX 1.9.5 或更高版本。

```
# 安装 NGINX
sudo apt-get install nginx
```

如果开启了 ufw ，需要让防火墙允许 HTTP 和 HTTPS 连接。打开防火墙：

`sudo ufw allow 'Nginx Full'`

### 安装 MySQL

然后安装 MySQL 作为生产环境数据库。

```
# 安装 MySQL
sudo apt-get install mysql-server
```

#### Ubuntu 18.04 上的 MySQL

如果您的系统是 Ubuntu 18.04，您需要为 MySQL 设置一个密码以确保兼容 Ghost-CLI。这需要一些额外步骤！

```
# 设置密码
sudo mysql

# 为用户设置密码
# 将 'password' 替换为您自己的密码，务必保留引号！
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

# 然后退出 MySQL
quit

# 然后以新创建的用户重新登录 Ubuntu
su - <user>
```

### 安装 Node.js

首先需要按照以下步骤在系统级环境中安装 被支持 的 Node 版本。如果您未按如下方式设置，可能会遇到问题。

```
# 添加 NodeSource APT 仓库用于安装 Node 8
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash

# 安装 Node.js
sudo apt-get install -y nodejs
```

## 安装 Ghost-CLI

Ghost-CLI 是一个命令行工具，用于辅助安装并设置 Ghost，使用方便、快捷。通过 `npm` 或 `yarn` 即可安装。

`sudo npm install ghost-cli@latest -g`

安装之后，您可以随时执行 `ghost help` 命令以查看所有可用命令列表。

## 安装 Ghost

服务器正确配置并安装 `ghost-cli` 之后，您就可以安装 Ghost 了。以下步骤都是推荐设置。如果您希望拥有更好的控制，CLI 提供了 参数 让您深入每一步骤并按照您的希望自定义。

> 注意：如果将 Ghost 安装在 `/root` 或 `home/<user>` 目录将导致安装错误。务必确保使用一个专用目录并设置正确的权限。

### 创建目录

为即将安装的 Ghost 创建一个专用目录，并设置正确的权限。

```
# 在此我们将目录命名为 'ghost' 。您可以选择任何名称
sudo mkdir -p /var/www/ghost

# 将 <user> 替换为您自己的用户名并拥有此目录
sudo chown <user>:<user> /var/www/ghost

# 设置正确的权限
sudo chmod 775 /var/www/ghost

# 然后进入此目录
cd /var/www/ghost
```


### 执行安装流程

到此，开始安装 Ghost。一条命令即可 😀

`ghost install`

### 安装过程中的站点配置问题

安装时，CLI 会提问几个关于站点配置的问题。

#### 博客网址

输入正确的 URL，以便您的站点可以被访问，并且要包含 HTTP 或 HTTPS 协议。例如，` https://example.com`。如果您使用的是 HTTPS，Ghost-CLI 会为您设置 SSL 证书。使用 IP 地址会导致错误。

#### MySQL hostname

此设置用于确定 MySQL 数据库的访问位置。当 MySQL 与 Ghost 安装在同一台服务器上，输入 localhost (点击 回车/Enter 键使用默认值) 即可。如果 MySQL 安装在其他服务器上，输入服务器对应的 hostname。

#### MySQL 用户名 / 密码

如果已经创建了 MySQL 数据库则输入用户名。否则就输入 `root`。接下来输入用户名所对应的密码。

#### Ghost 数据库名称

输入数据库名称。如果您输入的是一个 MySQL 的 非-root 用户，您就必须确保数据库已经存在并且此用户拥有正确的读写权限。如果您提供的是 root 用户，Ghost-CLI 可以帮您自动创建数据库。

#### 是否为 Ghost 设置一个独立的 MySQL 用户？ (推荐)

如果您提供的是 MySQL 的 root 用户名，Ghost-CLI 可以为您创建一个专门用于访问/编辑 Ghost 数据库的用户。

#### 是否设置 NGINX？ (推荐)

自动设置 NGINX 之后就可以让您的站点上线并被全世界访问了。手动设置 NGINX 也是可以的，但是您得准备好选择困难模式了吗？

#### 是否设置 SSL？ (推荐)

如果您使用的是 https 协议，并且已经将域名指向了正确的位置，Ghost-CLI 可以自动为您申请 Let's Encrypt 的 SSL 证书。您也可以将来任何时候通过执行 ghost setup ssl 命令自动设置证书。

#### 输入您的邮箱地址

SSL 认证过程中需要输入邮箱地址以便您的认证有任何问题时都能接收邮件通知，包括更新证书时也需要邮箱地址。

#### 是否设置 systemd？ (推荐)

systemd 是推荐的进程管理工具，用于保证 Ghost 持续运行。我们建议您选择 yes ，但是您也可以使用自己的进程管理工具。

#### 是否启动 Ghost？

选择 yes 启动 Ghost，站点开始运行。

## Ghost 的维护

一旦 Ghost 正确安装、设置之后，保持 Ghost 良好维护已经更新就变成了最重要的任务。幸好，Ghost-CLI 提供了相对容易的方法。执行 `ghost help` 查看所有可用的命令列表，或者探索完整的 Ghost-CLI d文档。

## 如果安装失败怎么办

如果遇到严重的安装错误，执行 `ghost uninstall` 命令删除当前安装并重试。最好是删除用于安装的文件夹，以确保没有遗留。

如果安装时被打断或者连接失败，执行 `ghost setup` 重新运行配置流程。

- - - - - 

扩展阅读：

- [如何在 Ubuntu 系统上安装 Ghost](http://docs.ghostchina.com/install/ubuntu/#%E5%AE%89%E8%A3%85-ghost-cli)

