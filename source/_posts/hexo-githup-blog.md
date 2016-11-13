title: hexo和github打造个人博客
date: 2015-12-20 22:35:04
tags: node
categories: 编程语言
---
** hexo和github打造个人博客 ：** <Excerpt in index | 首页摘要>
    使用hexo和github打造属于自己的静态博客，展示自己的作品，思想……
<!-- more -->
<The rest of contents | 余下全文>

##说明
    自己在使用hexo搭建静态博客的时候踩了许多坑,最终去官网看教程搞定了,  
    建议用hexo搭建个人博客的时候,最好看清教程的日期和使用的版本,这样就  
    不会因为版本的不同导致的问题了.建议先去hexo官网了解一下
   [**hexo官网**][1]
## 1.准备工作
 1. 安装nodejs
    - 去官网下载nodejs安装(推荐安装0.12.x),安装之后在命令行 node -v,如果成功说明node环境ok,不成功就去环境变量配置一下.
 2. 安装hexo
    - 使用命令 npm install hexo -g,执行hexo -v 查看版本,本教程适合**3.1.1**版本
 3. 安装git
    - 去官网下载git安装,不会自行百度
 4. 配置git
    - 配置ssh私钥,上传到github上

## 2.github-pages和gitcafe-page的说明

 1. github有两种主页,一种是github-page(个人主页),一种是项目主页,本教程针对个人主页
 2. gitcafe-page的个人主页只是在项目下有一个gitcafe-pages的分支,部署成功后访问主页即可
 3. github-page需要将hexo博客发布到repository的master(主干)即可
 4. gitcafe需要将hexo博客发布到repository的gitcafe-pages的分支
 5. github的个人主页要求repository的名称和username一致，加入username是tom，则repository的名称为tom.github.io
## 3.使用hexo写博客
    - 新建一个文件夹myblog,
    - 右键git bash here使用git的shell
    - 在shell中输入hexo init,回车执行
    - 在shell中输入hexo g ,回车
    - 在shell中hexo s,回车
    - 去浏览器访问http://localhost:4000,访问到主页,然后在shell中ctrl c停止
    - 在shell中hexo new "first-blog",回车
    - 在shell中hexo g ,回车
    - 在shell中hexo s ,回车,在访问
    - ok,在本地测试就没问题了

## 4.发布到github和gitcafe
    - 打开项目根部录下的.config.yml,找到deploy,修改如下:

```
   deploy:
    - type: git
      repo: git@github.com:yourname/yourname.github.io.git,master
    - type: git
      repo: git@gitcafe.com:yourname/yourname.git,gitcafe-pages
```
    - 如果只发布到github或者gitcafe上,修改如下:git的branch是master,gitcafe的是gitcafe-pages
```
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
  message: [message]
```
    访问地址就是 http://tom.github.io/
### 5.常用命令
    命令的简写为：
    ```
    hexo n == hexo new
    hexo g == hexo generate
    hexo s == hexo server
    hexo d == hexo deploy
    hexo clean  删除public文件夹
    ```

## 6.常见问题
1. 部署时出现git not found
  - npm install hexo-deployer-git --save  安装依赖包


## 7.详细设置    
    每个人对自己的博客都有不一样的要求，比如主题，分类，标签，评论插件的选择，  
    这些对程序员的你来说，都是小菜一碟，下面是官网教程：
   [hexo官方文档][2]


博客效果可以看我的个人博客     [我的个人博客][3]


  [1]: https://hexo.io/zh-cn/
  [2]: https://hexo.io/docs/
  [3]: http://geekwalker.cn
