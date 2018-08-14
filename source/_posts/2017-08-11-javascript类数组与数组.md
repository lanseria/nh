---
title: Javascript类数组转化为数组
date: 2017-08-11 16:32:58
categories:
 - 技术
tags:
 - javascript
 - call
 - 前端
 - 数组
 - 类数组
 - 面试
---
> 刚认识到这个知识点应该不晚吧~ 想想面试应该也会考到这类题，所以拿出来说说。
不过前天的面试估计没过，还是挺惋惜的，主要还是我态度不怎样，一是觉得是培训公司（这么大的一个招牌挂在上面，心里看上去就挺不爽的）无所谓了。二来还是觉得太急，想晚点找个好点的工作，这次也就当 `test` 吧。
不过面试中，我以为她都懂（就是一个正式的懂前端的人），现在想想，她就是拿着答案对我的口题，对我的笔试题，我想想就气。不知道技术的人拿着那种答案有什么用，只会一个劲儿的点点头，CTMD！（早知道，我一个劲的吹逼就行了，浪费我大一堆口舌 🙄）

切入正题，什么是类数组，它在我们频繁的操作中有什么关系，为什么要用到数组？
 <!-- more -->
其实，一切都要从对象还是讲起，毕竟JavaScript中任何类型都是基于一个对象
有一篇国外文章可能对对象，数组，讲得更好，本篇文章会部分引用他的案例[advanced-javascript-objects-arrays-and-array-like-objects](http://www.nfriedly.com/techblog/2009/06/advanced-javascript-objects-arrays-and-array-like-objects/)

# 什么是类数组
> 我这里说的类数组是广义上的类数组，其解释就类似于能通过`[].prototype.slice.call(ARRAYLIKE)`转化为数组的形式。
到目前为止，我用这类类数组转化为数组的方式，用在了许多地方

1. `Buffer` 对象，这是有点让我不解的地方
2. `DOM` 获取的同 `className` 的 `DOM` 节点群的类数组，当然还有同 `tagName`
3. 最后一种就是大家都知道的典型例子， `arguments`
所以呢，除了第一个，经常用js写前端的你，应该会碰到下面两种的转化

# 什么是数组
什么是数组，也就是为了讲明什么要将类数组转化为数组的原因
数组有很多好处，但这些好处都一并归纳在了这个数组对象所定义的方法里
比如说，堆栈队列的操作，pop(),push(),shift(),
排序的操作，sort()
还有很多方法可以使用，这里也就不在一一列举
数组与类数组有两点相似
1. 都内部都自己的子元素
2. 都有定义的长度

但是长度这里也比较奇怪的是
```js
var arr = [];
arr[0] = "cat"; // this adds to the array
arr[1] = "mouse"; // this adds to the array
arr.length; // returns 2

arr["favoriteFood"] = "pizza"; // this DOES NOT add to the array. Setting a string parameter adds to the underlying object
arr.length; // returns 2, not 3
```
The length property is only modified when you add an item to the array, not the underlying object.
长度这个属性，只会在你添加或删除一个元素的时候与触发，当你自己定义它（这个数组）的属性时，它是不会增加的（就如同又来了一个`length2`属性）。

而且，当你直接定义一个比较远的数组索引时，比如100，这时，length总会比其索引值大1，而且不会报错，然后，中间的其它值会暂时的定义为undefined，以便填充



# 坑 Gotchas

> 其实这篇文章已经讲完了，不过还是要总结下，这期间遇到的`坑 Gotchas`

1. DOM获取的class节点还是tagname节点都是一群对象组成一个类数组
2. 最好使用一些库函数，such as lodash.js 将这些类数组进行转化
