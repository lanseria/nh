---
title: Git 与远程仓库(Remote)的那些事(坑)
date: 2017-10-13 09:44:19
categories:
 - 技术
tags:
 - git
 - ssh
 - hole
---

> 参考：（无）

# 前言

好久没有更新博客了，自上次9月25日更新了一些数据结构的笔记以后，去忙别的事情了，原因的话，应该都懂。工作啦，还是学习，买手机等等，总之还是在规划自己的学习路线，时刻更新下，毕竟计划赶不上变化。
突然写这篇文章的原因有很多，有惠阳学弟向我要一些文章博客，还要我的博客...我就突然想起来了（我他妈博客几年没更新了，在我脑中），还有就是最近项目都有用到 `Git` 这个版本管理工具，虽然之前我经常用，不过自己平时与工作的情景毕竟已经不太一样了，除了要知道最基本的 `git init` , `git add -A`, `git commit -m "your commit"`, `git push` 等等，最重要的是你的远程管理仓库是不一定只有一个，比如我们公司要用的是 [gitlab](https://gitlab.com) 这完全与 [github](https://github.com) 不是同一个远程库，你得学会怎么同时管理多个远程库，而且里边有许多坑（Hole），这些坑你得自己找，网上是找不到的，毕竟现在人工智能的范围未能达到如此地步（我会在下文介绍一些我遇到坑）。
 <!-- more -->
# 目录

- 怎么将本地库与远程库关联
- 使用ssh-key去验证自己的身份
- 多远程仓库的管理与配置
- 一个巨大的坑（详细介绍）

# 怎么将本地库与远程库关联

{% img /img/2017-10-13-1.png 图1 %}
其实 `图1` 已经给出明确的答案了，如果需要详细的网页可以[点击这里](https://github.com/Lanseria/e)
这张图中第二部分就是给出的是，如果你已经先在本地有 `Git` 仓库了，
当然注意，这个仓库一定是已经 `git init` 过的，或者说是 `git clone` 过的，不能你自己以为就是以为。
然后，执行这句命令
```
git remote add origin https://github.com:[yourusername]/[repo].git
```
`yourusername` 是你在github或者其它刚刚建立的远程仓库的使用者，也就是你自己的昵称
`repo` 是你建立的仓库名字，这步一定是最前的，不然你怎么跟本地库关联。

最后将本地库推送到远程库
执行这句 
```
git push -u origin master
```
意思就是 推送 到 `origin` (没错这就是刚刚指代的你的远程库代称) `master` 就是远程库的 `master` 分支

# 使用ssh-key去验证自己的身份

可能看完上一章的童鞋会问，ssh-key是干吗用的，不是只要知道 `push` , `pull`, `clone` 命令就可以了吗？
看样子好像，似的。但是又不是
你可以自己看看 `图1` 的截图，和刚才添加命令中的地址信息，你知道 `https://github.com` 是什么意思吗？
我们放大 `图1` 的细节
{% img /img/2017-10-13-2.png 图2 %}
有 `https` 和 `ssh` 选项，而且你点击哪个，他的地址就会发生改变
ssh: git@github.com:Lanseria/e.git
https: https://github.com/Lanseria/e.git
这两种加密方式都是对你代码在 `tcp` 的传输中，不会被他人截取，所以不要想为什么没有 `http` 等等问题啦
我说说这两者的区别吧
 `https` 就像你登陆网站是去输入账户和密码来验证你的身份合法
而 `ssh` 则采用流行于 `linux` 的 `ssh` 登陆的加密方式去管理你的账户
所以两者的便捷性不言而喻
最大的体验就是
https 每次重启计算机，你下次去 push , pull , clone 都需要你重新输入密码
而 ssh 就像你和远程的仓库， 比如 github 已经互相确认了对方的信息，只要双方的加密过的 key 正确，我就相信你

所以，入门者可以用用 https 但是， 如果你用到工作中， ssh 必然会方便很多

## 如何配置

放一个官方的地址
https://help.github.com/articles/connecting-to-github-with-ssh/
你们应该不会打我吧
不会百度嘛，很轻松的事
我来列举下期间会用到的命令
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
// 这里 -b 4096 可以不加，邮箱填写自己的注册时的帮你邮箱
Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]
// 这可能会弹出这个，意思是这个key会保存在这里，并命名为id_rsa 你可以修改他的名字，不过建议如果你不是多仓库的话还是不要改了
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
// 这两个是加密的密码，听起来很绕，但直接回车，设置为空就可以了，两下回车，第二个是comfirm的意思
```
最后就生成了
你用编辑器，打开当个目录（/c/Users/you/.ssh/）的中*.pub的文件，将里边的字符复制出来
放到github中ssh管理中，添加即可

# 多远程仓库的管理与配置

来了，这个的前序章节
让我假装新手，百度一下
就这篇文章了吧https://my.oschina.net/guanyue/blog/485918
它说（/c/Users/you/.ssh/）下添加config配置文件
```
Host gitlab.xxx.com ##可以随意命名，链接时使用这个名字    
HostName gitlab.xxx.com    
User git    
Port 22    
IdentityFile ~/.ssh/id_rsa_second 
```
没了

# 一个巨大的坑（详细介绍）

```
#github HostName 和Host一定要一样，不能随便取，为什么？自己找
Host github.com 
    HostName github.com
    IdentityFile ~/.ssh/id_rsa_home
    PreferredAuthentications publickey
    User youreamil@qq.com
```
