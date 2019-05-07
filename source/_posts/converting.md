---
title: "Uncaught TypeError: Converting circular structure to JSON 解决办法"
date: 2019-04-08 15:14:55
tags: 
    - javaScript
categories: 
    - javaScript
cover: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3214278065,3431631512&fm=26&gp=0.jpg
---

今天遇到一个问题，使用JSON.stringify()方法时报错  Converting circular structure to JSON

报错的原因在于： 
在请求中传递的对象有一个循环引用

例如：

var a = {b};

b.parent = a;

 

只要修改如下，即可解决问题

 ```
   var cache = [];
        var str = JSON.stringify(org, function(key, value) {
            if (typeof value === 'object' && value !== null) {
                if (cache.indexOf(value) !== -1) {
                    // Circular reference found, discard key
                    return;
                }
                // Store value in our collection
                cache.push(value);
            }
            return value;
        });
        cache = null; // Enable garbage collection
```

留作记录，后续查其原理。