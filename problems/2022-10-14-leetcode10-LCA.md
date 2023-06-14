---
title: LeetCode, Least Common Anscestor of a Binary Search Tree

date: 2022-10-15

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

binary tree의 root와 두 노드가 주어진다. 두 노드의 공통된 조상 중 가장 가까운 조상(가장 덜 올라가는, LCA)을 반환하라.

- 조상에는 자기 자신도 포함된다.
- 노드의 개수는 10,000
- 모든 노드의 val은 unique
- 주어지는 두 노드는 서로 다르다
- 두 노드는 반드시 binary tree에 있다.

# 문제접근

1. 몇 가지 발견

- 어떤 노드에서 왼쪽과 오른쪽을 나누었을 때, 만약 왼쪽, 오른쪽에 찾는 노드가 하나 씩 있으면 그 노드가 LCA가 된다.
- 어떤 노드를 subroot로 하는 tree에 찾는 두 노드가 있고, 어떤 노드가 찾는 노드 중 하나라면 그 노드가 LCA가 된다.
- 만약 왼쪽 혹은 오른쪽 한 쪽에 찾는 노드 두개가 모두 있다면 해당 subtree를 기준으로 다시 탐색해주면 된다.

2. 시간복잡도  
1/2씩 탐색을 하기 때문에 N/2 + N/4 + N/8 + N/16 ... 이다. 따라서 O(N)

# 코드

\*tip: left 혹은 right 한 쪽에 대해서만 몇 개인지 확인해주면 반대쪽은 자동으로 알 수 있다. 이걸 고려하지 않고 양쪽다 각각 개수를 세는 코드를 짰을 떄는 163ms가 걸려서 리트코드 하위 10% 정도의 성능을 보여줬다. 이를 고려하여 아래처럼 수정해주니 83ms로 상위 5% 정도 성능이 나왔다.

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function (root, p, q) {
	const childs = ["left", "right"];

	const existNum = (subRoot) => {
		let findNum = 0;
		const stack = [subRoot];
		while (stack.length !== 0) {
			const node = stack.pop();
			if (node === p || node === q) findNum++;
			for (let child of childs) {
				if (node[child] !== null) stack.push(node[child]);
			}
		}
		return findNum;
	};

	let subRoot = root;
	while (true) {
		if (subRoot === p || subRoot === q) return subRoot;

		const leftExistNum = existNum(subRoot.left);
		if (leftExistNum === 2) {
			subRoot = subRoot.left;
		} else if (leftExistNum === 0) {
			subRoot = subRoot.right;
		} else {
			return subRoot;
		}
	}
};
```
