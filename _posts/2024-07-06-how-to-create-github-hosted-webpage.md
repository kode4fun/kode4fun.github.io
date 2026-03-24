---
title:      在 github 上建立个人主页
description:   将 jekyll 本地博客提交到 github
date:       2024-07-06 00:00:00+0800
author:     kode4fun
categories: [Blogging]
tags: [git, github, jekyll, Docker]
---

## 前期准备工作
### 生成密钥

在本地终端输入以下命令建立新的 `SSH Key` 备用：

```bash
ssh-keygen -t rsa -C "a_secret_code_for_ssh_key"
```

这样默认目录下生成如下文件

> .ssh/id_rsa  
> .ssh/id_rsa.pub  
> .ssh/known_hosts  

其中 `id_rsa.pub` 文件的内容复制出来
### 在 github 设置认证

+ 登录 github ，进入 `settings -> SSH and GPG keys`
+ 点击 `SSH Keys` 的 `New SSH Key` 并复制粘贴 `id_rsa.pub` 的内容

### 测试认证

在本地终端使用如下命令测试本地 git 到 _github.com_ 的连接是否成功

```bash
ssh -T git@github.com
```

### 建立 repo

+ 建立新 `repository`，_Repository name_ 要设置为 `<username>.github.io`
+ （可选）进入 `settings -> Develop settings -> Personal access token` 里，生成新的 `token`

## 本地操作

### 初始化 git 仓库
进入本地博客的目录，执行如下名利初始化 git 仓库

```bash
git init
```

### 建立到 repo 的链接
使用如下命令设置 git 用户信息

```bash
git config --global user.name <user-name>
git config --global user.email <user-email>
# 使用 git config user.name 确认设置完成
```

建立本地 git 到 `<usename>.github.io` 这个 _repo_ 的链接。有两种 `htts` 和 `ssh` 两种方式。

#### https 连接

```bash
# https 方式
git remote add homepage https://<your_access_token>@github.com/kode4fun/kode4fun.github.io
```

#### ssh 连接

```bash
# ssh 方式，注意结尾有 .git
git remote add homepage git@github.com:kode4fun/kode4fun.github.io.git
```

建立完成后，使用如下命令确认。

```bash
git remote -v
```

#### 修改和本地提交

```bash
git add .
git commit -m "memo_of_commit"
```

### 远程提交
第一次提交时，使用如下命令

```bash
git push -u homepage master
```

后续修改提交时，可以直接

```bash
git push
```

### 重置远程仓库
备份 git 相关文件外的文件

```bash
git rm -r --force .  # 清空本地
git push # 提交更新
```
