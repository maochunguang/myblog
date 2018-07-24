title: 突破算法第五天-选择排序
date: 2017-10-24 21:46:33
tags: 算法
categories: algorithm
---
** {{ title }}：** <Excerpt in index | 首页摘要>
选择排序很简单，属于交换排序算法。通过比较找到最大值或最小值，然后进行交换。
<!-- more -->
<The rest of contents | 余下全文>

## 选择排序的原理
首先找到数组中最小的元素，与数组第一个元素交换，然后在剩下的元素中选择最小的，与第二个元素交换，以此类推，直到排序完成。  
![选择排序图](http://o7kalf5h3.bkt.clouddn.com/selectSort.jpg)

## 选择排序的java实现
```java
    private static void sort(int[] a) {
        int len = a.length;
        for (int i = 1; i < len; i++) {
            for (int j = i; j > 0 && (a[j] < a[j - 1]); j--) {
                swap(a, j, j - 1);
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
选择排序的算法复杂度是O(n^2)

## 改进
1. 每次选择的时候把最大值和最小值都比较出来，双向进行交换排序







> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
