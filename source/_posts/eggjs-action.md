title: egg.js实战技巧
date: 2017-05-16 03:07:27
tags: node
categories: 编程语言
---
** {{ title }}：** <Excerpt in index | 首页摘要>
用eggjs一个多月，踩了不少坑，把踩坑过程分享出来，希望大家少走弯路。
<!-- more -->
<The rest of contents | 余下全文>

### egg.js介绍
请去官网看介绍 [egg.js](https://eggjs.org/zh-cn/)

### 说明
本文所有示例基于async function，node版本node>=7.6.

### controller和service的两种写法：基于类和普通方法
这两种写法在获取全局变量有些地方不一样，这里列举最常见的几个全局变量的异同。

1. 基于类的controller
```js
module.exports = app => {
  class User extends app.Controller {
    async queryUser() {
      // logger获取，request对象获取
      app.logger.info(this.ctx.request.body)
      // 全局配置文件获取
      const domain = app.config.domain
      let users = []
      try {
        // 全局插件获取
        users = await app.model.user.find({})
        // service获取
        await this.ctx.service.user.find({})
      } catch (e) {
        app.logger.error(e)
      }
      // 获取body
      this.ctx.body = {
        users,
      }
      // 重定向写法
      this.ctx.redirect('http://www.baidu.com')
      // render 模版引擎，
      this.ctx.body = app.nunjucks.render('register.nj')
    }
  return User
}
```

2. 基于普通方法(exports)
```js
exports.register = async function register() {
  // logger获取，request对象获取
  this.logger.info(this.request.body)
  try {
    // 全局配置文件获取
    const domain = this.app.config.domain
    // service获取
    await this.service.register.register(this.token, this.request.body)
    // 全局插件获取
    const users = await this.app.model.user.find({})
    this.service.message.send()
  } catch (e) {
    this.logger.error(e)
  }
  // 获取body
  this.body = { 'success' }
  // 重定向写法
  this.redirect('http://www.baidu.com')
  // render 模版引擎，
  this.body = this.app.nunjucks.render('success.nj', data)
}
```

### 自定义配置文件
egg.js的配置文件非常人性化，有config.default.js,config.default.prod.js,config.test.js等等。
运行时根据环境变量加载不同的配置文件。默认时config.default.js,指定环境变量后会把config.env.js(对应环境)的配置文件
和config.default.js合并。

### 调试代码
由于使用了async await，调试起来有些麻烦，建议使用webstorm或者chrome进行调试，eggjs自带egg-bin，支持在chrome里进行调试，对async，await有良好的支持。

### 此文章持续更新,欢迎收藏关注


> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
