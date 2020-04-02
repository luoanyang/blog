---
title: 记录自己leetcode刷题
date: 2020-03-04 20:12:10
tags: [算法]
category: 数据结构和算法
---

因为自己的算法很薄弱，所以准备在leetcode上刷题目，先从简单的但开始。标题取自leetcode上的题目。

## 难度（简单）

### 两数之和
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
[点击查看原题](https://leetcode-cn.com/problems/two-sum/)
代码：
```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    // 创建一个字典
    const m = {};
    for(let i=0;i<nums.length;i++){
        // 检查当前值是否对应的上 m 的 key
        if(m[nums[i]]>=0){
            return [m[nums[i]],i]
        }
        // 存储 （target - 当前的nums下标的值）对应 坐标
        m[target-nums[i]] = i;
    }
};
```



### 整数反转
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
[点击查看原题](https://leetcode-cn.com/problems/reverse-integer/)
代码：
```js
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function (x) {
    if (x === 0) return x;
    let MAX = Math.pow(2, 31) - 1;
    let MIN = Math.pow(-2, 31);
    // 反转
    let res = parseInt(x.toString().split('').reverse().join(''));
    if (x < 0) {
        res = `-${res}`;
    }
    if (res > MAX || res < MIN) return 0;
    return res;
};
```

### 回文数
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
[点击查看原题](https://leetcode-cn.com/problems/palindrome-number/)
```js
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    if(x<0) return false;
    const nArr = x.toString().split('');
    let length = nArr.length
    for(let i=0;i<length;i++){
        if(nArr[i]!=nArr[length-1-i]){
            return false;
        }
    }
    return true;
};
```

### 反转链表
反转一个单链表。
[点击查看原题](https://leetcode-cn.com/problems/reverse-linked-list/)
其中方法一是自己想的，方法二看了评论
代码
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * 方法1
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
    console.log(head)
    if (!head) return head;
    let key = [];
    key.push(head.val);
    let next = head.next;
    while (next) {
        key.push(next.val);
        next = next.next;
    }
    key.reverse();
    let res,temp;
    for (let i = 0; i < key.length; i++) {
        if(i===0){
            res = { val: key[i] };
            if (i < key.length - 1) {
                res.next = {};
                temp = res.next;
            }
        }else {
            temp.val = key[i]
            if (i < key.length - 1) {
                temp.next = {};
                temp = temp.next;
            }
        }
    }
    return res;
};
```

## 难度（中等）

## 难度（困难）