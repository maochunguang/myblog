title: java和javascript日期详解
date: 2016-05-13 21:48:00
tags: java
categories: 编程语言
---
** java，js日期转换：** <Excerpt in index | 首页摘要>
    java的各种日期转换
+ <!-- more -->
<The rest of contents | 余下全文>

### 日期表示类型
1. 获取long类型的日期格式
```
long time = System.currentTimeMillis();
System.out.printf(time+"");
Date date =new Date();
System.out.println(date.getTime());
```
2. 获取制定格式的日期
```
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
Date date =new Date();
System.out.println(sdf.format(date) );
```
3. 把制定格式的日期转为date或者毫秒值
```
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
Date date = sdf.parse("2016-05-22 10:15:21");
long mills = date.getTime();
```
- 说明:System.currentTimeMillis()并不能精确到1ms的级别,它取决于运行的系统,你再windows,mac,linux精确的范围都有差异,对于有高精度时间的要求,不能使用这个

### 日期计算
1. 最方便的方式是将时间转为毫秒值进行计算
```
Date from =new Date();
Thread.sleep(200);//线程休眠2ms
Date to =new Date();
System.out.println(to.getTime()-from.getTime());
```

### 高精度时间
```
long time1 =System.nanoTime();
System.out.printf(time1+"");
```
- 说明:System.nanoTime()提高了ns级别的精度,1ms=1000000ns,

### javascript日期
1. 获取时间的毫秒值

2. 格式化时间

3. 获取月份，时间
