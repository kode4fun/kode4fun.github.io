---
title:      Learn git branching 学习笔记
description: git 学习记录
date:       2026-03-24 00:00:00+0800
author:     kode4fun
categories: [tech,git]
tags:
    - git
---

## 基础篇
### git commit

| 命令                                        | 说明                                 |
| ------------------------------------------- | ------------------------------------ |
| `git commit -m "提交信息"`                  | 基础提交                             |
| `git commit -m "简短摘要" -m "详细描述"`    | 简短提交信息和详细描述               |
| `git commit --amend`                        | 修改最近一次提交（会修改 hash 值）   |
| `git commit --amend -m "新的提交信息"`      | 修改最近一次提交的信息               |
| `git commit --amend --author="姓名 <邮箱>"` | 修改最近一次提交的作者               |
| `git commit --amend --no-edit`              | 修改最近一次提交内容但保持提交信息   |
| `git commit -a -m "信息"`                   | 暂存已跟踪文件的修改和删除，直接提交 |
| `git commit -am "信息"`                     | 同上，简写形式                       |
| `git commit --allow-empty -m "空提交"`      | 允许空提交（没有文件变化）           |
| `git commit --allow-empty-message -m ""`    | 允许空提交信息                       |
| `git commit --no-verify -m "信息"`          | 跳过 pre-commit 钩子                 |

### git branch

| 命令                              | 说明                             |
| --------------------------------- | -------------------------------- |
| `git branch bugFix`               | 建立 bugFix 分支(不进行分支切换) |
| `git branch -m 新分支名`          | 当前分支重命名                   |
| `git branch -m 旧分支名 新分支名` | 其他分支重命名                   |
| `git branch -d 分支名`            | 删除已合并分支                   |
| `git branch -D 分支名`            | 强制删除分支，未合并也删         |

### git checkout

注意：`git checkout` 后如果是分支名，不会导致 HEAD 分离，如果是 _hash_ 会导致 HEAD 分离。

| 命令                                   | 说明                                 |
| -------------------------------------- | ------------------------------------ |
| `git checkout bugFix`                  | 切换到 bugFix 分支                   |
| `git checkout -b bugFix`               | 建立并切换分支                       |
| `git checkout -B bugFix`               | 建立或重置分支并切换（若分支已存在） |
| `git checkout --track origin/bugFix`   | 跟踪远程分支并切换                   |
| `git checkout -b bugFix origin/bugFix` | 基于远程分支创建本地分支             |
| `git checkout -`                       | 切换到上一个分支                     |
| `git checkout -- 文件名`               | 撤销工作区文件修改                   |
| `git checkout HEAD -- 文件名`          | 从暂存区撤销文件修改                 |
| `git checkout 提交hash -- 文件名`      | 恢复特定提交中的文件版本             |
| `git checkout -p`                      | 交互式选择要撤销的修改（文件块）     |

### git merge
在 git 中合并两个分支时会产生一个特殊的提交记录，它有两个 parent 节点。

| 命令                           | 说明                                     |
| ------------------------------ | ---------------------------------------- |
| `git merge bugFix`             | 将 bugFix 合并到当前分支                 |
| `git merge --abort`            | 取消合并操作                             |
| `git merge --no-ff bugFix`     | 创建合并提交，即使可以快速前进           |
| `git merge --ff-only bugFix`   | 仅进行快速前进（如果不能快速前进则失败） |
| `git merge --no-commit bugFix` | 不自动创建合并提交，需要手动 git commit  |
| `git merge -X ours bugFix`     | 冲突时使用本地版本                       |
| `git merge -X theirs bugFix`   | 冲突时使用合并分支版本                   |

- 使用场景：
  - **快速前进合并**：当前分支没有新提交时，直接将分支指针移到被合并分支的最新提交
  - **真实合并**：使用 `--no-ff` 保留合并历史，便于追踪分支演变过程
  - **处理冲突**：合并时出现冲突，可用 `-X` 选项自动解决策略

### git rebase
`git rebase` 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去。

rebase 的优势就是可以创造更线性的提交历史，这听上去有些难以理解。如果只允许使用 rebase 的话，代码库的提交历史将会变得异常清晰。

```bash
# 将 A 作为 B 的基准，会自动切换到 B 分支
git rebase A B
# 将 A 作为当前分支的基准
git rebase A
```

## 高级篇
### 分离 HEAD
HEAD 是当前所在提交记录的符号名称 —— 它本质上标识着你正在其上工作的那个提交。

HEAD 总是指向当前分支上最近一次提交记录。大多数修改提交树的 Git 命令都是从改变 HEAD 的指向开始的。

分离 HEAD 就是让其指向一个具体的提交，而不是某个分支。
注意：当 `git checkout` 的参数时分支名时，不会分离，如果是提交 hash 则会分离。

```bash
git checkout C1
```

### 相对引用
- 使用 `^` 向上移动 1 个提交记录
- 使用 `~<num>` 向上移动多个提交记录，如 `~3`

```bash
# HEAD 移动到 bugFix 的 parent 的 parent
git checkout bugFix^^
```

### 强制修改分支位置
可以直接使用 `git branch -f` 选项让分支指向另一个提交（并切换到该分支）。

```bash
# 将 main 分支强制指向 HEAD 的第 3 级 parent 并切换
git branch -f main HEAD~3
```

### 撤销变更
撤销变更也由底层部分（暂存单个文件或代码块）和上层部分（变更实际如何被撤销）组成。我们的应用将聚焦于后者。

#### git reset
`git reset` 通过将分支引用向后移动至一个更早的提交来撤销变更。从这个意义上说，你可以把它理解为“改写历史”。`git reset` 会把分支向上移动，就好像之前指向的提交从未发生过一样。

```bash
# 当前在 main 分支的 C2 提交
# 下面命令将 main 分支移回到了 C1；现在我们的本地仓库处于一种仿佛 C2 从未发生过的状态。
# 注：在reset后， C2 所做的变更还在，但是处于未加入暂存区状态
# 这种“改写历史”的方法对大家一起使用的远程分支是无效

git reset HEAD~1
```
#### git revert
为了撤销更改并分享给别人，我们需要使用 `git revert`。

```bash
git revert HEAD
```

## 移动提交记录
### git cherry-pick
将一些提交复制到当前所在的位置（HEAD）下面。

注意：要 pick 的提交不能在 HEAD 的上游。

`git cherry-pick <提交1> <提交2> <...>` 

### 交互式 rebase
```bash
git rebase --interactive HEAD~4

#  即可撤销操作 --active 操作
git rebase --abort
```

## 杂项
### 本地栈式提交
主要是以下命令的使用：

- `git rebase -i HEAD~4`
- `git branch -f`

### 提交的技巧1
在 _newImage_ 分支上进行了一次提交，然后又基于它创建了 _caption_ 分支，然后又提交了一次。

此时你想对某个以前的提交记录进行一些小小的调整。比如设计师想修改一下 newImage 中图片的分辨率，尽管那个提交记录并不是最新的了。我们可以通过下面的方法来克服困难：

- 先用 `git rebase -i` 将提交重新排序，然后把我们想要修改的提交记录挪到最前
- 然后用 `git commit --amend` 来进行一些小修改
- 接着再用 `git rebase -i` 来将他们调回原来的顺序
- 最后我们把 _main_ 分支移到修改的最前端（用你自己喜欢的方法），就大功告成啦！

### 提交的技巧2
- git cherry-pick

### git tag
标签可以被删除后重新在另外一个位置创建同名的标签）永久地将某个特定的提交命名为里程碑，然后就可以像分支一样引用。它们并不会随着新的提交而移动。你也不能切换到某个标签上面进行修改提交，它就像是提交树上的一个锚点，标识了某个特定的位置。

```bash
git tag V1 C1
```

### git describe

```bash
git describe <ref>
```

## 高级话题
### 多次 rebase

| 命令                        | 说明                                |
| --------------------------- | ----------------------------------- |
| `git rebase main bugFix`    | 将 bugFix 分支变基到 main 分支上    |
| `git rebase bugFix side`    | 将 side 分支变基到 bugFix 分支上    |
| `git rebase bugFix another` | 将 another 分支变基到 bugFix 分支上 |
| `git rebase another main`   | 将 main 分支变基到 another 分支上   |

### 两个 parent 节点
### 纠缠不清的分支

## Push & Pull —— Git 远程仓库
### git clone
远程分支有一个特别的属性，在你切换到远程分支时，自动进入分离 HEAD 状态。Git 这么做是出于不能直接在这些分支上进行操作的原因, 你必须在别的地方完成你的工作, （更新了远程分支之后）再用远程分享你的工作成果。远程仓库中相应的分支更新了以后才会更新。

### git fetch
- 从远程仓库下载本地仓库中缺失的提交记录
- 更新远程分支指针（如 o/main）
- `git fetch` 实际上将本地仓库中的远程分支更新成了远程仓库相应分支最新的状态。
- `git fetch` 并不会改变你本地仓库的状态。它不会更新你的 main 分支，也不会修改你磁盘上的文件

### git pull
其实有很多方法的 —— 当远程分支中有新的提交时，你可以像合并本地分支那样来合并远程分支。也就是说就是你可以执行以下命令:

- git cherry-pick o/main
- git rebase o/main
- git merge o/main

实际上，由于先抓取更新再合并到本地分支这个流程很常用，因此 Git 提供了一个专门的命令来完成这两个操作。它就是我们要讲的 `git pull`。

`git pull` 就是 `git fetch` 和 `git merge` 的缩写！

### git push
```bash
# 推送新分支
git push -u origin 新分支名
# 删除远程旧分支
git push origin :旧分支名
```

### 偏离的提交历史
当远程仓库改变时，本地的 _origin/main_ 与远程 _main_ 不一致造成无法 push。

```bash
# 方法一
git fetch
git rebase origin/main # 或 git merge origin/main
git push

# 方法二
git pull # 或 git pull --rebase
```

###
```bash
# 将 main 分支 指向 origin/main 位置
git branch -f main o/main
# 以 c2 提交建立并切换到 feature 分支
git checkout -b feature C2
# 推送 feature 分支
git push origin feature
```

### 锁定的 main 分支
新建一个分支feature, 推送到远程服务器. 然后reset你的main分支和远程服务器保持一致, 否则下次你pull并且他人的提交和你冲突的时候就会有问题.

## 关于 origin 和它的周边 —— Git 远程仓库高级操作

## 参考资源
- [https://learngitbranching.js.org/?locale=zh_CN](https://learngitbranching.js.org/?locale=zh_CN)
- [Git学习01-Learn Git Branching(在线学习工具)](https://cloud.tencent.com/developer/article/1641410)
