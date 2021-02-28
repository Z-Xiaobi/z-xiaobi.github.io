---
title: "二叉树 Binary Tree"
date: 2021-02-23T10:30:42+08:00
description: ""
tags: ["binary tree", "binary search tree"]
series: ["data structure"]
categories: ["Algorithms"]
draft: true
---
# 1 简介
简单来说，二叉树就是每个节点最多只有`2`个子节点的树。

# 2 遍历
当我们介绍数组、链表时，为什么没有着重研究他们的遍历过程呢？

二叉树的遍历又有什么特殊之处？

在计算机程序中，遍历本身是一个线性操作。所以遍历同样具有线性结构的数组或链表，是一件轻而易举的事情。

而二叉树，是典型的非线性数据结构，遍历时需要把非线性关联的节点转化成一个线性的序列，以不同的方式来遍历，遍历出的序列顺序也不同。

二叉树的遍历方式有两大类:
**深度优先遍历** Breadth First Search
(前序遍历、中序遍历、后序遍历）
**广度优先遍历**（层序遍历）

## 2.1 深度优先遍历 DFS
深度优先和广度优先这两个概念不止局限于二叉树，它们更是一种抽象的算法思想，决定了访问某些复杂数据结构的顺序。在访问树、图，或其他一些复杂数据结构时，这两个概念常常被使用到。

所谓深度优先，顾名思义，就是偏向于纵深，「一头扎到底」的访问方式。
### 2.1.1 前序遍历
二叉树的前序遍历，输出顺序是根节点、左子树、右子树。
### 2.1.2 中序遍历
二叉树的中序遍历，输出顺序是左子树、根节点、右子树。
### 2.1.3 后序遍历
二叉树的后序遍历，输出顺序是左子树、右子树、根节点。

### 2.1.4 代码实现
三种遍历方法的实现非常相似，只是在输出 (/使用)时所在的位置不同。
#### 2.1.4.1 递归遍历

```java
/**
 * 构建二叉树
 * @param inputList 输入序列
 */
public static TreeNode createBinaryTree(LinkedList<Integer>inputList){
    TreeNode node = null;
    if (inputList == null || inputList.isEmpty()) {
        return null;
    }
    Integer data = inputList.removeFirst();
    // 这里的判空很关键：如果元素是空，则不在进一步递归
    if (data != null) {
        node = new TreeNode(data);
        node.leftChild = createBinaryTree(inputList);
        node.rightChild = createBinaryTree(inputList);
    }
    return node;
}

/**
 * 二叉树前序遍历
 * @param node 二叉树节点
 */
public static void preOrderTraveral(TreeNode node) {
    if (node == null) {
        return;
    }
    System.out.println(node.data);
    preOrderTraveral(node.leftChild);
    preOrderTraveral(node.rightChild);
}

/**
 * 二叉树中序遍历
 * @param node 二叉树节点
 */
public static void inOrderTraveral(TreeNode node) {
    if (node == null) {
        return;
    }
    inOrderTraveral(node.leftChild);
    System.out.println(node.data);
    inOrderTraveral(node.rightChild);
}

/**
 * 二叉树后序遍历
 * @param node 二叉树节点
 */
public static void postOrderTraveral(TreeNode node) {
    if (node == null) {
        return;
    }
    postOrderTraveral(node.leftChild);
    postOrderTraveral(node.rightChild);
    System.out.println(node.data);
}

/**
    * 二叉树节点
    */
private static class TreeNode {
    int data;
    TreeNode leftChild;
    TreeNode rightChild;

    TreeNode(int data) {
        this.data = data;
    }
}

public static void main(String[] args) {
    LinkedList<Integer> inputList = new LinkedList<Integer>(Arrays.
              asList(new Integer[]{3,2,9,null,null,10,null,
              null,8,null,4}));
    TreeNode treeNode = createBinaryTree(inputList);
    System.out.println("前序遍历：");
    preOrderTraveral(treeNode);
    System.out.println("中序遍历：");
    inOrderTraveral(treeNode);
    System.out.println("后序遍历：");
    postOrderTraveral(treeNode);
}
```
代码中值得注意的一点是二叉树的构建。二叉树的构建方法有很多，这里把一个线性的链表转化成非线性的二叉树，链表节点的顺序恰恰是二叉树前序遍历的顺序。链表中的空值，代表二叉树节点的左孩子或右孩子为空的情况。

在代码的 main 函数中，通过 `{3,2,9,null,null,10,null,null,8,null,4}` 这样一个线性序列，构建成的二叉树如下。

```java
/**
 * 二叉树非递归前序遍历
 * @param root 二叉树根节点
 */
public static void preOrderTraveralWithStack(TreeNode root){
    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode treeNode = root;
    while(treeNode!=null || !stack.isEmpty()){
        //迭代访问节点的左孩子，并入栈
        while (treeNode != null){
            System.out.println(treeNode.data);
            stack.push(treeNode);
            treeNode = treeNode.leftChild;
        }
        //如果节点没有左孩子，则弹出栈顶节点，访问节点右孩子
        if(!stack.isEmpty()){
            treeNode = stack.pop();
            treeNode = treeNode.rightChild;
        }
    }
 }

```
#### 2.1.4.2 非递归遍历
绝大多数可以用递归解决的问题，其实都可以用另一种数据结构来解决，这种数据结构就是栈。因为递归和栈都有回溯的特性。
```java
/**
 * 二叉树非递归前序遍历
 * @param root 二叉树根节点
 */
public static void preOrderTraveralWithStack(TreeNode root){
    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode treeNode = root;
    while(treeNode!=null || !stack.isEmpty()){
        //迭代访问节点的左孩子，并入栈
        while (treeNode != null){
            System.out.println(treeNode.data);
            stack.push(treeNode);
            treeNode = treeNode.leftChild;
        }
        //如果节点没有左孩子，则弹出栈顶节点，访问节点右孩子
        if(!stack.isEmpty()){
            treeNode = stack.pop();
            treeNode = treeNode.rightChild;
        }
    }
 }

```

## 2.2 广度优先遍历 BFS
实际上，BFS就是把树一层一层遍历

```java
/**
 * 二叉树层序遍历
 * @param root 二叉树根节点
 */
public static void levelOrderTraversal(TreeNode root){
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.offer(root);
    while(!queue.isEmpty()){
        TreeNode node = queue.poll();
        System.out.println(node.data);
        if(node.leftChild != null){
            queue.offer(node.leftChild);
        }
        if(node.rightChild != null){
            queue.offer(node.rightChild);
        }
    }
 }

```
题目：先序遍历构造二叉树
返回与给定前序遍历 preorder 相匹配的二叉搜索树（binary search tree）的根结点。

(回想一下，二叉搜索树是二叉树的一种，其每个节点都满足以下规则，对于 `node.left` 的任何后代，值总 < `node.val`，而 `node.right` 的任何后代，值总 > `node.val`。此外，前序遍历首先显示节点 `node` 的值，然后遍历 `node.left`，接着遍历 `node.right`。）

题目保证，对于给定的测试用例，总能找到满足要求的二叉搜索树
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

// construct a binary search tree from given list in preorder
// v1 stack
// 2ms 36.4MB

class Solution {
    public TreeNode bstFromPreorder(int[] preorder) {

        TreeNode root =  new TreeNode(preorder[0], null, null);
        // stack to store roots， use push and pop to add and remove from first-end
        Deque<TreeNode> rootStack =  new ArrayDeque<TreeNode>();
        rootStack.push(root);
        TreeNode curr =  root; // current node

        for (int i = 1; i < preorder.length; i++) {

            int value = preorder[i];
            TreeNode currRoot = rootStack.getFirst();
            TreeNode newNode = new TreeNode(value);

            // trace proper root
            while (rootStack.size() > 0) {

                if (rootStack.getFirst().val < value) {
                    currRoot = rootStack.pop();
                } else {
                    break;
                }
            }
            if (currRoot.val < value){
                currRoot.right = newNode;
            } else {
                currRoot.left = newNode;
            }
            rootStack.push(newNode);


        }

        return root;
    }
}

// v2 recursive
/*
class Solution {
    //递归实现
    public TreeNode bstFromPreorder(int[] preorder) {
        int n = preorder.length;
        return createBinaryTree(preorder,0,n-1);
    }
    public TreeNode createBinaryTree(int preorder[],int left,int right){
        if(left>right){
            return null;
        }
        //双亲结点为当前先序序列中的第一个元素
        TreeNode root = new TreeNode(preorder[left]);
        int leftIndex = left;
        int rightIndex = right;
        //找到左子树和右子树的分界点
        //preorder[mid]的值大于双亲结点，说明它属于右子树
        //可以进一步用二分查找优化查找过程
        while (leftIndex < rightIndex) {
            int mid = leftIndex+(rightIndex-leftIndex+1)/2;
            if (preorder[mid] < preorder[left]) {
                leftIndex = mid;
            } else {
                rightIndex = mid-1;
            }
        }
        //递归构造
        root.left = createBinaryTree(preorder,left+1,leftIndex);
        root.right = createBinaryTree(preorder,leftIndex+1,right);
        return root;
    }
}

作者：二货磁铁
链接：https://leetcode-cn.com/leetbook/read/algorithm-and-interview-skills/xu1q2h/?discussion=ewvaCt
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
*/
```
# 3 二叉查找树 Binary Search Tree (BST)


从这个名字来说，二叉查找树肯定是一种二叉树。而查找这个词说明他肯定具有某些便于查找的性质，为了便于查找，其自身也有一些特殊的限制：

小知识：很多高级的数据结构通常都是对常用数据结构增加限制而来，例如栈是只能从尾部插入删除的线性表，堆是限制节点数值关系的完全二叉树等。

对于一棵二叉树，如果满足：

父节点的值大于等于左子树上所有节点的值。
父节点的值小于等于右子树上所有节点的值。
那么这棵二叉树是一棵 二叉查找树。

下图是一个实际的例子：



树中有两个父节点 3 和 8。以节点 8 举例，他的左子树上有 1，3，7，右子树上是 9，显然 8 比左子树上所有数都大，且比右子树上所有数都小，所以满足定义限定条件。同样节点 3 也满足相关条件，因此这棵树是一个二叉查找树。

思考：如何编写一个程序检查一棵树是否为二叉查找树？

二叉查找树的中序遍历
在树的章节，我们讲过树的很多遍历方式，其中中序遍历的顺序是左子树 -> 根 -> 右子树。对于二叉查找树而言，因为根和左右子树的大小关系性质，中序遍历得到的序列一定是 有序序列。这是一条非常重要的性质。

单选题
判断一下说法是否正确：

一棵二叉搜索树能确定唯一一个有序序列, 但一个有序序列能不能确定唯一一棵二叉搜索树。原因就是，树是可以旋转的，也就是说，在维持大小左右关系的同时，根节点可以左旋或者右旋，从而将其它节点点变成根节点。打个比方，手里面有一


## 3.1 二叉查找树的操作
从二叉查找树的名字就可以看出BST的主要目的就是查找。查找的内容包括最大/最小值，任意值，某个值的前一个/后一个值。这些操作均可以在 `O(h)` 的时间内完成，其中 `h` 为树的高度。

查找最大/最小值
前面提到，对于二叉查找树来说，左子树节点值 <= 父节点值 <= 右子树节点值，因此如果一个节点存在左子树，那么左子树上一定有更小的节点，且右子树上不会有更小的节点。那么，我们只要从根节点一直往左子树走，最后就能到达值最小的节点。

而对于最大值的查询也是同样的道理，我们只需要一直往右子树走，最后就能得到最大值节点了。

由于过程中只会从根节点走到某个叶子，所以时间复杂度为 O(h)O(h)，hh 为树的高度。

思考：如何编写一个程序查询二叉查找树中的最大值？

查找任意值是否存在
根据大小值递归向子树去查找

查找任意值第一次/最后一次出现的位置
现在我们需要考虑存在重复值的查找场景，在 C++ 中，查找区分了 upper_bound 和 lower_bound 两种，分别对应了查找不大于某个数的第一个数，以及大于某个数的第一个数。当相同的数字多次出现时，我们经常需要知道他第一次和最后一次出现的位置。

考虑下面的树，如果我们按照刚才提到的方法来查找，最终会落到粉色节点上，然后返回。



但是在数字重复的情况下，我们不能确定当前节点是否是第一个值为 1 的节点。于是在这里不退出，继续往左子树上查找。这时会发生两种情况：

左子树根节点是 1。 如果左子树根节点仍然是 1，那么显然可以继续递归左子树查找是否还有更前面的 1，即 如果目标值小于等于根节点，在左子树上继续查找。


左子树根节点不是1。 如下例，这说明了左子树根节点比 1 更小，但是 不能说明左子树上就没有值为 1 的节点了。


因此，就算当前子树节点的左孩子已经小于目标值了，查找仍然不能停止。我们需要继续在其右子树上进行查找是否还有目标值。即 如果目标值比根节点大，则在右子树查找。

最后，访问结束后我们最后一次访问到的目标值节点，就是该节点第一次出现的位置了。

由于查找同样最多从根走到叶子，时间复杂度为 O(h)O(h)。

思考：如何编写一个程序查询一棵二叉查找树中某个值最后一次出现的位置？

查找任意值的前驱/后继节点
前驱节点的位置
接下来，我们会来介绍如何找到一个节点的前一个节点和后一个节点，也就是所谓的前驱和后继。

小知识：对于二叉查找树来说，其中序遍历正好是有序结果。在中序遍历结果中，各个节点的出现次序，即为节点的次序。因此，某个节点的前驱和后继节点，分别对应其在中序遍历中前一个和后一个节点。

下图中序遍历结果为 [1,3,7,8,9]，因此节点 7 的前一个节点是 3，后一个节点是 8。



来思考一个实际的例子，如下图。现在要求 5 的前驱节点。



由于二叉查找树的性质，我们知道节点的前驱节点一定不大于该节点。因此 5 的前驱节点肯定是小于 5 的。所以如果当前节点有左子树，那么前驱节点一定在 左子树 上。

那么如果当前节点没有左子树，是不是他就没有前一个节点了呢？显然并不是这样的。下面这个例子里，对于节点 8 来说，他的中序遍历中的前一个节点实际上是节点 7。

目前我们还不知道如何找到节点 7，但是我们可以知道 前一个节点还可能存在于当前节点子树以外的部分。

到这里，我们发现了两个前驱节点可能的存在位置，分别是：

+ 当前节点的左子树
+ 当前子树以外的部分

前驱节点的位置情形
假设我们要查找节点 node 的前驱节点，我们分别来考虑他的父节点和左孩子的存在情况。

父节点和左孩子同时存在。

node 为父节点的左孩子。这种情况下，父节点比 node 大，显然不可能为其前驱节点。因此前驱节点在左子树上。

node 是父节点的右孩子。这种情况下，node 的父节点一定不大于 node 的左孩子。所以 node 的左孩子在遍历访问顺序上更靠近 node，因此 node 的父节点也不可能为 node 的前一个节点，前驱节点在左子树上。



综上，如果左子树和父节点同时存在。那么前驱节点在左子树上。

只有父节点存在。那么前驱节点范围只能是当前子树以外的部分了。



只有左孩子存在。同理，前驱节点只能在左子树上。
如果合并一下可以发现：

如果左孩子存在，那么前驱节点在左子树上。否则：
如果父节点存在，前驱结点在当前子树以外的部分。
如果左孩子和父节点都不存在，无前驱节点。
查找前驱节点
确定了前驱节点的位置，接下来就是真正找到前驱节点了。

前驱节点在 左子树上。要找当前节点最靠近的前一个节点，实际上等价于询问左子树上最大的节点了，利用前面提到的查询最大值的方法即可。

前驱节点在 当前子树以外的部分。这是更复杂的情况，如果左孩子不存在了，那么前一个节点只有可能出现在当前子树的以外部分。如果我们要找 node 的前驱节点，假设 fnode 为 node 的父节点：

node 为 fnode 右孩子。 因为 fnode 的左子树比 fnode 还要小，在遍历顺序上不会比 fnode 更靠近 node 。所以这种情况下 node 的前一个节点为 fnode

node 为 fnode 左孩子，且 fnode 为根节点。 这时显然 fnode 要大于 node，那么如果fnode 为根节点的话，node 就没有前一个节点了。


node 为 fnode 左孩子，且 fnode 不为根节点。 以下图为例，假设 node 为节点 8，对应的 fnode 为节点 9。其前驱结点是 7。


首先，不妨一直顺着 node 的父节点往上看：

第一个是 9。8 是 9 的左孩子，这也表明了 9 是大于 8 的，所以 9 和 9 的右子树不可能是 8 的前驱节点。
再往上走一层到节点 7。9 是 7 的右孩子，这说明 7 是小于 9 的；而 9 在 7 的右子树上，这也说明了 8 也在 7 的右子树上。也就是说我们现在知道了 7 这个节点是小于 8 的，7 可能是一个前驱节点。
知道 7 是一个可能的前驱节点之后，我们现在需要确定还有没有其他节点在 7 和 8 之间。

先考虑 7 的左子树。7 已经小于 8 了，所以 7 的左子树上不会有 7 到 8 之间的数了。
再来考虑 7 的父节点。如果 7 是父节点的左孩子，那么父节点会比 8 还要大，不会在 7 到 8 之间。如果 7 是父节点的右孩子，那么 7 和 8 都大于父节点了，显然也不会有其他节点在 7 到 8 之间。所以这种情况下，可以确定 7 就是 8 的前驱节点。
考虑另外一个例子。在这种情况下，6 是 7 的前一个节点。



但是这里需要顺着 node 往上走 3 层。这也说明了，当 fnode 是左孩子时，我们需要不停往上找，一直到出现右孩子的情况，才算找到了前驱节点。

总结一下过程，如果只有父节点存在的话：

遍历 node 的所有祖先节点 [f1,f2..] ，再将 node (用 f0 表示)放入得到 [f0,f1,f2…]，其中 f1 是 f0 的父节点，f2 是 f1 的父节点，以此类推。
从前往后，如果存在 fi-1 是 fi 右孩子,则 fi 为 node 的前一个节点。
若未发现满足条件的关系，那么 node 无前一个节点。
思考：如何编写一个程序查找一棵二叉查找树上某个节点的后一个节点？

查找前驱操作的时间复杂度为：`O(h)`，`h` 为树高



### 1.3.2 插入节点
小知识：二叉查找树的优势在于不光支持各种查询，同样支持对树结构的修改。
从根节点开始，用插入元素进行比较。首先，如果插入的元素小于等于当前节点。如果当前节点的左子树不为空的话，那么递归左子树继续比较。
第二种情况，如果插入元素大于当前节点，若当前节点节点右子树不为空，递归右子树继续比较。

那么对于插入节点小于当前节点的情况，如果当前节点的左子树为空，直接插入到左子树。
对应的，如果要插入的节点大于当前节点，且当前节点的右子树为空，那么插入右子树。

上述加粗部分即为插入的思路，非常简单：

根据当前节点和待插入节点的大小关系，决定往左或者右子树走。
如果走的方向子树为空，那么就插入，否则递归插入过程。

### 1.3.3 删除节点
最后是如何删除节点。对于删除的节点所处的位置不同，处理也会不一样，我们需要分类讨论。

首先，如果删除的节点是叶子节点的话，显然直接将他删除也不会有什么影响。即 叶节点直接删除

如果待删除的节点只有一个孩子 的话,可以直接 将其父节点和子节点相连，显然这样也仍然满足二叉查找树的性质。

这里需要注意一种特殊情况，如果待删除点是根节点的话，他是没有父节点的，需要将其孩子作为根节点。

待删除点有两个孩子

○ 代表单个节点
△ 代表子树
N 是待删除节点
F 是 N 的父节点所在的树其他部分
L 和 R 分别代表左子树和右孩子
R 节点右边的三角形 R 代表 R 节点的右子树。
回到问题，N 有两个孩子，我们要删除 N 的话，可以先找到其后一个节点 X,也就是其后继节点，因为右子树存在，所以 X 一定会在右子树中。

那么 X 会有左孩子么？如果 X 还有左孩子，那么 X 的左孩子肯定比 X 在中序遍历顺序上更靠近 N ，这也说明了 X 节点是一定没有左孩子。

现在删除就很简单了：

首先将 N 和 X 交换，因为 X 是 N 的后继，所以不会影响其余点的顺序。
原来 X 最多就一个孩子，所以现在的 N 最多也只有一个孩子了，可以很方便地删除。
思考：如何编写一个程序删除二叉查找树中的某个节点？
二叉查找树 VS 有序数组
最后，为了说明二叉查找树的查找性能，我们将其和有序数组进行比较：

|操作|	二叉查找树|	有序数组|
|:----|:----------|:---------|
|查询最大值	|`O(h)`|	`O(1)`|
|查询任意值	|`O(h)`|	`O(log(n))`|
|查询前一个/后一个值	| `O(h)`|	`O(1)`|
|插入	| `O(h)`|	`O(n)`|
|删除	|`O(h)`|	`O(n)`|

对于查询最大值的操作，有序数组只需要取最后一个数就可以了，也就是 `O(1)`，优于二叉查找树。

对于查询任意值，对于有序数组我们使用二分查找可以做到 `O(log(n))`，而对应的二叉查找树是 `O(h)`，因此这里也会比二叉查找树更优。

而对于查询前一个和后一个值，对于有序数组显然直接取其下标后一个树就可以了，因此也要优于二叉查找树。

对于插入和删除操作，使用有序数组都需要 `O(n)` 的时间来维护，而对应的二叉查找树只需要 `O(h)`，也就是最优情况下 `O(log(n))`的复杂度。所以我们说二叉查找树相比于有序数组，在不降低太多查询代价的情况下，极大地提高了可维护性。

而实际上，二叉查找树是可以通过平衡调整，使得操作代价上界降低为 `O(log(n))` ，这也是大名鼎鼎的平衡树(Balanced Tree)数据结构。

小知识：

在所有节点左右子树高度尽量一致的情况下(参考完全二叉树)。这时 nn 个节点的树高度为 `O(log(n))`O(log(n))。这时所有操作时间复杂度也能从最坏的线性优化到 `O(log(n))`O(log(n))。

平衡二叉树正是因为维护了节点的高度平衡，才能最终做到 `O(log(n))` 级别的插入，删除和查找。注意，二叉查找树的操作时间复杂度为 `O(h)`，`h` 和 `log(n)` 并不等价。只有平衡二叉树的操作代价才为 `O(log(n))` 。

常见的平衡树有 AVL 树，红黑树和 B 树等。



## Reference
- [程序员小灰力扣二叉树的遍历](https://leetcode-cn.com/leetbook/read/journey-of-algorithm/5obws7/)
- [力扣-高频算法实战-二叉查找树理论](https://leetcode-cn.com/leetbook/read/high-frequency-algorithm-exercise/xf4qxs/)
