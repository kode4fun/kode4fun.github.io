---
title: 在 Windows 上部署 stable diffusion
description: 本地部署 stable diffusion
author: kode4fun
date: 2025-06-27 00:00:00+0800
categories: [tech,AI]
tags: [AI, stable diffusion]
---

## 安装 CUDA 和 git
+ 检测支持的 CUDA 版本 `nvidia-smi`
+ 从 [CUDA 官网](https://developer.nvidia.com/cuda-toolkit-archive) 下载并安装
+ 安装后确认 CUDA 版本 `nvcc --version`
+ 下载 [Git-2.50.0-64-bit](https://ghfast.top/https://github.com/git-for-windows/git/releases/download/v2.50.0.windows.1/Git-2.50.0-64-bit.exe) 并安装

## 下载安装 Anaconda
从 [Anaconda 官网](https://repo.anaconda.com/archive/) 下载并安装，安装后执行如下命令

```bash
conda create -n stablediffusion python=3.10.8
conda env list
activate stablediffusion
```

## 安装 sd 相关文件
### 下载 sd 官方 checkpoint 文件
+ 从 [stable diffusion 官网](https://huggingface.co/CompVis/stable-diffusion-v-1-4-original) 下载
+ 将文件重命名为 model.ckpt

### 下载 stable-diffusion-webui
克隆 `stable-diffusion-webui` 到本地

```bash
git clone https://gh.llkk.cc/https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
```

## 部署
+ 将 _model.ckpt_ 放到 _models_ 文件夹下
+ 将 _repositories_ 文件夹和 _GFPGANv1.3.pth_ 放到 _webui_ 根目录下（_webui.bat_ 同一个文件夹）
+ 在 conda 虚拟环境命令行中进入到根目录中打开 _webui-user.bat_
+ 等一会之后，自动打开 _Stable Diffusion Web UI_，本地默认地址为 `http://127.0.0.1:7860`

### 汉化
+ https://gh.llkk.cc/https://github.com/dtlnor/stable-diffusion-webui-localization-zh_CN.git
