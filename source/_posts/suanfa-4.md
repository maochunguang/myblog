title: 突破算法第一天-归并排序
date: 2017-10-23 23:56:27
tags: 算法
categories: algorithm
---
** {{ title }}：** <Excerpt in index | 首页摘要>
归并排序是利用分治思想进行排序的典型应用，特别是对几个基本有序的子序列合并时，效率最高。在实际应用中，分布式应用，分布式查询排序会比较多应用到归并排序。
<!-- more -->
<The rest of contents | 余下全文>

## 归并排序的原理
归并（Merge）排序法是将两个（或两个以上）有序表合并成一个新的有序表，即把待排序序列分为若干个子序列，每个子序列是有序的。然后再把有序子序列合并为整体有序序列。
归并排序分为两种种，第一种是自底向上的归并。

第二种是自顶向下的归并。

## 自底向上的归并排序java实现
```java
public class MergeSortBU {
    private static void merge(int[] a, int[] aux, int lo, int mid, int hi) {
        // 复制到aux[]
        for (int k = lo; k <= hi; k++) {
            aux[k] = a[k];
        }
        // 合并回 a[]
        int i = lo, j = mid + 1;
        for (int k = lo; k <= hi; k++) {
            if (i > mid) a[k] = aux[j++];
            else if (j > hi) a[k] = aux[i++];
            else if (aux[j] < aux[i]) a[k] = aux[j++];
            else a[k] = aux[i++];
        }
    }

    public static void mergeSort(int[] a) {
        int n = a.length;
        int[] aux = new int[n];
        for (int len = 1; len < n; len *= 2) {
            for (int lo = 0; lo < n - len; lo += len + len) {
                int mid = lo + len - 1;
                int hi = Math.min(lo + len + len - 1, n - 1);
                merge(a, aux, lo, mid, hi);
            }
        }
    }

    public static void main(String[] args) {
        int[] arr = {49, 38, 65, 97, 76, 13, 27, 4, 78, 34, 12, 64, 1, 8};
        mergeSort(arr);
        System.out.println("排序之后：");
        System.out.println(Arrays.toString(arr));
    }
}
```
## 自顶向下的归并排序java实现
```java
private static void sort(int[] a, int low, int high) {
        if (high <= low) return;
        int mid = low + (high - low) / 2;
        sort(a, low, mid);
        sort(a, mid + 1, high);
        merge(a, low, mid, high);
    }

    private static void merge(int[] a, int lo, int mid, int hi) {
        // 复制到aux[]
        int[] aux = new int[a.length];
        for (int k = lo; k <= hi; k++) {
            aux[k] = a[k];
        }
        // 合并回 a[]
        int i = lo, j = mid + 1;
        for (int k = lo; k <= hi; k++) {
            if (i > mid) a[k] = aux[j++];
            else if (j > hi) a[k] = aux[i++];
            else if (aux[j] < aux[i]) a[k] = aux[j++];
            else a[k] = aux[i++];
        }
    }
```
## 算法复杂度
归并排序的算法复杂度是nlgn

## 应用场景
1. 几个基本有序的数组进行排序
2. 部分有序的数组



> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
