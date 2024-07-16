---
layout:     post
title:      "Win10 环境下 Docker 安装和配置"
subtitle:   "前方有坑，注意避让"
date:       2024-07-12 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - Docker
    - WSL
    - Ubuntu
---

> 从 0 开始入坑 Docker

## 安装 wsl 2
### 准备工作
从**控制面板** => **程序** => **启用或关闭功能** 中启用 **`Hyper-V`** 和 **`适用于Linux的Windows子系统`**
### 安装 wsl
管理员权限打开 Powershell  
运行 `wsl --install` 或 `wsl --update`  
运行 `wsl --status`  
### 启用虚拟化
重启电脑，进入 BIOS 启用虚拟化技术
### 安装 Ubuntu 22.04 子系统
## 安装 Ubuntu
### 下载 Ubuntu
因为微软应用商店安装不成功，可使用如下命令手动下载 Ubuntu 22.04
```bash
Invoke-WebRequest -Uri https://aka.ms/wslubuntu2204 -OutFile Ubuntu2204.appx -UseBasicParsing
```
### 安装
安装命令 `Add-AppxPackage .\Ubuntu2204.appx`  
安装完成后可从开始菜单启动 Ubuntu22.04
### 中文设置
使用如下命令后，选择 zh_CN.UTF-8 为默认即可设置默认语言为中文（未测试）
```bash
sudo dpkg-reconfigure locales`
```

## 安装 Docker
### 下载
从 `https://download.docker.com/linux/ubuntu/dists/` 根据 Ubuntu 发行代号找到对应版本

```
Ubuntu 18.04: Bionic Beaver
Ubuntu 20.04: Focal Fossa
Ubuntu 22.04: Jammy Jellyfish
Ubuntu 22.10: Kinetic Kudu
```
### 安装 Docker
```bash
sudo dpkg -i containerd.io_1.6.33-1_amd64.deb
sudo dpkg -i docker-ce-cli_26.1.4-1~ubuntu.22.04~jammy_amd64.deb
sudo dpkg -i docker-ce_26.1.4-1~ubuntu.22.04~jammy_amd64.deb
sudo dpkg -i docker-buildx-plugin_0.14.1-1~ubuntu.22.04~jammy_amd64.deb
sudo dpkg -i docker-compose-plugin_2.27.1-1~ubuntu.22.04~jammy_amd64.deb
sudo dpkg -i docker-ce-rootless-extras_26.1.4-1~ubuntu.22.04~jammy_amd64.deb
```

###  检查Docker服务的状态和安装的版本
```bash
sudo docker version
```
### 查看系统启动类型
查看系统是用 `sysvinit` 还是 `systemd` 启动的

```bash
ps -p 1 -o comm=
```
如果是显示的是 `init`，则使用 `sysvinit` 命令，否则使用 `systemctl` 命令

### 添加自启动
在系统启动时自动启动Docker服务
```bash
sudo service docker start
sudo service docker enable
sudo service docker status
sudo chkconfig docker on
```

+ 如果有镜像，可以 `wsl --import` 导入

### 安装 Docker
+ 下载 Docker `https://learn.microsoft.com/zh-cn/training/modules/use-docker-business-central/3-install-docker-windows`
+ 安装 Docker，查看版本 `docker --version`


## 迁移虚拟机
### 查看WSL虚拟机状态并停止
+ 在CMD中执行wsl -l -v命令，查看本机全部的wsl虚拟机的名称和状态：
+ 执行wsl --shutdown命令使其停止运行，再次执行wsl -l -v确认停用。

### 导出/导入备份
先手动创建迁移的目标文件夹，然后通过命令导出原虚拟机的备份：

```bash
wsl --export Ubuntu D:\ProgramData\WSL\Ubuntu\Ubuntu.tar
```

等待命令执行完毕，先在目标文件夹里确认备份文件Ubuntu.tar后，再进行下一步。

### 注释原wsl虚拟机
```bash
wsl --unregister Ubuntu-22.04
```

将备份导入到新的目标文件夹中：

```bash
wsl --import Ubuntu-22.04 D:\ProgramData\WSL\Ubuntu D:\ProgramData\WSL\Ubuntu\Ubuntu.tar
```

等待命令执行完毕，就可以重新启动Ubuntu了。这时候，会发现原来的默认用户没了。

恢复默认用户

执行如下命令Linux发行版名称 config --default-user 原本用户名：

```
Ubuntu2204 config --default-user u-xhp
```

注意：命令中的发行版名称的版本号是纯数字，比如Ubuntu-22.04就是Ubuntu2204。
等待命令执行完毕，再次运行Ubuntu，发现用户就恢复原来的用户了。

### 运行
运行第一个Docker镜像

```bash
    docker run hello-world
    wsl --import Ubuntu-22.04 g:\Docker\Ubuntu g:\Docker\Ubuntu.tar
```