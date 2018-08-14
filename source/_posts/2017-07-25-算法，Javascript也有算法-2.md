---
title: 算法，Javascript也有算法(2)
date: 2017-07-25 21:25:33
categories:
 - 技术
 - 面试
tags:
 - javascript
 - 算法
 - 回文数
---
>**提示** 这系列文章我会时常更新，时间也会相对应的更新，如果对前文想略过，可以直接点击这里[今天来讲什么的？我只能说小算法](/2017/07/23/算法，Javascript也有算法/#今天来讲什么的？我只能说小算法)

今天算是比较熟悉面试（牛客网）中的算法流程，虽然 `node v0.12` 版本的很恶心，但还是可以用的
 <!-- more -->
## 一、数据结构
参考：http://www.thatjsdude.com/interview/linkedList.html
### 链表
链表（Linked list）是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序存储数据，而是在每一个节点里存到下一个节点的指针(Pointer)。
### 单链表（单向链表）
链表中最简单的一种是单向链表，它包含两个域，一个信息域和一个指针域。这个链接指向列表中的下一个节点，而最后一个节点则指向一个空值。
{% img https://upload.wikimedia.org/wikipedia/commons/thumb/6/6d/Singly-linked-list.svg/408px-Singly-linked-list.svg.png 一个单向链表包含两个值: 当前节点的值和一个指向下一个节点的链接 %}
任何语言都有千万种表示方式去呈现链表，这里我写出两种，一种是在参考上的，一种是参考leetcode上的链表
One:
```js
function LinkedList(){
  this.head = null;
}

LinkedList.prototype.push = function(val){
  var node = {
    value: val,
    next: null
  }
  if(!this.head){
    this.head = node;
  }
  else{
    current = this.head;
    while(current.next){
      current = current.next;
    }
    current.next = node;
  }
}
var sll = new LinkedList();
sll.push(2);
sll.push(3);
sll.push(4);

console.log(sll);
console.log(sll.head);
console.log(sll.head.next);
console.log(sll.head.next.next);

```
Two: 
```js
function ListNode(val) {
  this.val = val;
  this.next = null;
}
ListNode.prototype.push = function(val){
  var node = new ListNode(val);
  var current = this;
  while(current.next){
    current = current.next
  }
  current.next = node;
}
```
### 双链表
一种更复杂的链表是“双向链表”或“双面链表”。每个节点有两个连接：一个指向前一个节点，（当此“连接”为第一个“连接”时，指向空值或者空列表）；而另一个指向下一个节点，（当此“连接”为最后一个“连接”时，指向空值或者空列表）
{% img https://upload.wikimedia.org/wikipedia/commons/thumb/5/5e/Doubly-linked-list.svg/610px-Doubly-linked-list.svg.png 一个双向链表有三个整数值: 数值, 向后的节点链接, 向前的节点链接 %}
```js
class DoublyLinkedList {
  constructor() {
    this.head = null;
  }
  push(val) {
    var head = this.head;
    var current = head;
    var previous = head;
    if (!head) {
      this.head = {
        value: val,
        previous: null,
        next: null
      };
    } else {
      while (current && current.next) {
        previous = current;
        current = current.next;
      }
      current.next = {
        value: val,
        previous: current,
        next: null
      };
    }
  }
}
var dll = new DoublyLinkedList();
dll.push(2);
dll.push(5);
console.log(dll.head.next.previous);
```
## 二、算法
### 1.回文数的最小个数
这里我只能大致讲一下题目意思，百度也有，也是只有 `JavaScript` 做的，所以我这里把题目抄一下：
> **Description**
提供一个字符串 `s` ，其中每个字符都是小写字母。并提供字符串长度。
要求：输字符串 `s` 中元素拼凑出的回文串的最小个数。其中，每个字符只能使用一次。
例如： `s="abbaa"` ，输出 `1` ，因为最少可以拼凑出 `"ababa"` 这一个回文串。
 `s="abc"` ，输出 `3` ，因为最少只能拼凑出 `"a"，"b"，"c"` 这三个回文串。

其实说实话，毫无思路，因为你不可能枚举那没多种情况，在网上看到题解之后，也懂了（秒懂）。
因为在一个随机的字符串中，如果需要求最少的回文个数的话，只要找其中每个字母的个数就可以了，其中呢，偶数完全不需要考虑，但是奇数个是整个回文数的核心，同个字母有几个个数就有几个回文数。偶数的字母只要靠边站就行了。
有思路就有动力了，用一个Map就可以了，再用一个数组将Map中的值包括起来，对奇数个数进行计算，就可以得出最后的结果了。
```js
'use strict';

function count(str) {
  str = String(str);
  if (!str) {
    return 0;
  }
  var nums = str.split('').reduce(function (map, s) {
    return map.set(s, (map.get(s) || 0) + 1);
  }, new Map());
  return Array.from(nums.values()).reduce(function (odd, n) {
    return n % 2 !== 0 ? odd + 1 : odd;
  }, 0) || 1;
}


```