title: java和javascript日期详解
date: 2016-05-13 21:48:00
tags: java
categories: 编程语言
---
** java，js日期转换：** <Excerpt in index | 首页摘要>
    java的各种日期转换
<!-- more -->
<The rest of contents | 余下全文>

## 日期表示类型
1. 获取long类型的日期格式
```java
long time = System.currentTimeMillis();
System.out.printf(time+"");
Date date =new Date();
System.out.println(date.getTime());
```
2. 获取制定格式的日期
```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
Date date =new Date();
System.out.println(sdf.format(date) );
```
3. 把制定格式的日期转为date或者毫秒值
```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
Date date = sdf.parse("2016-05-22 10:15:21");
long mills = date.getTime();
```
- 说明:System.currentTimeMillis()并不能精确到1ms的级别,它取决于运行的系统,你再windows,mac,linux精确的范围都有差异,对于有高精度时间的要求,不能使用这个

## 日期计算
1. 最方便的方式是将时间转为毫秒值进行计算
```java
Date from =new Date();
Thread.sleep(200);//线程休眠2ms
Date to =new Date();
System.out.println(to.getTime()-from.getTime());
```

## 高精度时间
```java
long time1 =System.nanoTime();
System.out.printf(time1+"");
```
- 说明:System.nanoTime()提高了ns级别的精度,1ms=1000000ns,

## javascript日期
1. 获取时间的毫秒值，获取月份，时间
```js
var myDate = new Date();
myDate.getYear(); //获取当前年份(2位)
myDate.getFullYear(); //获取完整的年份(4位,1970-????)
myDate.getMonth(); //获取当前月份(0-11,0代表1月)
myDate.getDate(); //获取当前日(1-31)
myDate.getDay(); //获取当前星期X(0-6,0代表星期天)
myDate.getTime(); //获取当前时间(从1970.1.1开始的毫秒数)
myDate.getHours(); //获取当前小时数(0-23)
myDate.getMinutes(); //获取当前分钟数(0-59)
myDate.getSeconds(); //获取当前秒数(0-59)
myDate.getMilliseconds(); //获取当前毫秒数(0-999)
myDate.toLocaleDateString(); //获取当前日期
var mytime=myDate.toLocaleTimeString(); //获取当前时间
myDate.toLocaleString( ); //获取日期与时间
```
2. 时间戳获取
注意，java，php等生成的时间戳是秒，不是毫秒，所以需要签名时间戳的时候，需要转为秒时间戳
```js
var time = new Date();
var timestamp = parseInt(time.getTime()/1000);
```
3. 格式化时间
```js
//获取当前时间，格式YYYY-MM-DD
function getNowFormatDate() {
    var date = new Date();
    var seperator1 = "-";
    var year = date.getFullYear();
    var month = date.getMonth() + 1;
    var strDate = date.getDate();
    if (month >= 1 && month <= 9) {
        month = "0" + month;
    }
    if (strDate >= 0 && strDate <= 9) {
        strDate = "0" + strDate;
    }
    var currentdate = year + seperator1 + month + seperator1 + strDate;
    return currentdate;
}
```
