title: kobo aura one导出笔记高级配置
date: 2017-06-23 10:22:57
tags: 开发工具
categories: others
---
** {{ title }}：** <Excerpt in index | 首页摘要>
kobo电子书折腾记，导出笔记，从激活到设置，打补丁实现自定义配置，还是自己折腾起来有意思啊。
<!-- more -->
<The rest of contents | 余下全文>

## 建议
买电子书是为了阅读和学习，不是天天折腾电子书，一天刷一次机，如果只是看书，做笔记，学个英文什么的
原生系统是最好的。如果看pdf为主，不建议买这电子书，看pdf首选电脑，平板，sony dsp系列，用普通的电子书阅读器，体验太差。

## kobo原生系统的功能（推荐原生系统，打上补丁）
1. 格式支持epub，mobi，cbz漫画，txt，kobo epub格式
2. 高亮，笔记，导出笔记（需要配置一下）
3. 字典（英文，中文，法文等多国字典，可以自己修改）
4. 阅读pocket文章（可以把网页保存到pocket，实用pocket同步到阅读器）
5. 自动亮度（最大的优点）

## koreader的功能
1. 格式支持epub，mobi，cbz漫画，txt，kobo epub格式
2. 扫描版pdf支持重拍，切边（最大特色）
3. 笔记导出到印象笔记
4. 字典（强大的字典扩展）


## 激活
**说明：wifi激活需要翻墙，可以实用笔记连接vpn，然后共享wifi给kobo**
1. wifi激活,
2. kobo setup desktop激活，去kobo官网下载软件，然后电脑需要翻墙，电子书连接上电脑，用软件登录激活。这个软件很不好用，bug也多，建议使用wifi激活。

## 更新固件，打补丁
kobo的更新固件，更新补丁都是一个模式，把固件或者补丁放到.kobo文件夹，弹出设备就会自动重启

## 字体
电脑连接kobo，在根目录建立一个fonts文件夹，把需要的字体放进去即可

## 词典
下载网上改好的字典，直接放到.kobo文件夹下的dict目录下，然后重启就可以了

## 自定义配置
1. 刷新页数（打补丁）
2. 上下页宽（打补丁）
3. 全屏模式（修改配置文件）
4. 字体高级设置（修改配置文件）
5. 导出笔记和高亮（修改配置文件）

## kobo高级配置文件详解
用电脑连接kobo电子书，打开Kobo找到eReader.conf文件，最好用notepad++修改，或者其他文本编辑器。
```conf
[FeatureSettings]
#导出笔记
ExportHighlightsEnabled=true
#显示全书的页码，而不是章节的页码
FullBookPageNumbers=true
#用在线等维基百科代替词典查询
OnlineWikipedia=true
#全屏阅读
FullScreenReading=true
#图片缩放
ImageZoom=true
#浏览器全屏
FullScreenBrowser=true
#关机键截图，但是关机键就无法关机了，不要设置这个鸡肋的功能
Screenshots=true
[Reading]
#翻页刷新的页数，20页全刷一次
numPartialUpdatePageTurns=20
#左边距
readingLeftMargin=0
#右边距
readingRightMargin=0
#行高
readingLineHeight=1.4

[PowerOptions]
#自动关机时间
AutoOffMinutes=60
```


> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
