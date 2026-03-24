---
title:      使用 jekyll+huxblog 搭建博客系统
description: 搭建基于 jekyll+huxblog 的博客系统，并使用 Docker 实现预览
date:       2024-07-02 00:00:00+0800
author:     kode4fun
categories: [Blogging]
tags: [Docker,jekyll]
---


### 克隆 huxpro 主题

```bash
git clone --branch master --depth 1 git@github.com:Huxpro/huxblog-boilerplate.git kode4fun.github.io
```

克隆后，可以删除 `kode4fun.github.io` 文件夹中的 _.git_ 目录

### 启动预览

```bash
docker run --name kode4fun -d --rm -v /f/scripts/github/kode4fun.github.io/:/usr/src/app -p 4000:4000 starefossen/github-pages
docker exec kode4fun jekyll build
```

第一次运行时会自动下载 `starefossen/github-pages` 的 Docker 镜像

### 修改主题设置
+ 编辑 `_confim.yml` 文件
+ 根据提示修正编译时 `_layouts` 下模板文件的错误
    + `if` 语句内的变量多余 `\{\{` 和 `\}\}`
    + `if` 中的 `&&` 替换为 `and`
+ 去除 `anchorjs` 的超链接多余 **#** 符号
    + 修改 `_layouts/post.html` 中的 `icon: ''` 为空
+ 查看网络请求，将部分外部资源本地化以提高加载速度

### 代码高亮和行号
+ 从 [prismjs 官网](https://prismjs.com/) 配置需要的语言并下载
    + `js` 并保存到 `js` 目录
    + `css` 并保存到 `css` 目录
+ 在 `_incudes/head.html` 中加入如下代码

```html
    <script src="/js/prism.js"></script>
    <link rel="stylesheet" href="/css/prism.css">
```

+ 在 `_includes/footer.html` 尾部中加入如下代码

```js
var pres = document.getElementsByTagName("pre");
for(var i = 0; i < pres.length; i++){
    var pre = pres[i];
    if(pre.childNodes[0].nodeName == "CODE"){
        pre.setAttribute("class","line-numbers");
    }
}
```

### less 编译
先在容器内安装 `less` 和 `clean-css`

```bash
docker exec kode4fun pwd
docker exec kode4fun ls
docker exec kode4fun node --version
docker exec kode4fun npm install -g less
docker exec kode4fun npm install -g clean-css
docker exec kode4fun npm list -g
```

编译 `less` 生成 `css`

```bash
docker exec kode4fun lessc less/hux-blog.less css/hux-blog.css
# 注意 -- 参数的传递
docker exec kode4fun lessc less/hux-blog.less css/hux-blog.min.css ----clean-css
```

>less 编译后需要重启镜像才能更新
{: .prompt-tip }

### 目录生成
+ 将 [jekyll-toc](https://github.com/allejo/jekyll-toc) 下载并复制到 `_includes_` 文件夹下
+ 在 `_layouts/post.html` 中添加 `\{\% include toc.html html=content \%\}`
+ 参照 [黄玄的博客](https://huangxuan.me/) 修改 _js_ 和 _css_
