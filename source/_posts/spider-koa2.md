title: 用koa2.x写下载漫画的爬虫
date: 2017-05-14 07:15:38
tags: nodejs
categories: 学习笔记
---
** {{ title }}：** <Excerpt in index | 首页摘要>
使用koa2.x的async ，await解决异步问题，写一个下载漫画的爬虫，代码里有惊喜和福利哦！
<!-- more -->
<The rest of contents | 余下全文>

## 项目搭建
1. 安装nodejs>7.6,安装koa-generator
2. 直接`koa2 spider`,生成项目
3. 安装request,request-promise,cheerio,mkdirp
4. npm install安装依赖

## 思路
图片或者漫画爬虫的思路很简单，首先观察url的规律，把url按规律加入到下载任务，其实就是请求获得html内容，然后对html进行解析，找到下载的图片url（一般都是img标签的src属性值），把url放到数组保存，使用async await控制所有的任务，直到把所有的图片下载完。

## 难点
但是nodejs本身上异步的，如果你直接在for循环里去下载，肯定是不行的，必须控制好异步的执行上关键。
爬虫简单，处理好异步难。这里我使用的es7中async，await配合promise解决异步问题，还可以使用async模块，eventproxy，等等异步控制模块来解决。

## 核心代码,spider.js
```js
const fs = require('fs');
const request = require("request-promise");
const cheerio = require("cheerio");
const mkdirp = require('mkdirp');
const config = require('../config');
exports.download = async function(ctx, next) {
    const dir = 'images';
    // 图片链接地址
    let links = [];
    // 创建目录
    mkdirp(dir);
    var urls = [];
    let tasks = [];
    let downloadTask = [];
    let url = config.url;
    for (var i = 1; i <= config.size; i++) {
        let link = url + '_' + i + '.html';
        if (i == 1) {
            link = url + '.html';
        }
        tasks.push(getResLink(i, link))
    }
    links = await Promise.all(tasks)
    console.log('links==========', links.length);

    for (var i = 0; i < links.length; i++) {
        let item = links[i];
        let index = item.split('___')[0];
        let src = item.split('___')[1];
        downloadTask.push(downloadImg(src, dir, index + links[i].substr(-4, 4)));
    }
    await Promise.all(downloadTask);
}

async function downloadImg(url, dir, filename) {
    console.log('download begin---', url);
    request.get(url).pipe(fs.createWriteStream(dir + "/" + filename)).on('close', function() {
        console.log('download success', url);
    });
}
async function getResLink(index, url) {
    const body = await request(url);
    let urls = [];
    var $ = cheerio.load(body);
    $(config.rule).each(function() {
        var src = $(this).attr('src');
        urls.push(src);
    });
    return index + '___' + urls[0];
}
```
## 基础配置
由于爬虫的复杂性基于不同的网站，不同的任务很不一样，这里只是把几个常用的变量抽取到了config.js。
```js
module.exports = {
    //初始url
    url: 'http://www.xieet.com/meinv/230',
    size: 10,
    // 选中图片img标签的选择器
    rule: '.imgbox a img'
};
```

## 运行代码
1. 下载我上传的代码[koa-spider](https://github.com/maochunguang/koa-spider)
2. npm install,npm start即可运行

## 总结
其实无论是写爬虫还是些其他程序，使用nodejs很大一部分都是要处理异步，要学好nodejs必须学好异步处理。


> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
