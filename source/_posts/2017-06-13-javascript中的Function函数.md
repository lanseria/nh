---
title: Javascript 中的 Function 函数
date: 2017-06-13 11:50:27
categories: 
  - 技术
  - 面试
tags:
  - javascript
  - 前端
  - 柯里化
  - currying
---
{% blockquote %}
本文引用的部分链接
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/A_re-introduction_to_JavaScript#内部函数
https://stackoverflow.com/questions/111102/how-do-javascript-closures-work
https://translate.google.cn/#en/zh-CN/Local%20variables%20that%20end%20up%20within%20closure
https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Inheritance
{% endblockquote %}
## 面试题强力分析:
记住这天，人生第一次在场笔试失利，虽然是自己在基础方面不够过关，但是我认为前端对于程序员来说，难度最大的还是在背诵方面，需要记忆的东西还是比一般的来说太多了，比如有一题我真的很少用原生的Js的控制Dom的习惯，题目是div自身点击，然后删除自己本身。
哇，现在看看真的是简单；笔试的时候没有工具是一件十分恶心的事，对于我来说！
 <!-- more -->
{% jsfiddle h5eyrL50 js,html %}
还有一题也是比较恶心，不是我觉得它难，而是觉得它让我回到了高考那个看题时代，题目是这样的：让一个div垂直水平居中。

具体可以参考别人的想法
http://blog.csdn.net/freshlover/article/details/11579669
[慕课前端面试图片](/img/2017-06-13-img1.jpg)
第一，我让有点印象这道题，没错就如同高考中可能会遇到类似题型的感觉

第二点，我就想到你这题没有说用css还是js啊，虽然js我可能不会，但是你不说明的话，只能怪我不客气了，我要写css！

所以第三点我就非常气，css写垂直水平居中到底是什么概念啊！我百度了下就是完全处于中心，嗯，这点我懂了，可以用绝对位置去应对
先看答案

{% jsfiddle ac5wjpvj css,html %}

垂直居中呢？而且200px与200pxd的宽度呢？这都是wrapper没有高度惹的祸
理论上应在根据窗口自适应是最好的，但这肯定要用到js方面的知识
这样不是也可以用margin了吗？
甚至效果更好不是吗？

{% jsfiddle rsk6xu5h css,html %}

其他都不说了，最气的还是有一题根本没遇到过这样的情况，来当作面题了
题目是这样的：
利用js代码实现add(1)(2)和add(1,2)
题目的思想很明显
当你在命令行敲入add(1,2)
返回3
同样的是add(1)(2)
也要返回3
这样的写法在任何语言都是一种语法的报错，但是在js就不同了，可以当时的我真没遇到这样的写法，在写项目也没有老师会用这样的写法，真的是日了狗了，好吧，他就是考的是基础
然后回到寝室，我开始奋力补习，亲切的打开MDN专业文档
https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Basics
先盲目补习下对象的知识
主要现在是正文真正的开始，js中的function是真的重要

## Js中的对象

空对象的定义十分简单
{% codeblock lang:javascript %}
var person = {};
{% endcodeblock %}
复杂的对象

{% codeblock lang:javascript %}
var person = {
  name : ['Bob', 'Smith'],
  age : 32,
  gender : 'male',
  interests : ['music', 'skiing'],
  bio : function() {
    alert(this.name[0] + ' ' + this.name[1] + ' is ' + this.age + ' years old. He likes ' + this.interests[0] + ' and ' + this.interests[1] + '.');
  },
  greeting: function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
  }
};
{% endcodeblock %}

其实，js中几乎什么都是一个对象，比如
{% img https://mdn.mozillademos.org/files/13883/person-diagram.png 一个简单的对象类 %}

{% codeblock lang:javascript %}
myString.split(',');
{% endcodeblock %}

myString继承了String的split方法
还有写DOM的对象这里就不详细地往下讲了

这里讲完对象就不得不讲讲对象中类，类不是js独创，不过在许多语言都用class这个概念，在php，java，c++，c#等等都可以看到
class的使用
其实就是为了继承，比如
{% img https://mdn.mozillademos.org/files/13883/MDN-Graphics-instantiation-2.png 类对象 %}

一个Person 类
在一个Person的基础上你不仅可以创造一个名为王小波的一个具体人的对象
你甚至可以继承Person类去创建一个新的，属于某个更加具体的类，比如老师Teacher()类
Student()类
{% img https://mdn.mozillademos.org/files/13881/MDN-Graphics-inherited-3.png 类继承 %}
其中属性的继承或者方法的重写都是一种很好的代码循环利用，在程序猿的世界，这叫做防止代码冗余，易于别人或自己看懂代码

对象的构造初始化

类的继承

类继承后可以继续创造相应的对象
{% img https://mdn.mozillademos.org/files/13885/MDN-Graphics-instantiation-teacher-3.png 继承后再次对象 %}
不过在Js是如何处理这类OOP理论的问题呢
引用MDN中的一句话，
{% blockquote %}
Some people argue that JavaScript is not a true object-oriented language — for example it doesn't have a class statement for creating classes like many OO languages. JavaScript instead uses special functions called constructor functions to define objects and their features. They are useful because you'll often come across situations in which you don't know how many objects you will be creating; constructors provide the means to create as many objects as you need in an effective way, attaching data and functions to them as required.

来自 <https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object-oriented_JS> 
{% endblockquote %}

Js似乎与其他语言不太一样，它并没有class的语法，在es5中，Js利用的是Function这种特殊的函数去代替class类，这在js中叫做构造器函数

其实这里有的跑题了，但还是要稍微讲一下
js中比较正规的创建一个类的方法是

{% codeblock lang:javascript %}
// 构造器及其属性定义
function Test(a,b,c,d) {
  // 属性定义
};
// 定义第一个方法
Test.prototype.x = function () { ... }
// 定义第二个方法
Test.prototype.y = function () { ... }
// 等等……
{% endcodeblock %}

一个类的方法定义在外面，方法与类之间加prototype原型这个单词

讲完对象，让我们与内部函数或者说闭包做一下对比
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/A_re-introduction_to_JavaScript#内部函数
这里它举例到有一种闭包的例子
{% codeblock lang:javascript %}
function makeAdder(a) {
    return function(b) {
        return a + b;
    }
}
var x = makeAdder(5);
var y = makeAdder(20);
x(6); // ?
y(7); // ?
{% endcodeblock %}

这里不是直接告诉我们答案了吗
这里的makeAdder(a)(b)
肯定是可以
所以这道面试一下子就可以解决了

其实这种写法叫做“柯里化”（'currying'）
[What's the currying?](http://www.ruanyifeng.com/blog/2017/03/ramda.html)
也就是所有多参数的函数，都可以默认单参数去使用。
![currying](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017030903.jpg)
*这是一种Javascript函数式编程种最理想的工具库*
{% jsfiddle f2L52x0f js %}

让我们无聊一把，如果参数无数呢？
就是add()()()()()()…和add(1,2,3,4,5,6,7,8,….)
这样的问题，如果下次面试是这个怎么办？
很明显，第二个还是很好解决的，第一就是要用到循环调用函数的问题了

{% jsfiddle 5rnnfp7k js %}

函数中循环调用子函数，直到没有()，也就是函数的调用，返回最终x的值
