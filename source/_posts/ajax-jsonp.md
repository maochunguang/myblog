title: ajax和jsonp区别
date: 2016-07-06 20:54:16
tags: http
categories: 编程语言
---
** ajax和jsonp区别 ：** <Excerpt in index | 首页摘要>
    jquery的封装影响了很多人的误解，所以有必要对ajax和jsonp的本质区别讲解，
<!-- more -->
<The rest of contents | 余下全文>

### jsonp是什么？
利用在页面中创建`<script>`节点的方法向不同域提交HTTP请求的方法称为JSONP，这项技术可以解决跨域提交Ajax请求的问题。JSONP的工作原理如下所述：假设在 http://example1.com/index.php 这个页面中向 http://example2.com/getinfo.php 提交GET请求，我们可以将下面的JavaScript代码放在 http://example1.com/index.php 这个页面中来实现：

```js
var eleScript= document.createElement("script");
eleScript.type = "text/javascript";
eleScript.src = "http://example2.com/getinfo.php";
document.getElementsByTagName("HEAD")[0].appendChild(eleScript);
```

当GET请求从 http://example2.com/getinfo.php 返回时，可以返回一段JavaScript代码，这段代码会自动执行，可以用来负责调用 http://example1.com/index.php 页面中的一个callback函数。

JSONP的优点是：它不像XMLHttpRequest对象实现的Ajax请求那样受到同源策略的限制；它的兼容性更好，在更加古老的浏览器中都可以运行，不需要XMLHttpRequest或ActiveX的支持；并且在请求完毕后可以通过调用callback的方式回传结果。

JSONP的缺点则是：它只支持GET请求而不支持POST等其它类型的HTTP请求；它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题。

### ajax是什么？

Ajax的原理简单来说通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。这其中最关键的一步就是从服务器获得请求数据。要清楚这个过程和原理，我们必须对 XMLHttpRequest有所了解。
　XMLHttpRequest是ajax的核心机制，它是在IE5中首先引入的，是一种支持异步请求的技术。简单的说，也就是javascript可以及时向服务器提出请求和处理响应，而不阻塞用户。达到无刷新的效果。

### 误区是怎么产生的？

这个很大程度上要归功于jquery的封装，由于jquery在api上，对json和jsonp都属于ajax模块，导致很多人误以为jsonp是ajax一种。




> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
