title: 突破算法第六天-冒泡排序
date: 2017-10-25 22:06:06
tags: algorithm
categories: 算法
---
** {{ title }}：** <Excerpt in index | 首页摘要>
冒泡排序也非常简单，效率比较低。了解即可。
<!-- more -->
<The rest of contents | 余下全文>

## 冒泡排序的原理
在要排序的一组数中，对当前还未排好序的范围内的全部数，自上而下对相邻的两个数依次进行比较和调整，让较大的数往下沉，较小的往上冒。即：每当两相邻的数比较后发现它们的排序与排序要求相反时，就将它们互换。

![冒泡排序图](http://o7kalf5h3.bkt.clouddn.com/bubbleSort.jpg)

## 冒泡排序的java实现
```java
private static void bubbleSort(int a[], int n) {
    for (int i = 0; i < n - 1; ++i) {
        for (int j = 0; j < n - i - 1; ++j) {
            if (a[j] > a[j + 1]) {
                int tmp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = tmp;
            }
        }
    }
}
```
## 算法复杂度
冒泡排序的复杂度为O(n^2)



> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
