---
title:      Python turtle 接口参考
date:       2024-09-04 00:00:00+0800
author:     kode4fun
categories: [tech,python]
tags:
    - python
    - turtle
---

> Python turtle API 参考

更多细节见 [Python turtle documentation](https://docs.python.org/zh-cn/3/library/turtle.html) 文档


## 绘图窗口设置

```python
turtle.title('A python turtle demo')
turtle.setup(width=0.5, height=0.75, startx=None, starty=None)
```

## 画布设置
```python
turtle.screensize(canvwidth=None, canvheight=None, bg=None)
```

### 画布背景图片
```python
# 背景图片，必须为 _gif_ 格式，当 nopic 为 None 时取消背景图
turtle.bgpic(picname=None)
```

## 绘制文字
```python
write(arg, move=False, align='left', font=('Arial', 8, 'normal'))
```

## 海龟动作
### 移动和绘制

|                        函数                         |                    说明                    |
| :-------------------------------------------------: | :----------------------------------------: |
|          `turtle.forward(distance)` / `fd`          |                    前进                    |
|     `turtle.backward(distance)`/ `bk` / `back`      |                    后退                    |
|                  `turtle.setx(x)`                   |                设置`x`坐标                 |
|                  `turtle.sety(y)`                   |                设置`y`坐标                 |
| `turtle.goto(x, y=None)` / `setpos` / `setposition` |                 前往/定位                  |
|   `turtle.teleport(x, y=None, *, fill_gap=False)`   |           将海龟移到某个绝对位置           |
|                   `turtle.home()`                   |                  返回原点                  |
|            `turtle.right(angle)` / `rt`             |                    右转                    |
|             `turtle.left(angle)` / `lt`             |                    左转                    |
|      `turtle.setheading(to_angle)` / `seth()`       |                  设置朝向                  |
|           `turtle.dot(size=None, *color)`           |                    画点                    |
|  `turtle.circle(radius, extent=None, steps=None)`   |                    画圆                    |
|                  `turtle.stamp()`                   |                    印章                    |
|                `turtle.clearstamp()`                |                  清除印章                  |
|               `turtle.clearstamps()`                |                清除多个印章                |
|                   `turtle.undo()`                   |                    撤消                    |
|             `turtle.speed(speed=None)`              | 速度(值 _1-10_ 时数字越大越快，**0** 最快) |

### 获取海龟的状态

|             函数             |               说明                |
| :--------------------------: | :-------------------------------: |
| `turtle.position()` / `pos`  |     返回海龟位置位置 _(x,y)_      |
|    `turtle.xcor()` / `x`     |           返回 _x_ 坐标           |
|    `turtle.ycor()` / `y`     |           返回 _y_ 坐标           |
|      `turtle.heading()`      |          返回海龟的朝向           |
| `turtle.distance(x, y=None)` |   返回当前位置到 _(x,y)_ 的距离   |
| `turtle.towards(x, y=None)`  | 返回当前位置与 _(x,y)_ 连线的方向 |

## 角度度量单位

|                函数                |        说明        |
| :--------------------------------: | :----------------: |
| `turtle.degrees(fullcircle=360.0)` |  设置角度单位为度  |
|         `turtle.radians()`         | 设置角度单位为弧度 |

## 画笔控制
### 绘图状态

|                  函数                  |        说明        |
| :------------------------------------: | :----------------: |
|   `turtle.pendown()` / `pd` / `down`   |      画笔落下      |
|     `turtle.penup()` / `pu` / `up`     |      画笔抬起      |
| `turtle.pensize(width=None)` / `width` | 设置或返回画笔粗细 |
|   `turtle.pen(pen=None, **pendict)`    | 设置或返回画笔对象 |
|           `turtle.isdown()`            |    画笔是否落下    |

### 颜色控制
_turtle_ 中，颜色值可以是颜色名字符串或 _(r,g,b)_ 元组，其中r、g、b 分量介于 _0 ~ 1_

|           函数            |             说明             |
| :-----------------------: | :--------------------------: |
|   `turtle.color(*args)`   | 返回或设置画笔颜色和填充颜色 |
| `turtle.pencolor(*args)`  |           画笔颜色           |
| `turtle.fillcolor(*args)` |           填充颜色           |

### 填充

|         函数          |         说明         |
| :-------------------: | :------------------: |
| `turtle.begin_fill()` |       开始填充       |
|  `turtle.end_fill()`  |       结束填充       |
|  `turtle.filling()`   | 返回是否处于填充状态 |

### 更多绘图控制

|       函数        |              说明              |
| :---------------: | :----------------------------: |
| `turltle.reset()` |  请空所绘图形并重置为初始状态  |
| `turltle.clear()` | 清空所绘制图形，但是不设置海龟 |

## 海龟状态
### 可见性

|             函数             |       说明       |
| :--------------------------: | :--------------: |
| `turtle.showturtle()` / `st` |     显示海龟     |
| `turtle.hideturtle()` / `ht` |     隐藏海龟     |
|     `turtle.isvisible()`     | 返回海龟是否可见 |

### 外观

|                                     函数                                     |                     说明                      |
| :--------------------------------------------------------------------------: | :-------------------------------------------: |
|                              `shape(name=None)`                              |                     形状                      |
|                           `resizemode(rmode=None)`                           | 大小调整模式，取值 _auto_、_user_、_noresize_ |
| `shapesize(stretch_wid=None, stretch_len=None, outline=None)` / `turtlesize` |                   形状大小                    |
|                          `shearfactor(shear=None)`                           |                   剪切因子                    |
|                            `settiltangle(angle)`                             |                   设置倾角                    |
|                           `tiltangle(angle=None)`                            |                     倾角                      |
|                                `tilt(angle)`                                 |                     倾斜                      |
|           `shapetransform(t11=None, t12=None, t21=None, t22=None)`           |                     变形                      |
|                              `get_shapepoly()`                               |                获取形状多边形                 |

### 海龟事件
|                   函数                   |   说明   |
| :--------------------------------------: | :------: |
| `turtle.onrelease(fun, btn=1, add=None)` | 鼠标释放 |
|  `turtle.ondrag(fun, btn=1, add=None)`   | 鼠标拖动 |

### 屏幕事件

|                           函数                           |           说明            |
| :------------------------------------------------------: | :-----------------------: |
|        `turtle.listen(xdummy=None, ydummy=None)`         |       监听键盘事件        |
|        `turtle.onkey()`/`onkeyreleaseo(fun, key)`        |     当键盘按下并释放      |
|            `turtle.onkeypress(fun, key=None)`            |        当键盘按下         |
| `turtle.onscreenclick(fun, btn=1, add=None)` / `onclick` |         鼠标单击          |
|                   `ontimer(fun, t=0)`                    | 计时器事件（单位为 _ms_） |

### 特殊海龟方法

|              函数               |       说明       |
| :-----------------------------: | :--------------: |
|      `turtle.begin_poly()`      |  开始记录多边形  |
|       `turtle.end_poly()`       |  结束记录多边形  |
|       `turtle.get_poly()`       |    获取多边形    |
|        `turtle.clone()`         |       克隆       |
| `turtle.getturtle()` / `getpen` |   获取海龟画笔   |
|      `turtle.getscreen()`       |     获取屏幕     |
|  `turtle.setundobuffer(size)`   |  设置撤消缓冲区  |
|  `turtle.undobufferentries()`   | 撤消缓冲区条目数 |


### 窗口控制

|                   函数                    |      说明      |
| :---------------------------------------: | :------------: |
|              `clearscreen()`              |  重置所有海龟  |
|              `resetscreen()`              |  重置所有海龟  |
| `setworldcoordinates(llx, lly, urx, ury)` | 设置世界坐标系 |

## 动画控制

|                函数                 |          说明          |
| :---------------------------------: | :--------------------: |
|     `turtle.delay(delay=None)`      | 延迟（单位为**毫秒**） |
| `turtle.tracer(n=None, delay=None)` |          追踪          |
|          `turtle.update()`          |          更新          |


## 设置与特殊方法

|              函数               |               说明               |
| :-----------------------------: | :------------------------------: |
|        `mode(mode=None)`        | 取值 _standard_、_logo_、_world_ |
|     `colormode(cmode=None)`     |    颜色模式，取值 1.0 或 255     |
|          `getcanvas()`          |             获取画布             |
|          `getshapes()`          |             获取形状             |
| `register_shape()` / `addshape` |             添加形状             |
|           `turtles()`           |             所有海龟             |
|        `window_height()`        |             窗口高度             |
|        `window_width()`         |             窗口宽度             |

## 输入方法

|                               函数                                |   说明   |
| :---------------------------------------------------------------: | :------: |
|                 `turtle.textinput(title, prompt)`                 | 文本输入 |
| `numinput(title, prompt, default=None, minval=None, maxval=None)` | 数字输入 |

## `Screen` 类及其方法
_Screen_ 类继承于 _TurtleScreen_ 类，_TurtleScreen_ 又继承自 _Tkinter_ 的 _Canvas_ 类，而 _Canvas_ 表示绘图区域。

### 获取 Screen 实例

```python
# 直接使用Screen类
from turtle import Screen
s = Screen()

# 使用Turtle实例的 Screen 方法
from turtle import Turtle
t = Turtle()
s = t.Screen()
turtle.TurtleScreen()
```

### Screen 方法

|        函数名        |     作用     | 备注  |
| :------------------: | :----------: | :---: |
|       `bye()`        | 关闭绘图窗口 |       |
|   `exitonclick()`    | 当点击时退出 |       |
| `title(titlestring)` | 绘图窗口标题 |       |


### 参考资料
+ [turtle 官方文档](https://docs.python.org/zh-cn/3/library/turtle.html#turtle.ontimer)
+ [turtle 方法汇总](https://www.jianshu.com/p/7bdb648e91a7)
+ [python 怎么让图随画布等比例变化 python画布大小设置](https://blog.51cto.com/u_16099170/6512938)
+ [Python标准库 - turtle (2)用Screen的方法控制画布](https://blog.csdn.net/qq_42783188/article/details/143205546)
