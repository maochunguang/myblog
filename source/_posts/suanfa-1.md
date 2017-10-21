title: 突破算法第一天-快速排序
date: 2017-10-20 23:46:59
tags: 算法
categories: algorithm
---
** {{ 30天突破算法之快速排序 }}：** <Excerpt in index | 首页摘要>
30天突破算法是我给自己定的一个学习计划，希望在这30天，每天都能完成计划。第一天学习最重要的快速排序。
<!-- more -->
<The rest of contents | 余下全文>

## 30天突破算法
算法种类不计其数，说30天突破只是给自己定的学习计划。目的是通过30天的记录熟悉常见的算法，提高自己的算法能力。对以后的工作来说也是打下夯实的基础。

## 快速排序的原理
快速排序也是分治法思想的一种实现，他的思路是使数组中的每个元素与基准值（Pivot，通常是数组的首个值，A[0]）比较，数组中比基准值小的放在基准值的左边，形成左部；大的放在右边，形成右部；接下来将左部和右部分别递归地执行上面的过程：选基准值，小的放在左边，大的放在右边。重复此过程，直到排序结束。步骤如下：
* 1.找基准值，设Pivot = a[0] 
* 2.分区（Partition）：比基准值小的放左边，大的放右边，基准值(Pivot)放左部与右部的之间。
* 3.进行左部（a[0] - a[pivot-1]）的递归，以及右部（a[pivot+1] - a[n-1]）的递归，重复上述步骤。

![快速排序原理图](http://o7kalf5h3.bkt.clouddn.com/quicksort.jpg)

## 快速排序java实现（递归版）
```java
public class QuickSort {
    public static void main(String[] args) {
        int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1,8};
        System.out.println("排序之前：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
        //快速排序
        quick(a);
        System.out.println();
        System.out.println("排序之后：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
    }

    private static void quick(int[] a) {
        if(a.length>0){
            quickSort(a,0,a.length-1);
        }
    }

    private static void quickSort(int[] a, int low, int high) {
        if(low<high){ //如果不加这个判断递归会无法退出导致堆栈溢出异常
            int middle = getMiddle(a,low,high);
            quickSort(a, 0, middle-1);
            quickSort(a, middle+1, high);
        }
    }

    private static int getMiddle(int[] a, int low, int high) {
        int temp = a[low];//基准元素
        while(low<high){
            //找到比基准元素小的元素位置
            while(low<high && a[high]>=temp){
                high--;
            }
            a[low] = a[high]; 
            while(low<high && a[low]<=temp){
                low++;
            }
            a[high] = a[low];
        }
        a[low] = temp;
        return low;
    }
}
```

## 快速排序三向切分法（改进的实现）
```java
    private static void quick3Sort(int[] a, int low, int high) {
        if (low >= high) return;
        int lt = low, gt = high;
        int temp = a[low];
        int i = low;
        while (i <= gt) {
            if (a[i] < temp) swap(a, lt++, i++);
            else if (a[i] > temp) swap(a, i, gt--);
            else i++;
        }
        quick3Sort(a, low, lt - 1);
        quick3Sort(a, gt + 1, high);
    }

    private static void swap(int[] a, int i, int j) {
        int swap = a[i];
        a[i] = a[j];
        a[j] = swap;
    }
```

## 快速排序的复杂度
时间复杂度 nlogn,排序方法中平均性能最好的。但若初始序列按关键码有序或基本有序时，快排序反而蜕化为冒泡排序。快速排序是一个不稳定的排序方法。

## 改进方法
1. 当数组比较小的时候，快速排序比插入排序慢，这个时候用插入排序替换比较好。
2. 通常以“三者取中法”来选取基准记录，即将排序区间的两个端点与中点三个记录关键码居中的调整为支点记录

## 适用场景
- 普通的无序集合排序，使用快速排序。 
- 包含很多重复元素的集合排序，使用三向切分的快速排序。
- 基本有序的集合使用归并排序。




> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
