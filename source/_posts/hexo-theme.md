title: hexo自用黑色主题
date: 2017-05-23 11:32:59
tags: hexo
categories: 开发工具
---
** hexo和coding打造静态博客 ：** <Excerpt in index | 首页摘要\>
使用hexo一年有余，对所有主题都感觉有所缺陷，便修改了一个自用黑色主题，本主题以黑色和蓝色为主，色彩鲜明，主题明确。	
<!-- more -->
<The rest of contents | 余下全文\>

## 主题图片
![主题首页](http://o7kalf5h3.bkt.clouddn.com/blog-index.png) 

## black-blue主题来源
本主题修改自**spfk**主题，但之前spfk主题有很多问题，本主题改进如下：
1. 压缩js，css提高性能
2. 代码段样式显示更完美
3. 增加本地搜索
4. 设置更合适的字体大小
5. 颜色以黑色和蓝色为主，色彩鲜明
6. seo适当优化
7. 删除多说，有言，增加畅言评论
8. 删除stylus，全部改用css方便修改

## 主题地址
[black-blue](https://github.com/maochunguang/black-blue)

## 注意：
大家使用主题的时候，把**主题配置文件_config.yml**以下几项必须修改，项目里实用的是我博客的正式代码，请大家修改成自己的！
```yml
google_analytics: xxx
baidu_analytics: xxxxxxx
disqus:
  on: false
  shortname: xxxx
# 畅言评论
changyan:
  on: true
  appid: xxxx
  conf: xxxxx

```
## black-blue主题配置
### 切换主题
复制主题到themes目录下`cd themes && git clone https://github.com/maochunguang/black-blue`，修改_config.yml `theme: black-blue`

### 安装常用插件，建议全部安装
```bash
## rss插件
npm install hexo-generator-feed --save
## 站点sitemap生成插件
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
## 百度url提交
npm install hexo-baidu-url-submit --save
## 本地搜索插件集成
npm install hexo-generator-search --save
```
### 博客全局配置，修改根目录下_config.yml
插件配置
```yml
Plugins:
- hexo-generator-feed
- hexo-generator-sitemap
- hexo-generator-baidu-sitemap
```
rss设置
```yml
feed:
  type: atom
  path: atom.xml
  limit: 20
```
本地搜索配置
```yml
search:
  path: search.json
  field: post
```
站点地图，seo搜索引擎需要
```yml
sitemap:
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml
```
### 主题配置
菜单配置
```yml
# Header | 主菜单
## About Page: `hexo new page about`
## Tags Cloud Page: `hexo new page tags`
menu:
  # 主页: /archives/
  所有文章: /archives/
  玩转开发工具: /categories/开发工具/
  玩转数码: /categories/digital
  认知提升: /categories/cognition
  关于我: /about/
```
评论配置
```yml
# 是否开启畅言评论，
changyan:
  on: true
  appid: xxxx
  conf: xxxxxxxxxxxx
# 是否开启disqus，
disqus:
  on: false
  shortname: mmmmmm
```

### 其他配置，**详细的配置请下载主题，都有注释**
```yml
# 数学公式支持
mathjax: false
# Socail Share | 是否开启分享
baidushare: true
# 谷歌分析，百度分析，seo分析很有用
google_analytics: xxxxxx
baidu_analytics: xxcxcxcsdsf

```