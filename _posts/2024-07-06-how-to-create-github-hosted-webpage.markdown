---
layout:     post
title:      "在 github 上建立个人主页"
subtitle:   "基本流程不完全记录"
date:       2024-07-06 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - git
    - github
    - WSL
---

> 在 github 上建立主页的流程的不完全记录。

### 初始化 git
使用如下命令初始化 git 的设置

```bash
git config --global user.name <user-name>
# 使用 git config user.name 确认设置完成
git config --global user.email <user-email>
```

在本地终端输入以下命令建立新的 `SSH Key` 备用：

```bash
ssh-keygen -t rsa -C "a_secret_code_for_ssh_key"
```

### 在 github.com 的准备工作
注册一个 `github.com` 的账号。

复制上面生成的 `ssh_key`，粘贴到 _github_ 账户设置的 `settings -> SSH and GPG keys`。

在本地终端使用如下命令测试本地 git 到 _github.com_ 的连接是否成功。

```bash
ssh -T git@github.com
```

建立新 `repository`，_Repository name_ 要设置为 `<username>.github.io`

进入 `settings -> Develop settings -> Personal access token` 里，生成新的 `token`。

### 建立到 repo 的链接
建立本地 git 到 `<usename>.github.io` 这个 _repo_ 的链接。有两种 `htts` 和 `ssh` 两种方式。

建立 `https` 链接的方式如下：

```bash
# https 方式
git remote add homepage https://<your_access_token>@github.com/kode4fun/kode4fun.github.io
```

建立 `ssh` 链接的方式如下：

```bash
# ssh 方式，注意结尾有 .git
git remote add homepage git@github.com:kode4fun/kode4fun.github.io.git
```

建立完成后，使用如下命令确认。

```bash
git remote -v
```

### 建立本地博客
使用下面的命令克隆模板和主题到本地。

```bash
# git clone git@github.com:keysaim/huxpro.github.io.git
git clone git@github.com:Huxpro/huxblog-boilerplate.git
```

然后对本地博客进行设置、修改、提交。

```bash
git add .
git commit -m "memo_of_commit"
```

### 本地预览博客的方法
启动 docker 并本地预览

```bash
sudo service docker start
sudo docker run -it --rm -v "/mnt/<path_to_your_pages>/":/usr/src/app -p "4000:4000" starefossen/github-pages
```

这样就可以在 `http://container_ip_address:4000/` 预览。

### 同步提交
第一次提交时，使用如下命令

```bash
git push -u homepage master
```
后续修改提交时，可以直接

```bash
git push
```