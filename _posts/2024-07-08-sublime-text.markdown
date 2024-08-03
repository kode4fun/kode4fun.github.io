---
layout:     post
title:      "Sublime Text 配置和使用"
subtitle:   "技巧和快捷键"
date:       2024-07-08 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - Sublime
    - Regular Expressions
---

> Sublime Text 配置和使用技巧

## 安装和设置
### 基本设置
在 `首选项` => `设置` 添加如下内容

```javascript
{
    "ignored_packages":
    [
        "Vintage",
    ],
    "draw_white_space":"all", // 始终显示空格
    "color_scheme": "Monokai.sublime-color-scheme",
    "check_update":false,
    "update_check":false,
    "hot_exit": false, // ??
    "remember_open_files":false,
}
```
### 禁止插件自动更新
在 `首选项` => `Package Settings` => `Package Controls` => `Settings`添加如下内容

```javascript
    "auto_upgrade":false //禁止启动时插件更新
```

### 配置 Python 编译环境
在 `工具` => `编译系统` => `新建编译系统` 写入以下内容

```javascript
{
    "cmd": ["python3", "-u", "$file"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "selector": "source.python",
    "env": {"PYTHONIOENCODING": "utf-8"},
    "windows": {
        "cmd": ["py", "-u", "$file"],
    },
    "variants":
    [
        {
            "name": "python3.8(cmd)",
            "shell_cmd": "start cmd /c \"python -u \"${file_name}\" & pause\"",
        }
    ]
}
```

保存到 `Data/Packages/User` 下建立 `Python3.8.sublime-build`

**`Ctrl+B`**编译，关闭窗口 **`Esc`**

## 常用快捷键
### 文本操作快捷键

| 快捷键 | 说明 |
| :--: | :--: |
| **`Ctrl + Shift + Enter`** | 在当前行前插入一行 |
| **`Ctrl + Enter`** | 在当前行后插入一行 |
| **`Ctrl + Delete`** | 删除光标后的一个单词（以单词为单位向后删除） |
| **`Ctrl + Backspace`** | 删除光标前的一个单词（以单词为单位向前） |
| **`Ctrl + Shift + K`** | 删除当前行 |
| **`Ctrl + K`** | 删除至 End |
| **`Ctrl + T`** | 逐个单词向前移位 |


### 行操作快捷键

| 快捷键 | 说明 | 快捷键 | 说明 |
| :--: | :--: | :--: | :--: |
| **`Ctrl + ]`** | 增加缩进 | **`Ctrl + shift + D`** | 复制当前行 |
| **`Ctrl + [`** | 减小缩进 | **`Ctrl + shift + K`** | 删除当前行 |
| **`Ctrl + L`** | 选择当前行 | **`Ctrl + shift + ↑/↓`** | 当前行与上/下行交换位置 |
|   **`Ctrl + Shift + J`**  | 合并多行 | **`Ctrl + Shift + L`** | 同时编辑多行 |

### 其它快捷键

| 快捷键 | 说明 | 快捷键 | 说明 |
| :--: | :--: | :--: | :--: |
| **`Alt + Shift + n`** | 分屏显示 | **`Ctrl + Shift + T`** | 打开已关闭的窗口 |
| **`Ctrl + /`** | 注释当前行 | **`Ctrl + Shift + /`** | 选中多行（块）进行注释 |
| **`Ctrl + Shift + [`** | 折叠代码  | **`Ctrl + Shift + ]`** | 展开代码  |


## 插件
### 常用插件

|       插件名称       |   作用   |
| :------------------: | :------: |
| ChineseLocalizations |   汉化   |
|        Emmet         | HTML/CSS |
|   MarkdownPreview    | 预览 markdown |
|   Pretty JSON    | 格式化 json |

### 设置**MarkdownPreview**支持快捷键
#### 设置快捷键
`首选项` => `快捷键设置`

```javascript
[
    {
        "keys": ["alt+m"],
        "command": "markdown_preview",
        "args": 
        {
            "target": "browser",
            "parser":"markdown"
        }
    }
]
```

#### Markdown Preview 的配置选项

```javascript
// 配置参考 https://facelessuser.github.io/MarkdownPreview/
{
    "path_tempfile": ".", // 临时文件路径就在当前目录
    "src_name_as_tmpfile_name":true, // 设置此项使临时文件与源文件同名
    // 需要修改原包实现
    "image_path_conversion": "relative",
    "file_path_conversions": "relative",
    "js": {
        // "https://unpkg.com/mermaid/dist/mermaid.min.js",//此版本不行
        // "https://unpkg.com/mermaid@8.8.4/dist/mermaid.min.js",
        // User configuration, should be loaded before the loader 
        // "res://MarkdownPreview/js/mermaid_config.js", 
        // "res://MarkdownPreview/js/mermaid.js" // Mermaid loader 
    },
    "pygments_style": "github2014"
}
```

#### 修改 **MarkdownPreview** 生成的预览文件名
`首选项` =>`Package Settings` => `Markdown Preview` => `Settings`

```javascript
{
    "path_tempfile": "./",
    "image_path_conversion": "none",
    "file_path_conversions": "none",
    "filename_tempfile":"SAME_LIKE_ORIGIN" // 自定义的生成原文件名.html
}
```

拷贝 `Data/Installed Packages/MarkdownPreview.sublime-package` 解压缩后修改 `markdown_preview.py`


```python
tmp_filename = '%s.html' % view.id()

# added by fish 20211124  ------------begin

# 临时文件名与原来文件名一致

# print('Sublime 所使用 Python 版本 %s' % sys.version)

if settings.get('filename_tempfile') :
	fn = settings.get('filename_tempfile')
	# 和原来文件名相同

	if fn.upper() == 'SAME_LIKE_ORIGIN' :
		tmp_filename = '%s.html' % os.path.splitext(view.file_name())[0]
	else:
		tmp_filename = '%s.html' % fn
# added by fish 20211124  ------------end

if settings.get('path_tempfile'):
```


#### 设置 Pretty JSON
安装后在`首选项` => `快捷键`设置中添加

```json
    {
        "keys":["Alt+Ctrl+j"],
        "command":"pretty_json",
        "args": {}
    }
```


## 正则表达式

+ 删除重复行 **`(^.*\n)(?=\1)`**
+ 删除空行 **`^[ \t]*\n`**


## 高级技巧
### `.sublime-package` 文件
本质是 `.zip`压缩包，在 `Data/Packages` 和 `Data/Installed Packages` 下


### 扩展启用 Python 3.8
+ 在扩展的根目录下添加文件 **`.python-version`**，内容为 **`3.8`**
+ 拷贝 `Data/Installed Packages/0_package_control_loader.sublime-package` 为 `Data/Installed Packages/1_package_control_loader.sublime-package`
+ 将`1_package_control_loader.sublime-package` 根目录下添加上述的`.python-version`文件
+ 启动时`1_package_control_loader.sublime-package`会自动加载到 Python 3.8 的 host 空间

## 插件开发
+ API 参考 https://www.sublimetext.com/docs/api_reference.html#module-sublime
```python
        # 显示窗口相关 API
        sublime.message_dialog
        sublime.ok_cancel_dialog
        sublime.open_dialog
        sublime.save_dialog
        sublime.select_folder_dialog
        sublime.yes_no_cancel_dialog
```