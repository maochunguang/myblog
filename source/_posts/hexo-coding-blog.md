title: hexo和coding打造静态博客
date: 2016-04-19 11:32:59
tags: node
categories: 编程语言
---
** hexo和coding打造静态博客 ：** <Excerpt in index | 首页摘要>
    使用hexo和coding打造属于自己的静态博客，展示自己的作品，思想……
+ <!-- more -->
<The rest of contents | 余下全文>

### 什么是hexo?
- hexo是台湾的一个大学生用nodejs做的一个静态博客框架，使用hexo可以速度搭建自己的静态博客，
结合第三方的代码托管平台即可实现个人的博客网站。

### 什么是coding?
- coding是和github类似的代码托管平台，但又不仅仅是托管平台，还有在线ide，码市等旗下产品，gitcafe也是被coding收购了，之前用gitcafe搭建的博客可以重新搭建了。

### 什么是静态博客?
- 静态博客就是只是有静态页面和js，css构成的博客，普通的网站都是有web服务构成的，而静态博客只是页面的集合，利用js做一些展示和特效。

### 安装hexo
- 首先确保自己安装nodejs，在命令行输入，尽量安装v0.12以上的版本
```
	node -v
```
- 安装hexo，命令行输入
```
	npm install hexo-cli -g
```
- 本地调试hexo，参考我的另一篇文章
[hexo和github打造个人博客](http://geekwalker.cn/2015/12/20/hexo-githup-blog/)

### 注册配置coding
- coding的pages功能有两种，一个是项目主页，一个是个人主页，本博客采用个人主页。
- 个人主页就是项目中有一个coding-pages的分支，把项目发布到这个分支即可
- coding的代码也是通过git这个工具，所以在coding个人设置里配置好自己的ssh公钥和github操作一样的
- 在coding建立一个项目，名字和你的个性后缀一样，也就是Global Key，在个人设置里可以看到

### 部署博客到coding
- 在项目根目录下，修改_config.yml文件，把maocg替换为你的Global Key
```
	deploy:
	- type: git
	  repo:git@git.coding.net:maocg/maocg.git,coding-pages
```
