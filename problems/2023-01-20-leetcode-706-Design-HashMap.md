---
title: LeetCode, 706. Design HashMap

date: 2023-1-20

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/design-hashmap/description/)

해시 테이블을 구현하라.

# 문제접근

1. `선형 프로빙` 방식과 `개별 체이닝` 방식 중에 선형 프로빙 방식을 적용하였다. 문제에서 get, put, remove 연산 총 합하여 만 번 이루어질 수 있다고 하였으므로 배열의 크기를 십만으로 설정하였다. key 값은 0부터 백만까지라서 모듈로 연산으로 해시하였다.

2. 선형 프로빙 방식에서 주의해야할 점은, 요소를 삭제할 때 null로 바꾸면 안 된다는 점이다. null을 만나면 선형 검색을 중단하도록 하였는데, 삭제 시에는 중간에 있는 요소도 지울 수 있으므로 이것이 선형검색을 중단하도록 하면 안 된다. 따라서 더미값을 넣어줘서 검색을 계속하도록 해준다.

# 코드

```javascript
/var Node = function(key, value){
    this.key = key;
    this.value = value;
}

var MyHashMap = function() {
    this.size = 10**5;
    this.array = Array(this.size).fill(null);
};

/**
 * @param {number} key
 * @param {number} value
 * @return {void}
 */
MyHashMap.prototype.put = function(key, value) {
    let keyIndex = this.findKeyIndex(key);
    this.array[keyIndex] = new Node(key, value);
};

/**
 * @param {number} key
 * @return {number}
 */
MyHashMap.prototype.get = function(key) {
    let keyIndex = this.findKeyIndex(key);
    return this.array[keyIndex] === null ? -1 : this.array[keyIndex].value;
};

/**
 * @param {number} key
 * @return {void}
 */
MyHashMap.prototype.remove = function(key) {
    let keyIndex = this.findKeyIndex(key);
    if(this.array[keyIndex] !== null) {
        this.array[keyIndex] = new Node();
    }
};

MyHashMap.prototype.findKeyIndex = function (key){
    let hash = key % this.size;
    while(
        this.array[hash] !== null &&
        this.array[hash].key !== key
    ){
        hash += 1;
        if(hash === this.size) hash = 0;
    }
    return hash;
}

/**
 * Your MyHashMap object will be instantiated and called as such:
 * var obj = new MyHashMap()
 * obj.put(key,value)
 * var param_2 = obj.get(key)
 * obj.remove(key)
 */
```
