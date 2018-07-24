title: 突破算法第11天-红黑树
date: 2017-10-30 22:35:37
tags: 算法
categories: algorithm
---
** {{ title }}：** <Excerpt in index | 首页摘要>
红黑树
<!-- more -->
<The rest of contents | 余下全文>

## 红黑树
本文的主要内容：
1. 红黑树的基本概念以及最重要的 5 点规则。
2. 红黑树的左旋转、右旋转、重新着色的原理与 Java 实现；
3. 红黑树的增加结点、删除结点过程解析；

## 红黑树的基本概念与数据结构表示

首先红黑树来个定义：

> 红黑树定义：红黑树又称红 - 黑二叉树，它首先是一颗二叉树，它具体二叉树所有的特性。同时红黑树更是一颗自平衡的排序二叉树 (平衡二叉树的一种实现方式)。

我们知道一颗基本的二叉排序树他们都需要满足一个基本性质：即树中的任何节点的值大于它的左子节点，且小于它的右子节点。

按照这个基本性质使得树的检索效率大大提高。我们知道在生成二叉排序树的过程是非常容易失衡的，最坏的情况就是一边倒（只有右 / 左子树），这样势必会导致二叉树的检索效率大大降低（O(n)），所以为了维持二叉排序树的平衡，大牛们提出了各种平衡二叉树的实现算法，如：AVL，SBT，伸展树，TREAP ，红黑树等等。

> 平衡二叉树必须具备如下特性：它是一棵空树或它的左右两个子树的高度差的绝对值不超过 1，并且左右两个子树都是一棵平衡二叉树。也就是说该二叉树的任何一个子节点，其左右子树的高度都相近。下面给出平衡二叉树的几个示意图：

![红黑树](http://img.blog.csdn.net/20170110134212154?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

红黑树顾名思义就是结点是红色或者是黑色的平衡二叉树，它通过颜色的约束来维持着二叉树的平衡。对于一棵有效的红黑树而言我们必须增加如下规则，这也是红黑树最重要的 5 点规则：

1. 每个结点都只能是红色或者黑色中的一种。
2. 根结点是黑色的。
3. 每个叶结点（NIL 节点，空节点）是黑色的。
4. 如果一个结点是红的，则它两个子节点都是黑的。也就是说在一条路径上不能出现相邻的两个红色结点。
5. 从任一结点到其每个叶子的所有路径都包含相同数目的黑色结点。

这些约束强制了红黑树的关键性质: 从根到叶子最长的可能路径不多于最短的可能路径的两倍长。结果是这棵树大致上是平衡的。因为操作比如插入、删除和查找某个值的最坏情况时间都要求与树的高度成比例，这个在高度上的理论上限允许红黑树在最坏情况下都是高效的，而不同于普通的二叉查找树。所以红黑树它是复杂而高效的，其检索效率 O(lg n)。下图为一颗典型的红黑二叉树：
![](http://img.blog.csdn.net/20170110134903553?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

上面关于红黑树的概念基本已经说得很清楚了，下面给出红黑树的结点用 Java 表示数据结构：

```java
private static final boolean RED = true;
private static final boolean BLACK = false;
private Node root;//二叉查找树的根节点

//结点数据结构
private class Node{
    private Key key;//键
    private Value value;//值
    private Node left, right;//指向子树的链接:左子树和右子树.
    private int N;//以该节点为根的子树中的结点总数
    boolean color;//由其父结点指向它的链接的颜色也就是结点颜色.

    public Node(Key key, Value value, int N, boolean color) {
        this.key = key;
        this.value = value;
        this.N = N;
        this.color = color;
    }
}

/**
 * 获取整个二叉查找树的大小
 * @return
 */
public int size(){
    return size(root);
}
/**
 * 获取某一个结点为根结点的二叉查找树的大小
 * @param x
 * @return
 */
private int size(Node x){
    if(x == null){
        return 0;
    } else {
        return x.N;
    }
}
private boolean isRed(Node x){
    if(x == null){
        return false;
    }
    return x.color == RED;
}
```

## 红黑树的三个基本操作

红黑树在插入，删除过程中可能会破坏原本的平衡条件导致不满足红黑树的性质，这时候一般情况下要通过左旋、右旋和重新着色这个三个操作来使红黑树重新满足平衡化条件。

## 旋转

旋转分为左旋和右旋。在我们实现某些操作中可能会出现红色右链接或则两个连续的红链接，这时候就要通过旋转修复。

通常左旋操作用于将一个向右倾斜的红色链接 (这个红色链接链连接的两个结点均是红色结点) 旋转为向左链接。对比操作前后，可以看出，该操作实际上是将红线链接的两个结点中的一个较大的结点移动到根结点上。

左旋的示意图如下：
![](http://img.blog.csdn.net/20170110141248765?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![](http://img.blog.csdn.net/20170110141309245?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

左旋的 Java 实现如下：

```java
/**
 * 左旋转
 * @param h
 * @return
 */
private Node rotateLeft(Node h){
    Node x = h.right;
    //把x的左结点赋值给h的右结点
    h.right = x.left;
    //把h赋值给x的左结点
    x.left = h;
    //
    x.color = h.color;
    h.color = RED;
    x.N = h.N;
    h.N = 1+ size(h.left) + size(h.right);

    return x;
}
```

左旋的动画效果如下：
![](http://img.blog.csdn.net/20170110142027660?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

右旋其实就是左旋的逆操作：
![](http://img.blog.csdn.net/20170110142230957?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![](http://img.blog.csdn.net/20170110142252648?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
右旋的代码如下：

```java
/**
 * 右旋转
 * @param h
 * @return
 */
private Node rotateRight(Node h){
    Node x = h.left;
    h.left = x.right;
    x.right = h;

    x.color = h.color;
    h.color = RED;
    x.N = h.N;
    h.N = 1+ size(h.left) + size(h.right);
    return x;
}
```

右旋的动态示意图：
![](http://img.blog.csdn.net/20170110142410322?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## <a></a>颜色反转

当出现一个临时的 4-node 的时候，即一个节点的两个子节点均为红色，如下图：
![](http://img.blog.csdn.net/20170110143015321?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
我们需要将 E 提升至父节点，操作方法很简单，就是把 E 对子节点的连线设置为黑色，自己的颜色设置为红色。颜色反转之后颜色如下：
![](http://img.blog.csdn.net/20170110143225712?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDg1MzI2MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
实现代码如下：

```java
/**
 * 颜色转换
 * @param h
 */
private void flipColors(Node h){
    h.color = RED;//父结点颜色变红
    h.left.color = BLACK;//子结点颜色变黑
    h.right.color = BLACK;//子结点颜色变黑
}
```

> 注意：以上的旋转和颜色反转操作都是针对单一结点的，反转或则颜色反转操作之后可能引起其父结点又不满足平衡性质。








> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
