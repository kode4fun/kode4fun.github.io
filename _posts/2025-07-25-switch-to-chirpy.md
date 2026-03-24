---
title: 博客升级到 chirpy theme
description: chirpy theme 确实很强大！
author: kode4fun
date: 2025-07-25 00:00:00 +0800
categories: [Blogging]
tags: [favicon]
---

### 修改模板
使用如下命令克隆 repo 到本地

```bash
git clone https://github.com/cotes2020/chirpy-starter.git
```

修改 `_config.yml` 文件内的配置信息，主要有
+ `lang` 修改为 `zh-CN`
+ `url` 修改为 `https://<username>.github.io`
+ `title` `description` `tagline`
+ `avatar`

### 本地预览
> 本地预览需先安装好 `Docker Desktop` 和 `VS Code`
{: .prompt-tip }

+ VS Code 安装 `Dev Contaivers` 插件
+ 打开目录，根据提示下载 Docker 镜像
+ 容器启动后，可以在 VS Code 终端执行
  
```bash
bundle install
bundle exec jekyll serve --force_polling
```

或者在 windows 终端执行

```bash
docker exec -it <container_name> bash
cd /workspaces/kode4fun.github.io
bundle install
bundle exec jekyll serve -force_polling
```

### 部署到 github

> 需要先删除默认的 `.github/workflows/pages-deploy.yml` 文件中 `branchs` 节多余的 `main` 分支
{: .prompt-tip }

+ 建立名字为 `<username>.github.io` 的 repo
+ 进入 `Settings->Pages`，`source` 中选择 `Github Actions`
+ push 到 github

### 参考资料
+ [chirpy 官网](https://chirpy.cotes.page/posts/getting-started/)
+ [Jekyll模板升级笔记](https://kukisama.github.io/JekyllfastStart/)
