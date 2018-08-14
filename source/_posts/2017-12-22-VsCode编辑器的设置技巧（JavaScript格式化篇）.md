---
title: VsCode编辑器的设置技巧（JavaScript格式化篇）
date: 2017-12-22 12:36:26
categories:
- 技术
tags:
- VsCode
- javascript
---
# 前言

作为一个IT工作者，不管你是前端还是后端，甚至是大数据分析师，只要你接触代码，你总会要去选择一款方便的编辑器来配合你的工作。

我这里来列举下通常情况下各个编辑器会被哪类人使用

`C++/C` 入门者：[CodeBlocks](http://www.codeblocks.org/)、 DevCpp、 Vc6++
`Java` : ecilipse、 [IntelliJ IDEA](https://www.jetbrains.com/idea/)
`Python`: [PyCharm](https://www.jetbrains.com/pycharm/)
`HTML等前端`: [HBuilder](http://www.dcloud.io/)
`通用编辑器`: [Sublime](https://www.sublimetext.com/)、 [Atom](https://atom.io/)、 [vsCode](https://code.visualstudio.com/)

有一款好用的编辑器或者说是工具，就是一个IT工作者很好的利器，但是光有还不行，你至少要学会如何配置它，但是如果你还有额外的经理和能力，工具是开源的情况下，你就可以升级它。把他变成你所想要的模样。
<!--more-->
# 格式化的重要性

1. 格式化是一个程序员的基本素养，我看过那些换行都懒得换的，写出来的样子都没有左对齐，简直难以审阅
2. 有良好的格式化能力，不仅帮你审阅代码时不疲劳，甚至可以减少代码出错的概率

从而帮你平常的一些小痛苦中解脱出来

# 编辑器的选择

编辑器，我这里还是以我在工作上的角度去编写，以 `VsCode` 配置来说一下 `Format` 技巧

不过还是要说说它的优点
相比大多数大型 `IDE` 而言，它的功能单一但是速度很快
相比 `ATOM` 等自由度甚高的来说，它稳定性高而且依赖群体人数多

# VsCode配置举例解说

如果你是经常用 `javscript` 写项目的话，你会发现 `eslint` 的配置和 `VsCode` 的格式化有一些些冲突
特别当你复制下面这一段代码时
```js
export function saveWorkbook (xlsxName: string, wb: XLSX.WorkBook) {
  /* write workbook (use type 'binary') */
  const wbout = XLSX.write(wb, {bookType: 'xlsx', type: 'binary'})
  saveAs(new Blob([s2ab(wbout)], {type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'}), xlsxName)
}
```
这段代码在普通的 `eslint` 下是不会报错的，但是如果你将它复制一下，他就可能格式错乱了
```js
export function saveWorkbook(xlsxName: string, wb: XLSX.WorkBook) {
  /* write workbook (use type 'binary') */
  const wbout = XLSX.write(wb, { bookType: 'xlsx', type: 'binary' })
  saveAs(new Blob([s2ab(wbout)], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'}), xlsxName)
}
```
当然不同的人，有不同的配置，这里不岔开去讲了

这里有亮点不同，我分别用中文与英文，还有我自己的理解去说

`saveWorkbook(xls`:`saveWorkbook (xls`
这个区别官方叫做 `在函数参数括号前定义空格处理`
英文写法是 `insertSpaceBeforeFunctionParenthesis`
其实英文的写法更好理解一些，叫做在方法名后参数列表前插入空格

`XLSX.write(wb, {bookType: 'xlsx', type: 'binary'})`
定义非空括号的左括号后面和右括号前面的空格处理
英文简写 `insertSpaceAfterOpeningAndBeforeClosingNonemptyBraces`
以我的定义就是前大括号之后的空格与后大括号的空格加不加的问题

其它还有很多，我就不累述了，因为 `Vscode` 在翻译上做的挺不错的

