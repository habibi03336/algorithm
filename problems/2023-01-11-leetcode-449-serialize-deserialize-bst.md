---
title: LeetCode, 449. Serialize and Deserialize BST

date: 2023-1-11

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/serialize-and-deserialize-bst/)

이진 탐색 트리를 serialize하고 다시 deserialize하는 두 함수를 구현하여라.

- serialize란 메모리 상 논리적인 자료구조를 네트워크 전송, 파일로 저장 등을 하기위해 물리적인 구조로 바꿔주는 것을 의미한다. deserialize는 serialize의 반대이다.

# 문제접근

1. 이진트리는 배열로 표현할 수 있다. 배열로 표현된 이진트리를 문자열로 간단히 반환할 수 있다.

# 구현

1. 모든 존재하지 않는 노드에 대해서 모두 null을 넣어 배열로 표현할 수 있다. 이렇게 하면 트리 노드의 배열상 위치가 항상 같다. 따라서 deserialize 할 때 재귀적인 dfs를 활용하는 등 보다 쉽게 할 수 있다. 하지만 이렇게하면 한 level에 노드가 1개만 있어도 빈 곳을 전부 null로 채워줘야 하기 때문에 비효율적이다. 시간 및 공간복잡도가 최악의 경우 (O(N\*2^N))이다.

2. 더 이상 노드가 존재하지 않는 위치에만 null을 넣는 방식을 사용할 수 있다. 이 방식을 이용하면 노드의 배열상 위치가 트리 모양에 따라서 바뀐다. 하지만 여전히 que를 활용해 온전하게 deserialize 할 수 있다. 이 방식은 복잡도가 O(N)이다.

# 코드

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * Encodes a tree to a single string.
 *
 * @param {TreeNode} root
 * @return {string}
 */
var serialize = function (root) {
	const nodeArray = [];
	const que = [root];
	while (que.length !== 0) {
		const node = que.shift();
		if (node === null) {
			nodeArray.push(null);
		} else {
			nodeArray.push(node.val);
			que.push(node.left);
			que.push(node.right);
		}
	}
	return nodeArray.join(",");
};

/**
 * Decodes your encoded data to tree.
 *
 * @param {string} data
 * @return {TreeNode}
 */
var deserialize = function (data) {
	if (data === "") return null;
	const nodeArray = data.split(",");
	const root = new TreeNode(nodeArray[0]);
	let idx = 1;
	const que = [root];
	while (que.length !== 0) {
		const node = que.shift();
		if (nodeArray[idx] !== "") {
			node.left = new TreeNode(nodeArray[idx]);
			que.push(node.left);
		}
		idx += 1;
		if (nodeArray[idx] !== "") {
			node.right = new TreeNode(nodeArray[idx]);
			que.push(node.right);
		}
		idx += 1;
	}
	return root;
};
```
