---
title: "node遍历文件并读取内容"
date: 2018-11-12 15:14:55
tags: 
    - fs
categories: 
    - nodeJs
cover: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1557224094681&di=46e2d144b53e77c806c5182fd18678b8&imgtype=0&src=http%3A%2F%2Fww2.sinaimg.cn%2Flarge%2F005SiNxyjw1exp8pwu2b8j30go08cjro.jpg
---
```
var fs = require('fs');
var path = require('path');//解析需要遍历的文件夹
var filePath = path.resolve('./dist');
//调用文件遍历方法
fileDisplay(filePath);
//文件遍历方法
function fileDisplay(filePath){
    //根据文件路径读取文件，返回文件列表
    fs.readdir(filePath,function(err,files){
        if(err){
            console.warn(err)
        }else{
            //遍历读取到的文件列表
            files.forEach(function(filename){
                //获取当前文件的绝对路径
                var filedir = path.join(filePath, filename);
                //根据文件路径获取文件信息，返回一个fs.Stats对象
                fs.stat(filedir,function(eror, stats){
                    if(eror){
                        console.warn('获取文件stats失败');
                    }else{
                        var isFile = stats.isFile();//是文件
                        var isDir = stats.isDirectory();//是文件夹
                        if(isFile){
                            console.log(filedir);
　　　　　　　　　　　　　　　　　// 读取文件内容
                            var content = fs.readFileSync(filedir, 'utf-8');
                            console.log(content);
                        }
                        if(isDir){
                            fileDisplay(filedir);//递归，如果是文件夹，就继续遍历该文件夹下面的文件
                        }
                    }
                })
            });
        }
    });
}



如果碰到有中文不能解析的html，这样写


var cheerio = require('cheerio');
var iconv = require('iconv-lite');
var myHtml = fs.readFileSync("index.html");
var myHtml2 = iconv.decode(myHtml, 'gbk');
console.log(myHtml2);
```