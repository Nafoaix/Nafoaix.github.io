---
title: WSL更换镜像源
tags:
  - WSL
categories:
  - 开发环境
toc: true
top_img: 'https://s2.loli.net/2022/07/22/FHrtNDyM3JaEzAP.png'
cover: 'https://s2.loli.net/2022/07/22/FHrtNDyM3JaEzAP.png'
abbrlink: 48884a62
date: 2022-03-22 19:56:17
---

# WSL更换清华镜像源

Ubuntu、Python、Nodejs、MySQL、Git、Chromium、Docker、Homebrew 等一系列的常用开源系统、软件都是国外开发的，下载地址位于国外，从国内访问、下载、更新速度慢所以我们要使用镜像源，其中清华源镜像镜像源数量最多，所以这里用清华源镜像做替换，下面是替换方法：

1. 备份原文件

```bash
sudo cp  /etc/apt/sources.list /etc/apt/sources.list.bak
```

2. 将`/etc/apt/sources.list`文件内容替换为清华镜像源

```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```

3. 更新软件源

```bash
sudo apt update
```