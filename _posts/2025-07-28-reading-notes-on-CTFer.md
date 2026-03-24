---
title:      《从0 到 1 ：CTFer 成长之路》读书笔记
description: Nu1l 团队的 《从0 到 1 ：CTFer 成长之路》读书笔记
date:       2025-07-28 00:00:00+0800
author:     kode4fun
categories: [tech,security]
tags:
    - 读书笔记
    - security
---

> 《从0 到 1 ：CTFer 成长之路》 读书笔记

## 信息搜集
### .git 泄露
+ git 信息扫描 [https://github.com/denny0223/scrabble](https://github.com/denny0223/scrabble)
+ git 回滚（git reset）
+ git 分支提取 [https://github.com/WangYihang/GitHacker](https://github.com/WangYihang/GitHacker)
+ 目录扫描 [https://github.com/maurosoria/dirsearch](https://github.com/maurosoria/dirsearch)
+ 若访问 `/.git` 403 可测试 `/.git/config`

### 备份文件泄露
+ gedit 备份文件 `filename~`
+ vim 备份文件 `.filename.swp`

### banner 信息
+ CMS 指纹识别工具：Wappalyzer

## SQL 注入
##  任意文件读取
### Web 语言中的问题
+ `%00` 截断
+ `os.path.join("/a","/b")` bug

### 服务端
#### nginx 配置问题
下面的配置，访问 `/static../` 会导致目录穿越

```
location /static {
  alias /home/myapp/static/;
}
``` 
#### 软连接
上传软连接 `ln -s` ，可能导致目录穿越

### 客户端


