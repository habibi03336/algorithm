---
title: LeetCode, 112. Path Sum

date: 2023-2-14

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/path-sum/description/)

이진 트리의 root가 주어진다. 또 목표가 되는 정수 값인 targetSum이 주어진다. treed의 root에서 leaf까지 모든 경로에 대해서 경로에 있는 노드의 값을 합쳤을 때 targetSum을 만족하는 경우가 있으면 true 없으면 false를 반환하라.

# 문제접근

1. leaf 노드(자식이 없는 노드)에서 targetSum을 만족하는 경우가 있는지 dfs로 확인해 줄 수 있다.

- 만약 노드가 null이면 부모 노드까지 만족하는 경우가 없었다는 의미이므로 false를 반환한다.
- 노드가 leaf이고 targetSum을 만족하면 true를 반환한다.
- 하나의 경우만 있어도 되기 때문에 or(||)로 최종 결과를 반환한다.

# 코드

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
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root == null) return false;
        if(root.left == null && root.right == null && targetSum - root.val == 0) return true;
        return hasPathSum(root.left, targetSum - root.val) || hasPathSum(root.right, targetSum - root.val);
    }
}
```
