title: 突破算法第八天-桶排序
date: 2017-10-27 22:51:06
tags: 算法
categories: algorithm
---
** {{ title }}：** <Excerpt in index | 首页摘要>
桶排序是个神奇的排序，在某些情况下可以达到O(N)的复杂度，快的离谱。但是桶排序是利用空间换时间，在空间充足的情况下，可以用桶排序进行高效的排序。
<!-- more -->
<The rest of contents | 余下全文>

## 桶排序的基本原理
将阵列分到有限数量的桶子里。每个桶子再个别排序（有可能再使用别的排序算法或是以递回方式继续使用桶排序进行排序）。当要被排序的阵列内的数值是均匀分配的时候，桶排序使用线性时间（Θ（n））。但桶排序并不是 比较排序，他不受到 O(nlogn) 下限的影响， 简单来说，就是把数据分组，放在一个个的桶中，然后对每个桶里面的在进行排序

## 桶排序的java实现
```java
public static void bucketSort1(int[] arr){
        //分桶，这里采用映射函数f(x)=x/10。
        int bucketCount =10;
        Integer[][] bucket = new Integer[bucketCount][arr.length];
        for (int i=0; i<arr.length; i++){
            int quotient = arr[i]/10;
            for (int j=0; j<arr.length; j++){
                if (bucket[quotient][j]==null){
                    bucket[quotient][j]=arr[i];
                    break;
                }
            }
        }
        //小桶排序
        for (int i=0; i<bucket.length; i++){
            //insertion sort
            for (int j=1; j<bucket[i].length; ++j){
                if(bucket[i][j]==null){
                    break;
                }
                int value = bucket[i][j];
                int position=j;
                while (position>0 && bucket[i][position-1]>value){
                    bucket[i][position] = bucket[i][position-1];
                    position--;
                }
                bucket[i][position] = value;
            }

        }
        //输出
        for (int i=0, index=0; i<bucket.length; i++){
            for (int j=0; j<bucket[i].length; j++){
                if (bucket[i][j]!=null){
                    arr[index] = bucket[i][j];
                    index++;
                }
                else{
                    break;
                }
            }
        }
    }
```
## 算法复杂度
前面说的几大排序算法 ，大部分时间复杂度都是O（n2），也有部分排序算法时间复杂度是O(nlogn)。而桶式排序却能实现O（n）的时间复杂度。
但桶排序的缺点是：
1. 首先是空间复杂度比较高，需要的额外开销大。排序有两个数组的空间开销，一个存放待排序数组，一个就是所谓的桶，比如待排序值是从0到m-1，那就需要m个桶，这个桶数组就要至少m个空间。
2. 其次待排序的元素都要在一定的范围内，限制较多。









> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
