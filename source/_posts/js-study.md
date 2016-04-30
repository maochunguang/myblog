title: js秘密花园
date: 2016-04-23 10:24:09
tags: javacript
categories: 学习笔记
---
** {{ title }}：** <Excerpt in index | 首页摘要>
    js学习中遇到的问题,非常实用!
+ <!-- more -->部分来自js秘密花园,其他都是自己的总结
<The rest of contents | 余下全文>

## 函数与匿名函数的写法
- 函数声明
    1. 第一种方式
    ```
    function foo() {}
    ```
    2. 第二种方式
    ```
    var foo = function() {};
    ```
    3. 第三种方式
    ```
    var foo = function bar() {
        bar(); // 正常运行
    }
    bar(); // 出错：ReferenceError
    ```
- 匿名函数
    1. 第一种方式
    ```
    (function(){
            test();
        })();
    ```
    2. 第二种方式
    ```
    (function(){
            test();
        }());
    ```

## js中for循环
- 为了达到遍历数组的最佳性能，推荐使用经典的 for 循环
    1. 经典for循环
    ```
    var arr = ['aa','bb','cc'];
    for(var i=0;i<arr.length;i++){
        console.log(arr[i]);
    }
    ```
    2. for in循环(可以循环对象的属性),for in 循环同样在查找对象属性时遍历原型链上的所有属性
    ```
    var arr = ['aa','bb','cc'];
    for(var i in arr){
        console.log(arr[i]);
    }
    ```
    3. forEach循环
    ```
    var arr = ['aa','bb','cc'];
    arr.forEach(function(ele){
        console.log(ele);
    });
    ```

## this的用法
- js中最复杂的莫过于的this的指向,此处大致介绍五种this的指向
    1. 全局范围内,this指向全局,浏览器中是window对象
    2. 函数调用,也是指向全局
    ```
    foo();
    ```
    3. 对象方法调用,指向调用者
    ```
    test.foo();
    ```
    4. 构造函数,指向新创建的对象
    ```
    new foo();
    ```
    5. 显示改变的this的指向
    ```
    function foo(a, b, c) {}
    var bar = {};
    foo.apply(bar, [1, 2, 3]); // 数组将会被扩展，如下所示
    foo.call(bar, 1, 2, 3); // 传递到foo的参数是：a = 1, b = 2, c = 3
    ```
## call和apply的解惑
- 这两个方法的用途都是在特定的作用域中调用函数,实际上等于设置函数体内 this 对象的值。首先, apply() 方法接收两个参数:一个是在其中运行函数的作用域,另一个是参数数组。其中,第二个参数可以是 Array 的实例,也可以是arguments 对象。例如:
    ```
    function sum(num1, num2){
        return num1 + num2;
    }
    function callSum1(num1, num2){
        return sum.apply(this, arguments);
    }
    function callSum2(num1, num2){
        return sum.apply(this, [num1, num2]);
    }
    console.log(callSum1(10,10));
    console.log(callSum2(10,10));
    // 传入 arguments 对象
    // 传入数组
    //20
    //20
    ```
- call() 方法与 apply() 方法的作用相同,它们的区别仅在于接收参数的方式不同。对于 call()
方法而言,第一个参数是 this 值没有变化,变化的是其余参数都直接传递给函数。换句话说,在使用
call() 方法时,传递给函数的参数必须逐个列举出来,如下面的例子所示:
    ```
    function sum(num1, num2){
        return num1 + num2;
    }
    function callSum(num1, num2){
        return sum.call(this, num1, num2);
    }
    cosole.log(callSum(10,10)); //20
    ```
- 事实上，传递参数并非 apply()和 call()真正的用武之地；它们真正强大的地方是能够扩充函数
赖以运行的作用域。下面来看一个例子。
    ```
    window.color = "red";
    var o = { color: "blue" };
    function sayColor(){
        alert(this.color);
    }
    sayColor(); //red
    sayColor.call(this); //red
    sayColor.call(window); //red
    sayColor.call(o); //blue
    ```
## js面向对象
- 未完待续
