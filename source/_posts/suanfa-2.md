title: 突破算法第二天-插入排序
date: 2017-10-21 09:41:27
tags: 算法
categories: algorithm
---
** {{ 插入排序 }}：** <Excerpt in index | 首页摘要>
    今天是突破算法第二天，插入排序，比较简单。效率比较低，但是思想很广泛，应用很广，是很多高级排序算法的一个子过程。
<!-- more -->
<The rest of contents | 余下全文>

## 插入排序的原理
 将一个记录插入到已排序好的有序表中，从而得到一个新，记录数增1的有序表。即：先将序列的第1个记录
 看成是一个有序的子序列，然后从第2个记录逐个进行插入，直至整个序列有序为止。
 要点：设立哨兵，作为临时存储和判断数组边界之用

![插入排序原理](http://o7kalf5h3.bkt.clouddn.com/insert.jpg)

## 插入排序java实现
```java
 public static void insertSort(int[] a, int n) {
        for (int i = 0; i < n; i++) {
            for (int j = i; j > 0 && (a[j]<a[j-1]); j--) {
                swap(a, j, j-1);
            }
        }
    }
    private static void swap(int[] a, int i, int j) {
        int swap = a[i];
        a[i] = a[j];
        a[j] = swap;
    }
```
## 算法复杂度
插入排序的复杂度为O（n^2）

## 改进方法
希尔排序，其他的插入排序有二分插入排序，2-路插入排序。

## 适用场景
插入排序比较适合部分有序的数组（以下四种数组）
* 数组中每个元素距离它的最终位置都不远
* 一个有序的大数组接一个小数组
* 数组中只有几个位置不正确
* 数组比较小



> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
