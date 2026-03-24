---
title:   使用 XLSX 模块导出 Excel 文件
description: 样式、合并单元格、公式等待补充
date:     2025-08-04 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
---

> 使用 XLSX 模块导出 Excel 文件
{: .prompt-tip }

```javascript
{
  // https://sheetjs.com/demo/table.html
  // https://github.com/rockboom/SheetJS-docs-zh-CN

  function loadXLSX(url=`https://cdn.bootcdn.net/ajax/libs/xlsx/0.18.5/xlsx.core.min.js`) {
    return new Promise((resolve,reject)=>{
      const script = document.createElement('SCRIPT');
      script.type = "text/javascript";
      script.src = url;
      script.onload = resolve;
      script.onerror = reject;
      document.body.appendChild(script);
    });
  }

  async function main() {
    if(typeof XLSX == 'undefined')
      await loadXLSX().then(()=>console.log(`XLSX loaded ok`));
    const excleData = [
      {周一: '语文', 周二: '数学', 周三: '历史', 周四: '政治', 周五: '英语'},
      {周一: '数学', 周二: '数学', 周三: '政治', 周四: '英语', 周五: '英语'},
      {周一: '政治', 周二: '英语', 周三: '历史', 周四: '政治', 周五: '数学'},
    ];
    const worksheet = XLSX.utils.json_to_sheet(excleData);
    // 设置表格样式，!cols为列宽
    const options = {
    '!cols': [
      { wpx: 100 },
      { wpx: 100 },
      { wpx: 100 },
      { wpx: 100 },
      { wpx: 100 },
    ]};
    worksheet['!cols'] = options['!cols'];
    // 配置合并单元格（例如，合并第一行的前两个单元格作为表头）
    worksheet['!merges'] = [{s: {r:0, c:0}, e: {r:0, c:4}}]; // 合并第一行的A和B列
    // 可以设置其他单元格的样式，例如表头加粗等
    worksheet['A1'].s = {font: {bold: true}}; // 使表头加粗
    worksheet['B1'].s = {font: {bold: true}}; // 使表头加粗
    worksheet['C1'].s = {font: {bold: true}}; // 使表头加粗
    worksheet['D1'].s = {font: {bold: true}}; // 使表头加粗
    const workbook = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(workbook, worksheet, 'Sheet1');
    XLSX.writeFile(workbook, '课程表.xlsx');
  }
  main();
}

```
