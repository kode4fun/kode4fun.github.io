---
title:   常用 Windows 批处理脚本
description: windows 批处理示例
date:     2025-08-22 00:00:00+0800
author:   kode4fun
categories: [tech,batch script]
tags:
  - snippets
  - batch script
---

## 批量重命名文件

```shell
@echo off
@chcp 65001 2>nul 1>nul
setlocal enabledelayedexpansion

if "%~1" equ "" (
  call :rename "txt" "md"
) else (
  call :rename "tmp" "md"
)

exit /b

:rename
  FOR /r %%i IN (*) DO (
    rem echo %%i,%%~xi
    if "%%~xi" equ ".%~1" (
      echo %%~fi --^> %%~ni.%~2
      rename "%%~fi"  "%%~ni.%~2"
    )
  )
  exit /b
```

## 获取字符串长度

```shell
@echo off
setlocal ENABLEDELAYEDEXPANSION
chcp 65001 1>nul 2>nul

set /p str=请输入任意长度字符串：
if not defined str goto :eof
echo 您输入了：%str%
call :getStrLen str
goto :eof

:getStrLen
  set x=0
  set tmpStr=!%1!
  :label
    set /a x+=1
    set tmpStr=%tmpStr:~0,-1%
    if defined tmpStr goto :label
    echo 字符串长度：%x%
  goto :eof

:eof
```
