---
title: "Binary Search Tree to Double Linked List CN"
date: 2021-02-27T10:25:53+08:00
description: ""
tags: ["binary search tree", "double linked list", "leetcode"]
series: ["data structure"]
categories: ["Algorithms"]
draft: true
---
# LeetCode 二叉搜索树与双向链表

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。
>注意：本题与[Leetcode中国主站 426 题](https://leetcode-cn.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/)相似


我首先想到的解法如下。从最小单元入手，即一个 `size <= 3` 的树结构，情况分为三种：
+ 三个 `Node` : `root` , `left` , `right`
+ 两个 `Node` :
  - `root` , `left`
  - `root` , `right`
+ 一个 `Node` : `root`

无论是哪种情况必然要将节点以 `left <--> root <--> right` 的顺序连接。再向上回溯，这个子树的连接顺序作为一个结果传递给它的根节点。结果就会变成 `left_list_start <-...-> left_list_end <---> root <--> right_list_start <-...-> right_list_end`. 实际上就是 `merge` ( `归并` )操作。最后返回 `left_list_start`。

因此我在 `sortInToLinkedList` 函数里面写了一个比较复杂的判断逻辑，并将 `return type` 设置为 `Node[]` 用来装 `start` 和 `end` 两个指针.

树转换成链表的原则与是将 `left` 指代为前一个 `Node` 的指针，`right` 指代为后一个 `Node` 的指针。
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
// v1 recursive
// sort the subtree , merge left and right
// LinkedList left <--> root <--->right   Node left = pre, Node right = next
// 56 ms -18.13%
// 39.1 MB - 5.18%
class Solution {

    public Node treeToDoublyList(Node root) {

        if (root == null) return null;
        Node smallest = sortInToLinkedList(root)[0];
        return smallest;

    }
    // end of left < root < start of right
    public Node[] sortInToLinkedList(Node root) {

        if (root == null) return null;
        Node[] l = sortInToLinkedList(root.left); // left subtree
        Node[] r = sortInToLinkedList(root.right); // right subtree

        Node smallest = l == null ? root : l[0];
        Node l_end = l == null ?  null : l[1];
        Node r_start = r == null ?  null : r[0];
        Node largest = r == null ? root : r[1];

        // // connect in two directions
        smallest.left = largest;
        if (l_end != null) {
            l_end.right = root;
            root.left = l_end;
        } else {
            root.left = largest;
        }
        if (r_start != null) {
            root.right = r_start;
            r_start.left = root;
        } else {
            root.right = smallest;
        }
        largest.right = smallest;


        // result
        Node[] list = new Node[2];
        list[0] = smallest;
        list[1] = largest;

        // System.out.printf("[%d, %d]\n", smallest.val, largest.val);

        return list;
    }
}
*/
```
剑指 Offer 36 题目解析
解题思路：
本文解法基于性质：二叉搜索树的中序遍历为 递增序列 。
将 二叉搜索树 转换成一个 “排序的循环双向链表” ，其中包含三个要素：

排序链表： 节点应从小到大排序，因此应使用 中序遍历 “从小到大”访问树的节点。
双向链表： 在构建相邻节点的引用关系时，设前驱节点 pre 和当前节点 cur ，不仅应构建 pre.right = cur ，也应构建 cur.left = pre 。
循环链表： 设链表头节点 head 和尾节点 tail ，则应构建 head.left = tail 和 tail.right = head 。

根据以上分析，考虑使用中序遍历访问树的各节点 cur ；并在访问每个节点时构建 cur 和前驱节点 pre 的引用指向；中序遍历完成后，最后构建头节点和尾节点的引用指向即可。

算法流程：
dfs(cur): 递归法中序遍历；

终止条件： 当节点 cur 为空，代表越过叶节点，直接返回；
递归左子树，即 dfs(cur.left) ；
构建链表：
当 pre 为空时： 代表正在访问链表头节点，记为 head ；
当 pre 不为空时： 修改双向节点引用，即 pre.right = cur ， cur.left = pre ；
保存 cur ： 更新 pre = cur ，即节点 cur 是后继节点的 pre ；
递归右子树，即 dfs(cur.right) ；
treeToDoublyList(root)：

特例处理： 若节点 root 为空，则直接返回；
初始化： 空节点 pre ；
转化为双向链表： 调用 dfs(root) ；
构建循环链表： 中序遍历完成后，head 指向头节点， pre 指向尾节点，因此修改 head 和 pre 的双向节点引用即可；
返回值： 返回链表的头节点 head 即可；

复杂度分析：
时间复杂度 O(N)O(N) ： NN 为二叉树的节点数，中序遍历需要访问所有节点。
空间复杂度 O(N)O(N) ： 最差情况下，即树退化为链表时，递归深度达到 NN，系统使用 O(N)O(N) 栈空间。




作者：Krahets
链接：https://leetcode-cn.com/leetbook/read/illustration-of-algorithm/5dj09d/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```python
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        def dfs(cur):
            if not cur: return
            dfs(cur.left) # 递归左子树
            if self.pre: # 修改节点引用
                self.pre.right, cur.left = cur, self.pre
            else: # 记录头节点
                self.head = cur
            self.pre = cur # 保存 cur
            dfs(cur.right) # 递归右子树

        if not root: return
        self.pre = None
        dfs(root)
        self.head.left, self.pre.right = self.pre, self.head
        return self.head

```
```java
class Solution {
    Node pre, head;
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
        dfs(root);
        head.left = pre;
        pre.right = head;
        return head;
    }
    void dfs(Node cur) {
        if(cur == null) return;
        dfs(cur.left);
        if(pre != null) pre.right = cur;
        else head = cur;
        cur.left = pre;
        pre = cur;
        dfs(cur.right);
    }
}

```
``` c++
class Solution {
public:
    Node* treeToDoublyList(Node* root) {
        if(root == nullptr) return nullptr;
        dfs(root);
        head->left = pre;
        pre->right = head;
        return head;
    }
private:
    Node *pre, *head;
    void dfs(Node* cur) {
        if(cur == nullptr) return;
        dfs(cur->left);
        if(pre != nullptr) pre->right = cur;
        else head = cur;
        cur->left = pre;
        pre = cur;
        dfs(cur->right);
    }
};
```
```java
// 0ms 37.8MB
class Solution {
    // head: the pointer that point to min value of linked list
    // pre : previous node of current root
    private Node pre, head;

    public Node treeToDoublyList(Node root) {

        if (root == null) return null;

        treeToDoublyList(root.left);
        if (pre != null) {
            pre.right = root;
        } else {
            head = root;
        }
        root.left = pre;
        pre = root;
        treeToDoublyList(root.right);
        head.left = pre;
        pre.right = head;
        return head;
    }
}
```



# Reference
**1** [力扣-剑指offer36. 二叉搜索树和双向链表](https://leetcode-cn.com/leetbook/read/illustration-of-algorithm/5dbies/)
**2** https://leetcode-cn.com/leetbook/read/illustration-of-algorithm/5dbies/?discussion=UbtO4J
