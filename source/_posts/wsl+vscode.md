---
title: 使用WSL+VScode搭建C/C++开发环境
tags:
  - WSL
  - VScode
categories:
  - 开发环境
toc: true
top_img: 'https://s2.loli.net/2022/08/15/zDUApfBkrYIXKNe.png'
cover: 'https://s2.loli.net/2022/08/15/zDUApfBkrYIXKNe.png'
abbrlink: d3d975f8
date: 2022-01-08 13:34:16
---

# 使用WSL+VScode搭建C/C++开发环境

{% note info %}
参考文档：https://docs.microsoft.com/zh-cn/windows/wsl/setup/environment
{% endnote %}

## 安装WSL

### 新版本
对于最新版本的 Windows （内部版本 20262+）可以使用简化的 --install 命令安装WSL。若要检查 Windows 版本及内部版本号，选择 Windows 徽标键 + R，然后键入“winver”，选择“确定”。

```PowerShell
wsl --install
```
--install 命令执行以下操作：
- 启用可选的 WSL 和虚拟机平台组件
- 下载并安装最新 Linux 内核
- 将 WSL 2 设置为默认值
- 下载并安装 Ubuntu Linux 发行版（可能需要重新启动）
  
在此安装过程中，你将需要重启计算机。

### 其他版本

若要更新到 WSL 2，需要运行 Windows 10。
对于 x64 系统：版本 1903 或更高版本，采用内部版本 18362 或更高版本。
对于 ARM64 系统：版本 2004 或更高版本，采用内部版本 19041 或更高版本。
低于 18362 的版本不支持 WSL 2。 使用 Windows Update 助手更新 Windows 版本。

如不是最新版本可以以管理员身份运行powershell，按以下步骤安装：

#### 1.启用 Windows-Subsystem-Linux子系统

```PowerShell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart   
```

#### 2.开启虚拟机功能

```PowerShell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart 
```

#### 3.重启后下载安装Linux内核更新包

下载地址： [适用于 x64 计算机的 WSL2 Linux 内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

#### 4.将wsl2设为默认版本

```PowerShell
wsl --set-default-version 2
```

#### 5.自己选择安装Linux发行版

打开 [Microsoft Store](https://www.microsoft.com/store/apps/9n6svws3rx71)，并选择你偏好的 Linux 分发版。

## 配置开发环境

安装完成后打开WSL的shell

### 1.更新ubuntu软件

```bash
sudo apt update
```

### 2.安装编译器调试器

```bash
sudo apt -y install gcc g++ gdb
```

![](https://s2.loli.net/2022/07/10/KR7aLrdD4tZkjUf.png)

### 3.在VScode中安装wsl插件
在子系统shell中输入` code . `就可以打开VScode，第一次从子系统打开VS Code会自动安装一些插件，等待安装完成后就可以在WSL中进行开发了。