---
title: "Kth Ele in Binary Search Tree CN"
date: 2021-02-27T10:22:19+08:00
description: ""
tags: ["binary search tree", "leetcode"]
series: ["data structure"]
categories: ["Algorithms"]
draft: true
---


# LeetCode 二叉查找树中第 K 小的元素 II
给定一个二叉搜索树，我们希望找到其中第 `k` 小的元素。

在这道题目中，除了原始的二叉搜索树 `root` 以外，你还会得到一个和其结构完全一致的二叉树 `nodenum_root`，树上节点的值代表以该节点为根的子树的节点数量。

知道了以每个节点为根的子树的节点数量，那么根据当前根节点左右子树的节点数量以及 k，我们可以直接推断出目标点在当前节点的左子树还是右子树上。这样就可以直接递归下去找到目标点，时间复杂度从 `O(n)` 降低为 `O(h)`，`h` 为树的高度。


```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
// v1 recurisive, without nodenum_root, 0ms 38.2
// order to access subtree: left  root  right
 class Solution {

    private int result;
    private int k;

    public int kthSmallestII(TreeNode root, TreeNode nodenum_root, int k) {
        this.k = k;
        kthSmallestII(root);
        return this.result;
    }

    private void kthSmallestII(TreeNode root) {
        if (root == null) return;
        // search left root right
        kthSmallestII(root.left);
        k--; // go back to root, count the 1st, 2nd...smallest
        if (k  == 0) this.result = root.val;
        kthSmallestII(root.right);
    }

}

*/

// v2 use nodenum_root  O(n) -> O(h)
// order to access subtree of nodenum_root: root left right
// 0 ms 38.3MB
class Solution {
    public int kthSmallestII(TreeNode root, TreeNode nodenum_root, int k) {

        // if (root == null) return -1;

        // root_num = left_num + right_num + 1
        int left_num = nodenum_root.left == null ? 0 : nodenum_root.left.val;

        // root is the result
        if (left_num + 1 ==  k) {
            return root.val;
        } else if (left_num + 1 < k) { // result is in the right subtree
            return kthSmallestII(root.right, nodenum_root.right, k - left_num - 1);
        // } else if (left_num + 1 > k){ // result is in the left subtree
        } else {
            return kthSmallestII(root.left, nodenum_root.left, k);
        }

    }
}
```
