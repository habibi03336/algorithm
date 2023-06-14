---
title: LeetCode, 450. Delete Node in a BST

date: 2023-2-10

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/delete-node-in-a-bst/description/)

BST의 루트와 삭제하고자 하는 노드가 주어진다. 해당 노드가 삭제된 BST의 루트를 반환하라.

- 노드가 BST에 없을 경우 기존 BST를 그대로 반환한다.

# 문제접근

1. 삭제할 노드를 찾고, 해당 노드를 제외한 자식 노드에 대해서 BST를 다시 구축하여 풀어줄 수 있다.

# 코드

```java
class Solution {

    private TreeNode constructBstByArrayList(List<TreeNode> list){
        if(list.size() == 0) return null;
        TreeNode root = list.get(list.size() / 2);
        TreeNode left = constructBstByArrayList(list.stream().limit(list.size() / 2).collect(Collectors.toList()));
        TreeNode right = constructBstByArrayList(list.stream().skip((list.size() / 2) + 1).collect(Collectors.toList()));
        root.left = left;
        root.right = right;
        return root;
    }

    private TreeNode reconstructTreeWithoutRoot(TreeNode root){
        ArrayList<TreeNode> nodes = new ArrayList<TreeNode>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        if(root.left != null) stack.push(root.left);
        if(root.right != null) stack.push(root.right);
        while(stack.size() != 0){
            TreeNode node = stack.pop();
            nodes.add(node);
            if(node.left != null) stack.push(node.left);
            if(node.right != null) stack.push(node.right);
        }
        Comparator<TreeNode> comparator = new Comparator<TreeNode>() {
            @Override
            public int compare(TreeNode a, TreeNode b) {
                return a.val - b.val;
            }
        };
        nodes.sort(comparator);
        return constructBstByArrayList(nodes);
    }

    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null) return root;
        TreeNode parent = null;
        TreeNode tracer = root;
        while(tracer != null){
            if(tracer.val == key) {
                TreeNode subTree = reconstructTreeWithoutRoot(tracer);
                if(parent == null) {
                    root = subTree;
                    break;
                }
                if(parent.left == tracer){
                    parent.left = subTree;
                } else {
                    parent.right = subTree;
                }
                break;
            }
            parent = tracer;
            if(tracer.val > key){
                tracer = tracer.left;
            } else {
                tracer = tracer.right;
            }
        }
        return root;
    }
}
```
