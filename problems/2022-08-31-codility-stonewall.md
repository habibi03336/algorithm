---
title: Codility, StoneWall

date: 2022-8-31

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://codility.com/media/train/solution-stone-wall.pdf)

높이가 들쑥날쑥인 벽을 사각형으로 만들려고 한다. 사각형을 최대한 적게 쓴다고 할 떄 몇 개가 필요한가?

# 문제접근

명확한 공식으로 푸는 문제는 아니라고 생각을 해서 각 위치에서 벽의 높이를 조회해 가면서 greedy한 방식으로 답을 구해보고자 했다.

1. 높이가 높아질 때: 추가적인 사각형이 반드시 필요하다.

2. 높이가 낮아질 때: 이전의 최소치까지의 높이와 비교했을 때 새로운 높이면 추가적인 사각형이 필요하다. 이를 위해서 LIFO로 이전 벽 높이와 비교할 수 있어야 한다.

3. 높이가 같을 때: 추가적인 사각형이 필요없다.

# [코드](https://app.codility.com/demo/results/trainingSYNBA5-MSY/)

```javascript
function solution(H) {
	const stack = [];
	let cnt = 0;
	H.forEach((elem) => {
		if (stack.length === 0) {
			stack.push(elem);
		} else {
			const priorH = stack[stack.length - 1];
			if (priorH < elem) {
				stack.push(elem);
			} else if (priorH > elem) {
				while (stack.length !== 0) {
					const h = stack[stack.length - 1];
					if (h > elem) {
						stack.pop();
						cnt++;
						continue;
					} else if (h < elem) {
						stack.push(elem);
					}
					break;
				}
				if (stack.length === 0) stack.push(elem);
			}
		}
	});

	cnt += stack.length;

	return cnt;
}
```

### 코드 수정

```javascript
function solution(H) {
	const stack = [];
	let cnt = 0;
	H.forEach((elem) => {
		while (stack.length !== 0) {
			const h = stack[stack.length - 1];
			if (h > elem) {
				stack.pop();
				continue;
			} else if (h < elem) {
				stack.push(elem);
				cnt++;
			}
			break;
		}
		if (stack.length === 0) {
			stack.push(elem);
			cnt++;
		}
	});

	return cnt;
}
```
