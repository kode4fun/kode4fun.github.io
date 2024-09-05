---
layout:     post
title:      "Python turtle 接口参考"
subtitle:   ""
date:       2024-09-04 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - Python
    - turtle
---

> Python turtle 接口参考

更多细节见 [Python Documention](https://docs.python.org/zh-cn/3/library/turtle.html) 文档

## 海龟动作
### 移动和绘制

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
|`forward(distance)` / `fd` | 前进 | |
|`backward(distance)`/ `bk` / `back` | 后退 | |
|`right(angle)` / `rt` | 右转 | |
|`left(angle)` / `lt` | 左转 | |
|`goto(x, y=None)` / `setpos` / `setposition` | 前往/定位 | |
| `teleport(x, y=None, *, fill_gap=False)` | 将海龟移到某个绝对位置 | |
|`setx(x)` | 设置`x`坐标 | |
|`sety(y)` | 设置`y`坐标 | |
|`setheading(to_angle)` / `seth()` | 设置朝向 | |
|`home()` | 返回原点 |
|`circle(radius, extent=None, steps=None)` | 画圆 | |
|`dot(size=None, *color)` | 画点 | |
|`stamp()` | 印章 | |
|`clearstamp()` | 清除印章 | |
|`clearstamps()` | 清除多个印章 | |
|`undo()` | 撤消 | |
|`speed(speed=None)` | 速度 | 取值 0-10 或 字符串 |

### 获取海龟的状态

| 函数名 | 别名 | 作用 | 备注 |
| :---: | :---: | :---: | :---: |
| `position()` | `pos` | 位置 | |
| `towards(x, y=None)` | | 目标方向 | |
| `xcor()` | | `x`坐标 | |
| `ycor()` | | `y`坐标 | |
| `heading()` | | 朝向 | |
| `distance(x, y=None)` | | 距离 | |

## 设置与度量单位

| 函数名 | 别名 | 作用 | 备注 |
| :---: | :---: | :---: | :---: |
| `degrees(fullcircle=360.0)` | | 角度 | |
| `radians()` | | 弧度 | |

## 画笔控制
### 绘图状态

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
| `pendown()` / `pd` / `down` | 画笔落下 | |
| `penup()` / `pu` /`up` | 画笔抬起 | |
| `pensize(width=None)` / `width` | 画笔粗细 | |
| `pen(pen=None, **pendict)` | 画笔 | |
| `isdown()` | 画笔是否落下 | |

### 颜色控制

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
| `color(*args)` | 返回或设置画笔颜色和填充颜色 | |
| `pencolor(*args)` | 画笔颜色 | |
| `fillcolor(*args)` | 填充颜色 | |

### 填充

| 函数名 | 别名 | 作用 | 备注 |
| :---: | :---: | :---: | :---: |
| `filling()` | | 是否填充 | |
| `begin_fill()` | | 开始填充 | |
| `end_fill()` | | 结束填充 | |

### 更多绘图控制

| 函数名 | 别名 | 作用 | 备注 |
| :---: | :---: | :---: | :---: |
| `reset()` | | 重置 | |
| `clear()` | | 清空 | |
| `write()` | | 书写 | |

## 海龟状态
### 可见性

| 函数名 | 别名 | 作用 | 备注 |
| :---: | :---: | :---: | :---: |
| `showturtle()` | `st` | 显示海龟 | |
| `hideturtle()` | `ht` | 隐藏海龟 | |
| `isvisible()` | | 是否可见 | |

### 外观

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
| `shape(name=None)` | | 形状 | |
| `resizemode(rmode=None)` | | 大小调整模式 | _auto_、_user_、_noresize_ |
| `shapesize(stretch_wid=None, stretch_len=None, outline=None)` / `turtlesize` | 形状大小 | |
| `shearfactor(shear=None)` | 剪切因子 | |
| `settiltangle(angle)` | 设置倾角 | |
| `tiltangle(angle=None)` | 倾角 | |
| `tilt(angle)` | 倾斜 | |
| `shapetransform(t11=None, t12=None, t21=None, t22=None)` | 变形 | |
| `get_shapepoly()` | 获取形状多边形 | |

### 使用事件

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
| `onclick(fun, btn=1, add=None)` | 当鼠标点击 | |
| `onrelease(fun, btn=1, add=None)` | 当鼠标释放 | |
| `ondrag(fun, btn=1, add=None)` | 当鼠标拖动 | |

### 特殊海龟方法

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
| `begin_poly()` | 开始记录多边形 | |
| `end_poly()` | 结束记录多边形 | |
| `get_poly()` | 获取多边形 | |
| `clone()` | 克隆 | |
| `getturtle()` | `getpen` | 获取海龟画笔 | |
| `getscreen()` | 获取屏幕 | |
| `setundobuffer(size)` | 设置撤消缓冲区 | |
| `undobufferentries()` | 撤消缓冲区条目数 | |
| `TurtleScreen()` / `Screen` | 方法 | |


### 窗口控制

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
| `bgcolor(*args)` | 背景颜色 | |
| `turtle.bgpic(picname=None)` | 背景图片 | 值 `nopic` 为取消|
| `clearscreen()` | | |
| `resetscreen()` | | |
| `screensize(canvwidth=None, canvheight=None, bg=None)` | 屏幕大小 | |
| `setworldcoordinates(llx, lly, urx, ury)` | 设置世界坐标系 | |

## 动画控制

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: | :---: |
| `delay(delay=None)` | 延迟 | 单位为毫秒 |
| `tracer(n=None, delay=None)` | 追踪 | |
| `update()` | 更新 | |

## 使用屏幕事件

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: | :---: |
| `listen(xdummy=None, ydummy=None)` | 监听 | |
| `onkey()`/`onkeyreleaseo(fun, key)` | 当键盘按下并释放 | |
| `onkeypress(fun, key=None)` | 当键盘按下 | |
| `onclick()`/`onscreenclick(fun, btn=1, add=None)` | 当点击屏幕 | |
| `ontimer(fun, t=0)` | 当达到定时 | |
| `mainloop()` | `done` | 主循环 | |

## 设置与特殊方法

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
| `mode(mode=None)` | | 取值 _standard_、_logo_、_world_|
| `colormode(cmode=None)` | 颜色模式 | 可用值 1 或 255 |
| `getcanvas()` | 获取画布 | |
| `getshapes()` | 获取形状 | |
| `register_shape()` / `addshape` | 添加形状 |
| `turtles()` | 所有海龟 | |
| `window_height()` | 窗口高度 | |
| `window_width()` | 窗口宽度 | |

## 输入方法

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
| `textinput(title, prompt)` | 文本输入 | |
| `numinput(title, prompt, default=None, minval=None, maxval=None)` | 数字输入 | |

### `Screen` 专有方法

| 函数名 | 作用 | 备注 |
| :---: | :---: | :---: |
| `bye()` | 退出 | |
| `exitonclick()` | 当点击时退出 | |
| `setup()` | 设置 | |
| `title(titlestring)` | 绘图窗口标题 | |