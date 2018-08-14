---
title: 算法，Javascript也有算法(1)
date: 2017-07-23 21:11:07
categories:
 - 技术
 - 面试
tags:
 - javascript
 - 算法
 - 数据结构
 - 尺取法
 - 堆栈
 - 队列
 - 优先队列
---
>**提示** 这系列文章我会时常更新，时间也会相对应的更新，如果对前文想略过，可以直接点击这里[今天来讲什么的？我只能说小算法](/2017/07/23/算法，Javascript也有算法/#今天来讲什么的？我只能说小算法)

# 前言
现在还没工作的我，对IT互联网的技术也有自己的一番认知，同时尽量把我所知道的传播给下面的学弟学妹，让她们提提力气，不要只局限于老师所教的知识就以为，以为计算机这个大专业就这么点东西。
虽然身在电科这个很·棒的专业，但我也在日常与室友计科软工的带领下，自己走向一条对自己适合的道路。
什么都会点，还是不太精，什么都做过点项目，但是成品还是有点粗糙。我的理念也不像那些呆在实验室的人一样，老师给什么，就做什么。老师提什么，我都会跟他一块去讨论，将别人的想法稍稍改变下，总的来说，使项目朝着自己所期望的做，所以一般情况下，项目转交给别人，一般就传不下去了……
 <!-- more -->
# 废话又扯多了，算法，咳咳
说实话，大一参加ACM我竟然退了，现在开始想想也挺后悔的，更后悔的是还在一个现在根本不想呆的地方，浪费了我这么多时间。
说到ACM，难免会提到算法，可能大一的我没能认识到算法的重要性，觉得它并有没实际的优越感，就放弃了，想想很可惜的。
好的程序 = 优秀的算法 + 可靠的数据结构
万年不变的真理啊，但真正能掌握他们的人，少之又少。
# Javascript需要算法吗？
甚至更加简单一点的，前端需要算法吗？
可能很多人都会说，不就是写界面吗？还需要这么高深的算法吗？
对，你们所知道的前端还是过去的前端，属于设计师与代码混合的年代，而现在分工却更加的明确。前端需要更多的知识填充，而这些知识包括，websocket，tcp，http，网络，linux这些后端才会遇到的知识。所以，想当然，算法是完全有必要的。
# 今天来讲什么的？我只能说小算法
个人比较谦虚，真是不会太难的算法，所以这里只能将我了解到的算法，在这里给你们分享下，当然，介绍算法的同时不会忘记介绍JavaScript中的数据结构。

- 推荐的OJ：
  + [LeetCode](https://leetcode.com/)

- 阅读推荐：
  + [算法导论](https://book.douban.com/subject/20432061/)
  + [算法设计与分析基础](https://book.douban.com/subject/26337727/)
  + [程序员代码面试指南：IT名企算法与数据结构题目最优解](https://book.douban.com/subject/26638586/)

## 一、数据结构
参考：http://www.thatjsdude.com/interview/linkedList.html
### 1.堆栈和队列
如果不是 JavaScript 的新手，应该知道JavaScript是已经自己实现了堆栈和队列。
你只需要简单的调用 `push` ， `pop` ， `shift` 这些操作 `array` 的函数即可。
不过，你甚至对C++中的优先队列有点印象的话，也有人实现了类似的原型构造，
[priorityQueueJS的简化版本](https://github.com/janogonzalez/priorityqueuejs/blob/master/index.js)
堆栈：
```js
var myStack = [];

//push
myStack.push(1);
myStack.push(2);
myStack.push(3);

//pop
myStack.pop(); //3
myStack.pop(); //2
myStack.pop(); //1
```
队列：
```js
var myQueue = [];

//push
myQueue.push(1);
myQueue.push(2);
myQueue.push(3);

//pop
myQueue.shift(); //1
myQueue.shift(); //2
myQueue.shift(); //3
```
Priority Queue优先队列
```js
const PriorityQueue = require('./priorityqueue')
var p = new PriorityQueue();
p.enq(1);
p.enq(3);
p.enq(2);

console.log(p.deq())
console.log(p.deq())
console.log(p.deq())
console.log(p);
```
## 二、算法
算法不像数据结构，每个人都有每个人的想法，思维的不一样，导致写出来的代码精炼程度也不大一样，所以，我这里在我自己与室友的帮助下，会写下自己在 [LeetCode](https://leetcode.com/)中的题解，与之对应的算法。
### 1.尺取法
> 去一块一块地去截取你所要的序列，一旦不满足就后自增1，前再去探索，使其满足要求，也如同蚯蚓的爬动，用O(n)的复杂度，求得最优值。

![图解](http://images.cnitblog.com/blog/597004/201408/291224259702079.jpg)
![图解](https://yuhaomin.github.io/uploads/images/1-1%E5%B0%BA%E5%8F%96%E6%B3%95.png)
尺取法我想我这种程度是想不到类似巧妙的算法的，我之前的算法自己想想就觉得太复杂了。而且呢。没有AC掉题，代码真是又臭又长。之后寻求了室友的帮助提示，他告诉了我尺取法来解决这类序列不动求其最长或最短的方法，一下子就把问题变得简单了。
所以以后遇到类似的问题，不用在意是数字还是字母，只要要求序列不排序，就可以用此方法解决。

举个 [LeetCode](https://leetcode.com/) 中较为形象的题型

#### 1.例题：求最长子字符串
[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/
)
> **Description**
Given a string, find the length of the longest substring without repeating characters.
Examples:
Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.
Given `"bbbbb"`, the answer is `"b"`, with the length of 1.
Given `"pwwkew"`, the answer is `"wke"`, with the length of 3. Note that the answer must be a substring, `"pwke"` is a subsequence and not a substring.

这里它还提醒我们跟之前的题不太一样，这个序列不准变，不是求最长子序列，是最长子字符串
来说说我之前的复杂的想法吧，首先将字符串转化成数组，然后慢慢去用 `new Map()` 这类数据类型去找不一样的字符串，如果不对记录下其长度，放入数组中，最后求出最大值，
！但是这个解决方案看似完美，但有致命的缺点，就是忽略了第一次确定了那个最长序列之中的有可能的最长序列。所以让我们还是用最优的方案吧--尺取法

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s){
  var m = new Map();
  var Imax = 0;
  var i = 0, j = 0;
  while(1){
    while(j<s.length && !m.has(s.charAt(j))){
      m.set(s.charAt(j));
      j++; 
    }
    Imax = Math.max(Imax, j-i);
    if(j >= s.length)
      break;
    m.delete(s.charAt(i++));
  }
  return Imax;
};
```
**说明**
这个整体思路就相当清晰了，除了避免了字符串变数组的多余的循环，也添加了尺取法的构思，可看到，while这个语句在算法中是经常能用到的。
首先利用es6的新特性，map数据结构，使这个数据有数据有字典的属性，可以方便的查询。然后定义头与尾，用头去探索，尾部去迭代，只要头部探索完毕，就释放尾部进1。使头部继续去探索。最后求得最大值。
