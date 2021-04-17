---
title: 阿里云服务器部署个人网盘服务
tags:
    - LINUX
    - 服务器
categories:
    - LINUX
    - 服务器
toc: true
cover: "https://cdn.jsdelivr.net/gh/Nafoaix/pic-repo@master/cover/kiftd.png"
date: 2021-04-13 09:17:16
---

# 阿里云服务器部署个人网盘服务

> 相关资源：
> kiftd ： <https://github.com/KOHGYLW/kiftd>

## 1.安装JAVA运行环境

```bash
yum install java-11-openjdk-devel
```

安装完成后查看版本，输出下面内容说明已经安装好。

```bash
[root@nafxali ~]# java -version
openjdk version "11.0.9.1" 2020-11-04 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.9.1+1-LTS)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.9.1+1-LTS, mixed mode, sharing)
```

CentOS 8还支持无头版本的OpenJDK，该版本提供了无需图形用户界面（不支持键盘，鼠标和显示系统）来执行应用程序所需的最少Java运行时，该版本具有更少的依赖性和更少的系统资源，因此它更适合于服务器应用程序。

```bash
sudo yum install java-11-openjdk-headless
```

## 2.下载kiftd包并解压

Linux压缩包：<https://cloud.189.cn/t/ruIr2eyeyUVb> (访问码:v5pj)
下载压缩包并上传到服务器，在具备rwx权限且不含中文的文件夹中解压，即可以用命令直接启动kiftd服务。

```bash
java -jar kiftd-x.x.x-xxx.jar -console 
```

## 3.后台运行kiftd

### Screen 工具

Screen 工具能够虚拟出一个终端并执行相应的操作。使用前，您需要先安装
该工具，例如在 Ubnutu 系统中，您可以使用以下命令进行安装： 

```bash
apt-get install screen 
```

该工具安装完成后，您便可以使用它来运行 kiftd:  

#### 1.创建一个虚拟终端： 

```bash
screen -S {自定义的虚拟终端名称} 
```

例如： 

```bash
screen -S kiftd 
```

#### 2.在虚拟终端中以命令模式启动 kiftd： 

```bash
java -jar kiftd-x.x.x-xxx.jar -console 
```

之后您便可以断开 SSH 连接或者使用 Ctrl+A Ctrl+D 键暂时退出虚拟终端以进行其他操作。

#### 3，当您需要继续操作 kiftd 时： 

请使用 SSH 重新链接至远程服务器，之后使用虚拟终端名重新回到 screen虚拟终端： 

```bash
screen -r {自定义的虚拟终端名称} 
```

例如：

```bash
screen -r kiftd 
```

这样您便能返回之前的虚拟终端并继续操作 kiftd。

## 4.开放端口

### 服务器防火墙开放端口

```bash
查看防火墙某个端口是否开放
firewall-cmd --query-port=3306/tcp
开放防火墙端口3306
firewall-cmd --zone=public --add-port=3306/tcp --permanent
注意：开放端口后要重启防火墙生效
重启防火墙
systemctl restart firewalld
关闭防火墙端口
firewall-cmd --remove-port=3306/tcp --permanent
查看防火墙状态
systemctl status firewalld
关闭防火墙
systemctl stop firewalld
打开防火墙
systemctl start firewalld
开放一段端口
firewall-cmd --zone=public --add-port=40000-45000/tcp --permanent
查看开放的端口列表
firewall-cmd --zone=public --list-ports
查看被监听(Listen)的端口
netstat -lntp
检查端口被哪个进程占用
netstat -lnp|grep 3306
```

### 阿里云开放端口

实例=>安全组=>手动添加
