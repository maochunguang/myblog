title: hexo自用黑色主题
date: 2016-04-19 11:32:59
tags: hexo
categories: 开发工具
---
** hexo和coding打造静态博客 ：** <Excerpt in index | 首页摘要\>
使用hexo一年有余，对所有主题都感觉有所缺陷，便修改了一个自用黑色主题，本主题以黑色和蓝色为主，色彩鲜明，主题明确。	
<!-- more -->
<The rest of contents | 余下全文\>

### black-blue主题来源
本主题修改自**spfk**主题，但之前spfk主题有很多问题，本主题改进如下：
1. 压缩js，css提高性能
2. 代码段样式显示更完美
3. 增加本地搜索
4. 设置更合适的字体大小
5. 颜色以黑色和蓝色为主，色彩鲜明
6. seo适当优化
7. 删除多说，有言，增加畅言评论
8. 删除stylus，全部改用css方便修改

### black-blue主题配置
1. 切换主题
复制主题到themes目录下`cd themes && git clone https://github.com/maochunguang/black-blue`，修改_config.yml `theme: black-blue`

2. 安装常用插件，建议全部安装
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
3. 博客全局配置，修改根目录下_config.yml
插件配置
```yml
Plugins:
- hexo-generator-feed
- hexo-generator-sitemap
- hexo-generator-baidu-sitemap
```
```yml
feed:
  type: atom
  path: atom.xml
  limit: 20
```
```yml
search:
  path: search.json
  field: post
```
```yml
sitemap:
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml
```
