---
title:   基于 XMLHttpRequest 的 Ajax
description: 很久很久之前的 ajax 实现方案
date:     2025-08-02 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
---

> XMLHttpRequest 对象实现 Ajax，Fetch 出来后基本都不用了
{: .prompt-tip }

```javascript
((url)=>{
  const AjaxFactory = function(timeout=3) {
    const Json2QueryString = Symbol();
    const onreadystatechange = Symbol();
    const ontimeout = Symbol();
    class CAjax {
      constructor(timeout) {
        this.xhr = null;
        if (window.XMLHttpRequest)
          this.xhr = new window.XMLHttpRequest();
        else
          this.xhr = new window.ActiveXObject("Microsoft.XMLHttp");
        this.timeout = timeout || 0;
      }

      // 转 JSON 为 a=b&c=d 格式
      [Json2QueryString](json) {
        let arr = [];
        for(let key in json)
          arr.push(`${key}=${json[key]}`);
        return arr.join('&');
      }

      // form 提交时，application/json 或 application-application/x-www-form-urlencoded
      // formDataType 取值为 json 或 urlencoded
      ajax(url,payload,method="GET",formDataType = "urlencoded") {
        payload = payload || {} ;
        const queryString = this[Json2QueryString](payload);
        // GET 方式，参数附着在后面
        url = (method === 'GET' && queryString.length !==0 )?`${url}?${queryString}`: url;
        return new Promise((resolve,reject) => {
          this.xhr.open(method,url,true);
          this.xhr.onreadystatechange = this[onreadystatechange].bind(this,resolve,reject);
          this.xhr.ontimeout = this[ontimeout].bind(this,resolve,reject);
          let data = null; // GET 方式时，不需要提交 data
          if(method === 'POST') {
            // 默认 POST 以 application/x-www-form-urlencoded 提交数据
            let content_type = 'application/x-www-form-urlencoded; charset=UTF-8';
            data = queryString;
            if (formDataType.toLowerCase() === "json") {
              content_type = 'application/json; charset=UTF-8';
              data = JSON.stringify(payload);
            }
            this.xhr.setRequestHeader('Content-Type',content_type);
          }
          this.xhr.send(data);
        })
      }

      [onreadystatechange](resolve,reject) {
        // 1=open  2=send  3=接收到部分数据  4=接收到完整数据
        if (this.xhr.readyState == 4) 
          if (this.xhr.status == 200) 
            resolve(this.xhr)
          else
            reject(`[${this.xhr.status} ${this.xhr.statusText}]${this.xhr.responseURL}`);
      }

      [ontimeout](resolve,reject) {
        reject(`Ajax Request Timeout!`);
      }
    }
    return new CAjax(timeout);
  };

  const ajaxer = AjaxFactory();
  const payload = {
    a:"HELLO",
    b:"测试",
    c:3
  }
  url = url?url:window.location.href;
  ajaxer.ajax(url,payload,method="GET")
  .then(xhr=>console.dir(xhr))
  .then(()=>ajaxer.ajax(url,payload,method="POST"))
  .then(xhr=>console.dir(xhr))
  .then(()=>ajaxer.ajax(url,payload,method="POST",formDataType = "json"))
  .then(xhr=>console.dir(xhr))
  .catch((err)=>console.error(err))
})();
```
