---
title:      在 hyper-v 中安装和使用 Ubuntu 虚拟机
description: Win11 + hyper-v + Ubuntu24.04使用中的坑
date:       2026-04-10 00:00:00+0800
author:     kode4fun
categories: [tech,Ubuntu]
tags:
    - Ubuntu
---

### 启用 hyper-v
- 搜索 `hyper-v` 安装

### 设置全屏
#### 修改 grub 配置文件
使用如下命令编辑 grub 配置

```bash
sudo nano /etc/default/grub
```

修改为

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video=hyperv_fb:2560x1600"
```

执行以下命令

```bash
sudo update-grub
sudo grub-mkconfig
sudo shutdown -h now
```

#### 修改 hyper-v 设置

进入 Powershell ，执行以下命令

```bash
set-vmvideo -vmname <vm-name> -horizontalresolution:2560 -verticalresolution:1600 -resolutiontype single
set-vm <vm-name> -EnhancedSessionTransportType HVSocket
```
