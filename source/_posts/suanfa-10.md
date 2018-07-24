title: 突破算法第10天-二叉树
date: 2017-10-29 21:17:09
tags: 算法
categories: algorithm
---
** {{ title }}：** <Excerpt in index | 首页摘要>
用java实现算法求出二叉树的高度
<!-- more -->
<The rest of contents | 余下全文>
## 树
* 先序遍历：先访问根结点，然后左节点，最后右节点
* 中序遍历：先访问左结点，然后根节点，最后右节点
* 后续遍历：先访问左结点，然后右节点，最后根节点

## java实现
```java
public class TreeNode {
    TreeNode left;
    TreeNode right;
    int val;

    TreeNode(int val) {
        this.val = val;
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        TreeNode left1 = new TreeNode(2);
        TreeNode left2 = new TreeNode(3);
        TreeNode right1 = new TreeNode(4);
        //创建一棵树
        root.left = left1;
        left1.right = left2;
        root.right = right1;
        scanNodes(root);
        System.out.println("树的深度是：" + getDepth(root));
        System.out.println("非递归深度：" + findDeep2(root));
    }

    // 递归返回二叉树的深度
    static int getDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = getDepth(root.left);
        int right = getDepth(root.right);
        return left > right ? left + 1 : right + 1;
    }

    static void scanNodes(TreeNode root) {
        if (root == null) {
            return;
        }
//        System.out.println(root.val); //先序遍历
        scanNodes(root.left);
//        System.out.println(root.val); //中序遍历
        scanNodes(root.right);
        System.out.println(root.val); // 后序遍历
    }
    // 非递归求深度
    public static int findDeep2(TreeNode root) {
        if (root == null)
            return 0;
        TreeNode current = null;
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int cur, next;
        int level = 0;
        while (!queue.isEmpty()) {
            cur = 0;
            //当遍历完当前层以后，队列里元素全是下一层的元素，队列的长度是这一层的节点的个数
            next = queue.size();
            while (cur < next) {
                current = queue.poll();
                cur++;
                //把当前节点的左右节点入队（如果存在的话）  
                if (current.left != null) {
                    queue.offer(current.left);
                }
                if (current.right != null) {
                    queue.offer(current.right);
                }
            }
            level++;
        }
        return level;
    }
}

```

## 树的变种
二叉查找树，平衡二叉查找树，红黑树，b树
红黑树和平衡二叉树（AVL树）类似，都是在进行插入和删除操作时通过特定操作保持二叉查找树的平衡，从而获得较高的查找性能。红黑树和AVL树的区别在于它使用颜色来标识结点的高度，它所追求的是局部平衡而不是AVL树中的非常严格的平衡。
由于二叉树的效率和深度息息相关，于是出现了多路的B树，B+树等等。b树是叶子为n的平衡树。







> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
