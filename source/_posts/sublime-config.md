title: sublime详细配置
date: 2016-04-26 18:47:46
tags: others
categories: 开发工具
---
** sublime详细配置：** <Excerpt in index | 首页摘要>
	subime的常用配置应有尽有，快快来看吧
 <!-- more -->
<The rest of contents | 余下全文>

### 安装sublime text3
- 去官网或者搜索一下，然后进行自行破解，建议安装sublime3083版本

### 安装package管理工具
- 使用通过View->Show Console菜单打开命令行，粘贴如下代码：
	```
	import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
	```

- 手动安装
	1. 点击Preferences > Browse Packages菜单
	2. 进入打开的目录的上层目录，然后再进入Installed Packages/目录
	3. 下载[package controle](https://sublime.wbond.net/Package%20Control.sublime-package)并复制到Installed Packages/目录
	4. 重启Sublime Text。

### 安装常用插件
1. 点击Preferences > package controle菜单输入install package,输入插件名称
2. 常用插件名称：
	1. emmet,git,sidebar


### 主题和配色设置
-　推荐seti主题，配色主题推荐one dark（和atom一样）

### 侧边栏高级设置
- 修改侧边栏字体大小 用sublime编辑主题文件，搜索sidebar_label，找到font,把相关的字体调大

### 开发环境配置

### 快捷键设置

### over
