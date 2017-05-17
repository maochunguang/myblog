title: ajax简单教程
date: 2016-07-06 20:53:11
tags: http
categories: 编程语言
---
** ajax简单教程：** <Excerpt in index | 首页摘要>
    ajax常用的方法，一些容易出错的地方
<!-- more -->
<The rest of contents | 余下全文>

### ajax原理
Ajax的原理简单来说通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。这其中最关键的一步就是从服务器获得请求数据。要清楚这个过程和原理，我们必须对 XMLHttpRequest有所了解。
XMLHttpRequest是ajax的核心机制，它是在IE5中首先引入的，是一种支持异步请求的技术。简单的说，也就是javascript可以及时向服务器提出请求和处理响应，而不阻塞用户。达到无刷新的效果。
所以我们先从XMLHttpRequest讲起，来看看它的工作原理。首先，我们先来看看XMLHttpRequest这个对象的属性。
它的属性有：

  | onreadystatechange   | responseText    |
  | :------------- | :------------- |
  |  每次状态改变所触发事件的事件处理程序    | 从服务器进程返回数据的字符串形式    |

  | responseXML   | status     |
  | :------------- | :------------- |
  | 从服务器进程返回的DOM兼容的文档数据对象  | 从服务器返回的数字代码，比如常见的404 |

  | status Text    | readyState    |
  | :------------- | :------------- |
  | 伴随状态码的字符串信息                 | 对象状态值                        |
readyState 对象状态值
- 0 (未初始化) 对象已建立，但是尚未初始化（尚未调用open方法）
- 1 (初始化) 对象已建立，尚未调用send方法
- 2 (发送数据) send方法已调用，但是当前的状态及http头未知
- 3 (数据传送中) 已接收部分数据，因为响应及http头不全，这时通过responseBody和responseText获取部分数据会出现错误，
- 4 (完成) 数据接收完毕,此时可以通过通过responseXml和responseText获取完整的回应数据

### ajax的使用

1. 原生的ajax

```js
function CreateXmlHttp() {
    //非IE浏览器创建XmlHttpRequest对象
    if (window.XmlHttpRequest) {
        xmlhttp = new XmlHttpRequest();
    }
    //IE浏览器创建XmlHttpRequest对象
    if (window.ActiveXObject) {
        try {
            xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
        }
        catch (e) {
            try {
                xmlhttp = new ActiveXObject("msxml2.XMLHTTP");
            }
            catch (ex) { }
        }
    }
}

function Ustbwuyi() {
    var data = document.getElementById("username").value;
    CreateXmlHttp();
    if (!xmlhttp) {
        alert("创建xmlhttp对象异常！");
        return false;
    }
    xmlhttp.open("POST", url, false);
    xmlhttp.onreadystatechange = function () {
        if (xmlhttp.readyState == 4) {
            document.getElementById("user1").innerHTML = "数据正在加载...";
            if (xmlhttp.status == 200) {
                document.write(xmlhttp.responseText);
            }
        }
    }
    xmlhttp.send();
}
```

2. jquery调用ajax

```js
$.ajax({
    type: "get",
    url: "http://www.cnblogs.com/rss",
    beforeSend: function(XMLHttpRequest){
    //ShowLoading();
    },
    success: function(data, textStatus){
        $(".ajax.ajaxResult").html("");
        $("item",data).each(function(i, domEle){
        $(".ajax.ajaxResult").append("<li>"+$(domEle).children("title").text()+"</li>");
        });

    },
    complete: function(XMLHttpRequest, textStatus){
    //HideLoading();
    },
    error: function(){
    //请求出错处理
    }
});
```





> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
