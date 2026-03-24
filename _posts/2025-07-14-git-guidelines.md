---
title:      git 使用指南
description: git 常用命令速查
date:       2025-07-14 00:00:00+0800
author:     kode4fun
categories: [tech,git]
tags:
    - git
    - github
---

## 配置 git
### 列出配置
```bash
# 设置全局用户名和用户邮箱
git config --global user.name <username>
git config --global user.email <user@mail.com>
# 显示设置信息
git config --list --show-origin
```

### 命令别名
```bash
# 建立 git co 等价于 git checkout
git config --global alias.co checkout

# 使用如下命令恢复文件到上次提交状态
git config --global alias.unstage 'reset HEAD --'

# 最后一次提交记录
git config --global alias.last 'log -1 HEAD'
```

### git clone
```bash
git clone --depth=1 --banch=master <url>
```

### git status

```bash
git status --short # 或 git status -s
```

### git log

```bash
# 查看最近 2 次的纪录
git log --patch -2
# 以单行显示结果
git log --oneline
# 显示状态信息
git log --state
# 指定输出时格式设置
git log --graph
git log --pretty=oneline # full/fuller/short
git log --pretty=format:""
```

### git show

```bash
git show <tag>
```


## 文件管理
### git add

```bash
# 将文件添加到暂存区
git add <file>
```

### git rm

```bash
# 物理性删除文件
git rm <file>
# 只是从暂存区移除而不物理删除用
git rm --cached <file>
# 删除之前修改过或已经放到暂存区的文件
git rm --force <file>
```

### git mv

```bash
git mv <old_name> <new_name>
```

## git commit

```bash
git commit -a -m <comment>
```

## 撤销操作
### 修改上次的提交
```bash
# 如果要添加新文件需要 git add newfile
git commit --amend -a -m new_msg
git commit --amend --no-edit
# 修改后推送到远程分支（防止覆盖其它人自上次拉取以来的更新）
git push --force-with-release
```

### 取消暂存的文件
```bash
# 取消前一次的 git add <file>
git reset HEAD <file>
# 建议使用
git restore --staged <file>
```

### 撤销对文件的修改
```bash
# 取消文件修改至最后一次提交的状态
git checkout <file>
# 建议使用
git restore <file>
```

## 远程仓库
### 查看远程仓库

```bash
# 显示远程配置信息
git remote -v
git remote show <remote>
```

### 添加远程仓库

```bash
# 设置远程 repo 信息
git remote add <remote> <url>
```

### 从远程仓库中抓取与拉取

```bash
# 拉取远程数据
git fetch <remote>
git pull <remote>
```

### 推送到远程仓库

```bash
git push <remote> <branch>
# 推送时包含标签
git push <remote> <branch> <tag>
git push <remote> <branch> --tags
```

### 远程仓库的重命名与移除

```bash
# 重命名远程 repo 的别名
git remote rename <old_name> <new_name>
# 移除远程 repo
git remote remove <remote>
```

## 标签
标签 **_tag_** 分为 _light-weight_ 和 _annoted_ 两种

### 列出标签

```bash
git tag
git tag --list "v0.1*"
git show <tag>

```bash
git show <tag>
```

### 建立标签

```bash
# 建立 light-weight 标签
git tag v0.1-lw
# 建立 annotated 标签
git tag -a v0.1 -m "version 0.1"
# 为之前的 commit 补充标签
git tag -a v0.2 <hash>
```

### 共享标签
```bash
# 默认情况下，git push 命令不会传送标签到远程 repo
git push orgin master <tag>
git push origin master --tags
```

### 删除标签

```bash
# 删除本地标签
git tag -d <tagname>
# 删除远程 repo 上的标签
git push origin --delete <tagname>
```

## 分支
### 查看分支

```bash
# 只查看本地分支
git branch
# 只查看远程分支
git branch -r
# 查看本地和远程分支
git branch -a
```

### 建立分支

```bash
git branch <branch>
```

### 切换分支

```bash
# 切换到分支，高版本建议使用 git switch
git checkout <branch>
# 建立并切换到新分支
git checkout -b <branch>
```

### 合并分支

```bash
# 将指定分支合并到当前分支
git merge <branch>
```

### 删除分支

```bash
git brach -d <branch>
```

### 分支管理

```bash
# 显示分支状态信息
git branch -v
# 显示已合并或未合并分支
git branch --merged # --no-merged
```

## git reset
撤销提交记录（之后的提交记录全部没有了）

```bash
# 取消暂存的文件
git reset HEAD <file>

git reset --hard <hash>
git reset --hard HEAD~1
git push -f
```

## git revert
撤销提交记录（推荐使用）

```bash
# 撤销提交
git revert HEAD~1
```

## git rebase
改变当前分支的 base

```bash
# 以交互方式执行
git rebase --interactive HEAD~3
```

## git cherry-pick

```bash
git cherry-pick <commit1> <commit2> ....
```


## 高级议题
### 管理 HEAD

```bash
# 将 HEAD 移动到某次提交（HEAD 进入 detached 状态）
# 高版本建议 git restore
git checkout <commit_hash>
# 移动 HEAD
git checkout HEAD^
git checkout HEAD~

# 将 main 分支强制指向 HEAD 的第 3 级 parent 提交
git branch -f main HEAD~3

# 撤销对文件的修改
git checkout -- HEAD <file>

# 向上移动两个提交
git checkout main^^
# 向上移动三个提交
git checkout main~3
# 也可以用 HEAD
git checkout HEAD^
```

### push 到 private repo

+ 在 github 进入 `settings->Developer settings->Personal access tokens` 生成 _token_
+ 在本地 repo 中设置连接并 push

```bash
git remote add origin https://<token>@github.com/user/repo.git
git remote -v
git push -u origin master
```

## 清除 reflog 历史

```bash
# 删除 reflog 历史
git reflog expire --expire=now --all
```

## git 学习资料
+ [git pro 电子书](https://git-scm.com/book/zh/v2)
+ [https://learngitbranching.js.org/?locale=zh_CN](https://learngitbranching.js.org/?locale=zh_CN)
+ [git log 命令详解](https://blog.csdn.net/wzt001005/article/details/145754205)
