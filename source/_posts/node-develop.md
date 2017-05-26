title: nodejs开发规范
date: 2016-05-23 14:18:02
tags: node
categories: 编程语言
---
** nodejs开发规范：** <Excerpt in index | 首页摘要>
    nodejs开发中应当遵循的规范，以及最佳实践
<!-- more -->
<The rest of contents | 余下全文>

## node开发需要编程规范吗？
1. js的灵活性非常大，如果开发人员每个人都按自己的习惯随意编写，js的代码会非常混乱不堪。js程序员需要更强的自律性和规范，才能写出易读性，易维护的代码。
2. 随着前端mvc的崛起，前端的js代码会更加庞大难以管理，如果没有统一的规范，后期维护会比登天还难。

## 编码规范

1. 缩进
采用两个空格缩进，在编辑器中设置tab为两个空格

2. 变量声明
- 用var声明变量
var assert = require('assert');
var fork = require('child_process').fork;
var net = require('net');

错误实例：
var assert = require('assert')
, fork = require('child_process').fork
, net = require('net')；

- 用字面量声明方式
var num = 123;
var aaa = {};
var arr = [];
var isAdmin = true;
- 避免使用：
var obj =new Object();
var arr = new Array();
var test  =new String("");
var size = new Number();

- 不要在for循环等循环里声明var变量
首先var是函数作用域，在循环声明以后只有等函数声明周期结束这些资源才会释放


3. 空格
在操作符前后需要加上空格,= 、% 、* 、- 、+ 前后都应该加一个空格
比如：var foo = 'bar' + baz;
错误实例：var foo='bar'+baz;

4. 单双引号的使用
在node中尽量使用单引号，
var html = '<a href="http://cnodejs.org">CNode</a>';
 在json中使用双引号

5. 分号
给表达式结尾加分号，尽管js会自动在行尾加上分号，但是会产生一些误解

## 命名规范
在编码中，命名是重头戏。好的命名可以使代码赏心悦目，具有良好的维护性。

1. 变量命名
变量名采用小驼峰命名，单词之间没有任何符号如：
var adminUser = {};
var callNum = 2134323;
2. 方法命名
也是采用小驼峰命名，与变量不同的是采用动词或判断行词汇，如：
var getUser = function(){};
var isAdmin = function(){};
var findUser = function(){};

3. 类命名
类名采用大驼峰，所有单词首字母大写，如：
function User{}

4. 常量命名
作为常量，单词所有字母大写，用下划线分割，如：
var PINK_COLOR = "PINK";

5. 文件命名
命名文件时，尽量使用下划线分割单词，比如child_process.js和string_decode.js

6. 包名
在包名中尽量不要包含js和node的字样，应当适当短并且有意义

## 其它要点

1. 作用域
慎用with和eval（），容易引起作用域混乱

2. 比较操作
尽量使用===代替==,否则会遇到下面的情况，'0'==0;//true;
 ''==0;//true;
 '0'===''//false;

3. 严格模式
在node后台中尽量全使用严格模式
'use strict';

4. 对象和数组遍历
数组遍历使用普通for循环，避免使用for in对数组遍历，
对象的遍历使用for in

## 项目中实践
1. sublime和webstorm都有JSLint,JSHint这样的代码质量工具，在配置文件中制定好模板规范即可

2. 在版本控制工具中设置hook，在precommit的脚本中设置，如果代码不符合标准，就无法提交

##  参考文献
1. 深入浅出nodejs
2. js秘密花园
3. js高级编程
