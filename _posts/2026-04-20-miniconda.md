---
title:      miniconda 配置 python 环境
description: 建立和管理虚拟环境
date:       2026-04-20 00:00:00+0800
author:     kode4fun
categories: [tech,python]
tags:
    - python
---

### 环境相关
```bash
# 显示虚拟环境
conda env list

# 建立虚拟环境
conda create -n env_name python=3.8 -y

# 删除虚拟环境
conda env remove -n env_name
```
### 关联终端
```bash
conda init cmd.exe powershell
conda config --set auto_activate_base false
conda --version
```

### 安装包
```bash
# 先清空所有旧源，避免冲突
conda config --remove-key channels
# 添加清华主源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
# 关键：conda-forge（matplotlib 绝大多数都在这里）
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --set show_channel_urls yes
# 清理旧索引缓存，立刻生效
conda clean -i

conda install matplotlib
```

### 卸载包
```bash
# 卸载包 + 连带所有依赖
conda uninstall --all 包名

# 清理无用缓存，释放空间
conda clean --all
```

### sublime 编译配置
```json
{
// miniconda.sublime-build
"cmd": ["C:\\ProgramData\\miniconda3\\python.exe", "-u", "$file"],
"path": "C:\\ProgramData\\miniconda3;C:\\ProgramData\\miniconda3\\Library\\mingw-w64\\bin;C:\\ProgramData\\miniconda3\\Library\\bin;C:\\ProgramData\\miniconda3\\Scripts;%PATH%",
"encoding":"utf8",
"env":{"PYTHONIOENCODING":"utf8"},
"file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
"selector": "source.python",
"shell": true,
"interactive":true,
}
```
### 斋藤康毅《深度学习入门：基于Python的理论与实现(deep-learning-from-scratch)》
```bash
# 创建环境（名字：dl_book，python 3.7 最稳）
conda create -n dl_book python=3.8 -y

# 进入环境
conda activate dl_book

# 安装书本必须的包（精准兼容版本）
conda install -y numpy=1.19.5 matplotlib=3.2.2 pillow=7.2.0
```
