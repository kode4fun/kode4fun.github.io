---
title:   javascript Canvas 实现打字机效果
description: js + canvas 应用案例
date:     2025-08-08 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
---

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
  <meta charset="utf-8">
  <title>typewritter</title>
  <style type="text/css">
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    canvas#myCanvas
    {
      width:500px;
      height:300px;
      background:#eeeeee;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  打字机效果
  <div id="canvasWrapper">
    <canvas id="myCanvas"></canvas>
  </div>

<script type="text/javascript">
  function TypeWritter(canvasID,config={}) {
    this.canvas = document.querySelector(canvasID);
    this.canvas.setAttribute('width',this.canvas.offsetWidth+'px'); 
    this.canvas.setAttribute('height',this.canvas.offsetHeight+'px');
    this.ctx = this.canvas.getContext("2d");
    this.CANVAS_CONFIG = Object.assign({
      paddingX : 20, // 画布垂直边距
      paddingY : 10, // 画布水平边距
    },config);
    this.FONT_CONFIG = {
      fillStyle:'#000000',
      textBaseline:'top',// 默认为alphabetic，为便于计算字符位置做了修改
      font:'20px sens-serif'
    };
    this.V_SPACING = parseInt(parseInt(this.FONT_CONFIG.font)*0.5); // 字符垂直间距
    this.H_SPACING = parseInt(parseInt(this.FONT_CONFIG.font)*0.2); // 字符水平间距
    this.resetCursor();
  }
  TypeWritter.prototype = {
    constructor:TypeWritter,
    randomColor:()=> '#' + Math.floor(Math.random()*16777215).toString(16).padStart(6, '0'),
    clear:function () {
      this.ctx.clearRect(0,0,this.canvas.offsetWidth,this.canvas.offsetHeight);
      this.resetCursor();
      return this;
    },
    resetCursor:function(x=undefined,y=undefined) {
      const _x = (typeof x == 'undefined')?this.CANVAS_CONFIG.paddingX:x;
      const _y = (typeof y == 'undefined')?this.CANVAS_CONFIG.paddingY:y;
      this.cursor_pos = {x:_x,y:_y};
      return this;
    },
    print_char:async function (char,x=undefined,y=undefined,font_config={}) {
      Object.assign(this.ctx,this.FONT_CONFIG,font_config);
      // ctx.beginPath(); // beginPath 用于 draw 相关的图形操作对text无效
      textMetrics = this.ctx.measureText(char);
      // 未传入光标位置时，使用上次打印输出之后的位置
      x = (typeof x == 'undefined')?this.cursor_pos.x:x;
      y = (typeof y == 'undefined')?this.cursor_pos.y:y;
      const bIsLineBreak = ['\n','\r'].includes(char);// 文字中有换行标记
      //水平方向溢出
      const bIsHOverflow = (x + textMetrics.width)>(this.canvas.offsetWidth-2*this.CANVAS_CONFIG.paddingX);
      if (bIsLineBreak || bIsHOverflow) { 
        this.cursor_pos.x = this.CANVAS_CONFIG.paddingX;
        this.cursor_pos.y = y + parseInt(this.ctx.font) + this.V_SPACING;
      }
      if (bIsLineBreak) return this;
      console.log(`当前字符 ${char}, 位于 x=${x},y=${y}`); //计算位置
      this.ctx.fillText(char,this.cursor_pos.x,this.cursor_pos.y);
      Object.assign(this.cursor_pos,{
        x:this.cursor_pos.x + textMetrics.width+this.H_SPACING,
        y:this.cursor_pos.y
      }); // 光标位置
      // ctx.lineTo(col*25,row*25);
      // ctx.stroke();
      return this;
    },
    write:async function(string,interval=100) {
      for(let char of string)
        await this.print_char(char,undefined,undefined,{
          fillStyle:this.randomColor()
        }).then(()=>this.sleep(interval))
      return this;
    },
    sleep:function (interval=0) { //interval millionseconds
      let animate_id = null;
      interval = Number.isNaN(Number(interval))?1000:Number(interval);
      return new Promise((resolve,reject) => {
        const _sleep = (_starttime = 0) => {
          if((new Date().getTime()-_starttime)<interval)
            animate_id = window.requestAnimationFrame(_sleep.bind(null,_starttime)); // 递归
          else {
            window.cancelAnimationFrame(animate_id);
            resolve(this);
          }
        }
        window.requestAnimationFrame(_sleep.bind(null,new Date().getTime()))
      });
    }
  }

  window.onload = function() {
    let tw = new TypeWritter("#myCanvas");
    const poem = `
      青天有月来几时，我今停杯一问之。
      人攀明月不可得，月行却与人相随。
      皎如飞镜临丹阙，绿烟灭尽清辉发。
      但见宵从海上来，宁知晓向云间没。
      白兔捣药秋复春，嫦娥孤栖与谁邻。
      今人不见古时月，今月曾经照古人。
      古人今人若流水，共看明月皆如此。
      唯愿当歌对酒时，月光长照金樽里。`; //先显示
    tw.write(poem)
      .then(writer=>writer.sleep(5000))
      .then(writer=>writer.clear())
      .then(writer=>writer.write(poem));
  }
</script>
</body>
</html>
```

