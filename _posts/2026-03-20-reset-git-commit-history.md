---
title:      重置 git 提交记录
description: reset git history
date:       2026-03-20 00:00:00+0800
author:     kode4fun
categories: [tech,git]
tags:
    - git
    - github
---

## 方法一
### 本地重置仓库
#### 删除旧记录
手动删除项目根目录下的 `.git` 文件夹（或者在终端执行 `rm -rf .git`）。

#### 重新初始化

```bash
git init
```

#### 添加文件

```bash
git add .
```
#### 提交首个记录

```bash
git commit -m "🎉 重新开始：重构后的首个提交"
```

### 关联远程 GitHub

由于你删除了 `.git` 文件夹，本地已经忘记了 GitHub 地址，需要重新关联：

```bash
git remote add origin <你的仓库SSH或HTTPS地址>
```

### 强制推送到 GitHub

这是最重要的一步。普通的 `git push` 会因为历史不一致而被拒绝，你需要使用 **`--force` (或 `-f`)** 参数，强制用本地这一个干净的提交覆盖远程的所有历史。

```bash
git push -u origin master --force
# 如果你的默认分支名是 main，请使用：
git push -u origin main --force
```

## 方法二

如果你只是觉得记录乱，但想保留当前的仓库配置，可以使用 `git checkout --orphan` 创建一个 **孤儿分支**，这比删文件夹更显专业，操作如下：

1.  `git checkout --orphan latest_branch` (创建一个没有历史的新分支)
2.  `git add -A`
3.  `git commit -am "清理全部历史"`
4.  `git branch -D master` (删除旧的 master)
5.  `git branch -m master` (将当前分支重命名为 master)
6.  `git push -f origin master` (强制推送)
