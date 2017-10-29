title: 突破算法第七天-堆排序
date: 2017-10-26 22:50:57
tags: 算法
categories: algorithm
---
** {{ title }}：** <Excerpt in index | 首页摘要>
堆排序是利用二叉树的原理实现的一种排序，难点在于要构建堆,构建堆一般可以采用下沉或者上浮的算法进行。
<!-- more -->
<The rest of contents | 余下全文>

## 堆排序的基本原理
初始时把要排序的n个数的序列看作是一棵顺序存储的二叉树（一维数组存储二叉树），调整它们的存储序，使之成为一个堆，将堆顶元素输出，得到n 个元素中最小(或最大)的元素，这时堆的根节点的数最小（或者最大）。然后对前面(n-1)个元素重新调整使之成为堆，输出堆顶元素，得到n 个元素中次小(或次大)的元素。依此类推，直到只有两个节点的堆，并对它们作交换，最后得到有n个节点的有序序列。称这个过程为堆排序。
因此，实现堆排序需解决两个问题：
1. 如何将n 个待排序的数建成堆；
2. 输出堆顶元素后，怎样调整剩余n-1 个元素，使其成为一个新堆。

## 堆排序java实现
```java
public static void sort(int[] a) {
        int n = a.length;
        for (int k = n / 2; k >= 1; k--)
            sink(a, k, n);
        while (n > 1) {
            swap(a, 1, n--);
            sink(a, 1, n);
        }
    }

private static void sink(int[] a, int k, int n) {
    while (2 * k <= n) {
        int j = 2 * k;
        if (j < n && a[j - 1] < a[j + 1 - 1]) j++;
        if (a[k - 1] >= a[j - 1]) break;
        swap(a, k, j);
        k = j;
    }
}

private static void swap(int[] a, int i, int j) {
    int swap = a[i - 1];
    a[i - 1] = a[j - 1];
    a[j - 1] = swap;
}

public static void main(String[] args) {
    int[] arr = {49, 38, 65, 97, 76, 13, 27, 4, 78, 34, 12, 64, 1, 8};
    sort(arr);
    System.out.println("排序之后：");
    System.out.println(Arrays.toString(arr));
}
```
## 算法复杂度
堆排序的平均时间复杂度为Ο(nlogn)




> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
