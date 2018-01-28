title: mybatis-generator
date: 2018-01-28 17:30:19
tags: mysql
categories: 开发工具
---
** {{ title }}：** <Excerpt in index | 首页摘要>
mybatis反向生成器，根据数据库表，自动创建pojo，mapper以及mybatis配置文件，能极大的提高开发效率。
<!-- more -->
<The rest of contents | 余下全文>

## 插件介绍
本插件fork自[mybatis-generator-gui](http://link),在此基础上加了批量生成表。

## 插件特性
1. 保存数据库配置
2. 根据表生成pojo，mapper以及mybatis配置文件
3. 批量生成
4. 其它功能（待开发）

## 插件使用
### 要求
本工具由于使用了Java 8的众多特性，所以要求JDK <strong>1.8.0.60</strong>以上版本，对于JDK版本还没有升级的童鞋表示歉意。

### 启动本软件

* 方法一: 自助构建

```bash
    git clone https://github.com/maochunguang/mybatis-generator-gui
    cd mybatis-generator-gui
    mvn jfx:jar
    cd target/jfx/app/
    java -jar mybatis-generator-gui.jar
```
* 方法二: IDE中运行Eclipse or IntelliJ IDEA中启动, 找到`com.zzg.mybatis.generator.MainUI`类并运行就可以了


### 文档
更多详细文档请参考本库的Wiki
* [Usage](https://github.com/maochunguang/mybatis-generator-gui/wiki)

## 截图参考
![MainUI](http://o7kalf5h3.bkt.clouddn.com/mybatis.png)










> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
