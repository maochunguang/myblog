title: 微信公众号开发
date: 2017-04-28 12:55:33
tags: 编程语言
categories: javacript
---
** {{ title }}：** <Excerpt in index | 首页摘要>
微信公众号开发的一些注意事项
<!-- more -->
<The rest of contents | 余下全文>

### 开发环境搭建
1. 微信公众号开发者配置，url，token，
2. 本地调试，使用内网穿透工具，花生壳，或者netapp，买一个可以自定义域名的，内网映射到制定端口，
3. 项目搭建，express或koa搭建项目，npm有微信的现成包，直接配置

### 回复
1. 回复和发消息并没有什么特别注意的地方，这里不多说

### 菜单
1. 微信菜单有自定义菜单，有个性化菜单，但是个性化菜单优先级高于个性化菜单
2. 个性化菜单可以根据用户的tag，sex，group等属性进行区分菜单
3. 注意，我在使用时发现**个性化菜单经常会失效**，不起作用，偶尔会起作用，如果线上打算使用个性化菜单，请慎重并仔细测试

### 授权
授权有网页授权，js sdk授权，
网页授权也有两种，一个上静默授权，一个是点击授权，贴一下js sdk调用前认证的代码，要使用sha1加密
```js
async getSignConfig(originUrl) {
      let data = {}
      const sha1 = crypto.createHash('sha1')
      const appId = this.app.config.weixin.appID
      const jsapi_ticket = await this.ctx.service.token.getJSApiTicket()
      const noncestr = this.app.config.jsapi.noncestr
      const url = this.app.config.domain + originUrl
      const timestamp = parseInt(new Date().getTime() / 1000)
      // sha1加密
      const str = `jsapi_ticket=${jsapi_ticket}&noncestr=${noncestr}&timestamp=${timestamp}&url=${url}`
      sha1.update(str)
      const signature = sha1.digest('hex')
      data = { jsapi_ticket, noncestr, timestamp, url, signature, appId }
      return data
    }
```
调用js sdk页面上代码
```js
wx.config({
    debug: false, // 开启调试模式,
    appId: appId, // 必填，公众号的唯一标识
    timestamp: timestamp, // 必填，生成签名的时间戳
    nonceStr: nonceStr, // 必填，生成签名的随机串
    signature:  signature,// 必填，签名，见附录1
    jsApiList: ['closeWindow'] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
});
wx.ready(function(){
    setTimeout(function(){
      wx.closeWindow();
    },2000);
});
```


### 实用的常识
1. tag不能重复创建，但是给用户可以重复打同一个tag
2. 更改菜单一般五分钟生效，或者重新关注公众号，立马能看到
3. 如果调用js sdk，务必使用https，防止因为安全问题，导致ios下js下载失败。如果你的服务是https，而引用了https的微信js，在ios下肯定会下载失败，这是ios的安全机制导致的。
4. 微信关闭窗口的js接口，不管jsconfig验证是否通过，窗口都可以关闭
5. 微信的token过期时间上2h，但是很多时候30分钟不到可能已经失效，建议**把token过期时间设置为10分钟之内**

### 常见报错
1. 创建菜单的时候，菜单长度不合法，仔细检查自己传的json菜单，一般都是**json格式问题**，而不是长度
2. redirect_uri不合法，是创建授权菜单的redirect_uri和**网页授权域名**配置不一样
3. 关注公众号，服务端设置的欢迎消息发不过去，如果自己代码无异常，一般是因为**token过期**

### 以后遇到其他问题继续补充


> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
