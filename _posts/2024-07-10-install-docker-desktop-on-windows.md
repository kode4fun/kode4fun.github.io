---
title:      windows 环境下 Docker 安装和配置
description: windows 下安装 Docker 
date:       2024-07-10 00:00:00+0800
author:     kode4fun
categories: [tech,docker]
tags:
    - Docker
    - WSL
    - Ubuntu
---

# Windows 环境下 Docker 安装与配置指南

本文详细介绍如何在 Windows 系统下安装和配置 Docker，包括 WSL 子系统的准备、Docker Desktop 的安装、以及在 WSL 内安装原生 Docker。适合开发者和运维人员参考。

---

## 一、准备工作

### 1. 启用虚拟化
- 进入 BIOS，开启虚拟化（Virtualization）选项。
- 在 Windows 中：
    - 打开“控制面板 → 程序 → 启用或关闭 Windows 功能”，勾选 `Hyper-V` 和 `适用于 Linux 的 Windows 子系统（WSL）`。

### 2. 安装 WSL
- 管理员权限打开 PowerShell，执行：

```bash
wsl --install           # 安装 WSL
wsl --update            # 升级 WSL
wsl --set-version 2     # 设置为 WSL 2（Docker 推荐）
wsl --status            # 查看 WSL 状态
```

### 3. 安装 Ubuntu 子系统（可选）
- 方法一：微软应用商店搜索 Ubuntu 并安装。
- 方法二：命令行手动下载并安装 Ubuntu 22.04：

```bash
Invoke-WebRequest -Uri https://aka.ms/wslubuntu2404 -OutFile Ubuntu2404.appx -UseBasicParsing
Add-AppxPackage .\Ubuntu2404.appx
```

- 设置 Ubuntu 为默认子系统：

```bash
wsl --list --verbose
wsl --set-default Ubuntu-20.04
```

- 设置默认语言为中文（可选，未测试）：

```bash
sudo dpkg-reconfigure locales
```

---

## 二、安装 Docker

### 1. 安装 Docker Desktop for Windows（推荐）
- 从 [Docker 官网](https://www.docker.com/products/docker-desktop/) 下载并安装 Docker Desktop。
- 安装后可在设置中调整：
    - `Settings → Resources → Advanced → Disk Image location`（磁盘镜像位置）
    - `Settings → Resources → WSL Integration`（WSL 集成）
- Docker Desktop 支持图形界面管理容器，适合初学者和日常开发。

### 2. 在 WSL 子系统内安装原生 Docker（进阶）

#### 2.1 下载 Docker 安装包
- 访问 [Docker 官方仓库](https://download.docker.com/linux/ubuntu/dists/) ，根据 Ubuntu 版本选择对应包：
    - Ubuntu 18.04: Bionic Beaver  
    - Ubuntu 20.04: Focal Fossa  
    - Ubuntu 22.04: Jammy Jellyfish  
    - Ubuntu 22.10: Kinetic Kudu  

#### 2.2 安装 Docker 组件
- 在 WSL Ubuntu 内执行：

```bash
sudo dpkg -i containerd.io_1.6.33-1_amd64.deb
sudo dpkg -i docker-ce-cli_26.1.4-1~ubuntu.22.04~jammy_amd64.deb
sudo dpkg -i docker-ce_26.1.4-1~ubuntu.22.04~jammy_amd64.deb
sudo dpkg -i docker-buildx-plugin_0.14.1-1~ubuntu.22.04~jammy_amd64.deb
sudo dpkg -i docker-compose-plugin_2.27.1-1~ubuntu.22.04~jammy_amd64.deb
sudo dpkg -i docker-ce-rootless-extras_26.1.4-1~ubuntu.22.04~jammy_amd64.deb
```

#### 2.3 检查 Docker 服务状态与版本

```bash
sudo docker version
```

#### 2.4 启动与自启动配置
- 查看系统启动类型：

```bash
ps -p 1 -o comm=
```
- 若显示 `init`，使用 sysvinit 命令；若显示 `systemd`，使用 systemctl 命令。
- 添加 Docker 服务自启动：

```bash
sudo service docker start
sudo service docker enable
sudo service docker status
sudo chkconfig docker on
```

- 若有镜像文件，可用 `wsl --import` 导入。

---

## 三、WSL 常用命令

### 1. 查看 WSL 虚拟机状态与管理

```bash
wsl -l -v           # 查看虚拟机状态
wsl --shutdown vm-name   # 停止虚拟机
```

### 2. 迁移虚拟机

```bash
wsl --export Ubuntu-20.04 D:\ProgramData\WSL\Ubuntu\Ubuntu.tar
wsl --unregister Ubuntu-22.04
wsl --import Ubuntu-22.04 E:\wsl\vm D:\ProgramData\WSL\Ubuntu\Ubuntu.tar
```

- 执行完毕后可重新启动 Ubuntu，注意默认用户可能丢失。

### 3. 恢复默认用户

- 执行如下命令恢复默认用户：

```bash
Ubuntu2204 config --default-user u-xhp
```

> 注意：命令中的发行版名称的版本号是纯数字，如 Ubuntu-22.04 对应 Ubuntu2204。

---

## 四、常见问题与建议

1. **WSL 版本不兼容**：确保已设置为 WSL 2，否则 Docker 无法正常运行。
2. **虚拟化未开启**：BIOS 必须开启虚拟化，否则 WSL 和 Docker 都无法启动。
3. **磁盘空间不足**：建议将 Docker 镜像存储位置设置到大容量磁盘。
4. **网络问题**：如遇到容器无法联网，检查 Windows 防火墙和代理设置。
5. **权限问题**：WSL 内操作 Docker 需 root 权限，建议使用 sudo。

---

## 五、参考链接

- [Docker 官方文档](https://docs.docker.com/)
- [WSL 官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/)
- [微软官方 Ubuntu 下载](https://aka.ms/wslubuntu2404)

---

> 持续修改中
