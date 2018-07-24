title: 查找链表倒数第n个元素
date: 2018-02-16 21:45:45
tags: 算法
categories: algorithm
---
** {{ title }}：** <Excerpt in index | 首页摘要>
链表应用很广泛，有单向链表，双向链表。单向链表如何查找倒数第n个元素呢？本文以java代码实现链表反向查找。
<!-- more -->
<The rest of contents | 余下全文>

## 单向链表的定义
单向链表，主要有数据存储，下一个节点的引用这两个元素组成。
```
public class Node {
    int value;
    Node next;

    Node(int value) {
        this.value = value;
    }
}
```

## 遍历倒数第n个元素
在查找过程中，设置两个指针，让其中一个指针比另一个指针先前移k-1步，
然后两个指针同时往前移动。循环直到先行的指针指为NULL时，另一个指针所指的位置就是所要找的位置
算法复杂度为o（n）

```
public Node findDescEle(Node head, int k) {
    if (k < 1 || head == null) {
        return null;
    }
    Node p1 = head;
    Node p2 = head;
    //前移k-1步
    int step = 0;
    for (int i = 0; i < k; i++) {
        step++;
        if (p1.next != null) {
            p1 = p1.next;
        } else {
            return null;
        }
    }
    while (p1 != null) {
        step++;
        p1 = p1.next;
        p2 = p2.next;
    }
    System.out.println("o(n)==" + step);
    return p2;
}
```
## 总结
查找链表倒数第n个元素，复杂度为o(n),使用两个指针即可简单实现。




> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
