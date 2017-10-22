title: 突破算法第三天-希尔排序
date: 2017-10-22 21:51:03
tags: 算法
categories: algorithm
---
** {{ 希尔排序 }}：** <Excerpt in index | 首页摘要>
    希尔排序平常用的比较少，主要是基于插入排序的改进。但是希尔排序的性能很高，数组越大，性能优势越明显。
<!-- more -->
<The rest of contents | 余下全文>

## 希尔排序的基本原理
基本思想：先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录“基本有序”时，再对全体记录进行依次直接插入排序。  
操作方法：
1. 选择一个增量序列t1，t2，…，tk，其中ti>tj，tk=1； 
2. 按增量序列个数k，对序列进行k 趟排序； 
3. 每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m 的子序列，分别对各子表进行直接插入排序。仅增量因子为1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

![希尔排序原理图](http://o7kalf5h3.bkt.clouddn.com/shellSort.jpg)

## 希尔排序java实现
```java
    public static void shellSort(int[] a) {
        int n = a.length;
        int h = 1;
        while (h < n/3) h = 3*h + 1;
        while (h >= 1) {
            // h-sort the array
            for (int i = h; i < n; i++) {
                for (int j = i; j >= h && (a[j]< a[j-h]); j -= h) {
                    swap(a, j, j-h);
                }
            }
            h /= 3;
        }
    }
    private static void swap(int[] a, int i, int j) {
        int swap = a[i];
        a[i] = a[j];
        a[j] = swap;
    }
```
## 算法复杂度
希尔排序时效分析很难，关键码的比较次数与记录移动次数依赖于增量因子序列d的选取，是一个不稳定排序算法

## 适用场景







> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
