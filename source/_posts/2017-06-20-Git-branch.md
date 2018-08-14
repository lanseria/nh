---
title: Git中的平行时空-分支Branch
date: 2017-06-20 17:03:20
categories:
 - 技术
tags:
 - git
---
{% cq %}
本文大多数引用廖老师的教程
--@git教程
http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000
{% endcq %}

# 分支管理

分支这个概念是很神奇的，你不仅可以用来开发，对我来说还可以用特别的软件观察出你的这个项目到底有多么宏伟（虽然现在只是空谈。。）
 <!-- more -->
分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！

{% img http://www.liaoxuefeng.com/files/attachments/001384908633976bb65b57548e64bf9be7253aebebd49af000/0 漫画 %}

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

其他版本控制系统如SVN等都有分支管理，但是用过之后你会发现，这些版本控制系统创建和切换分支比蜗牛还慢，简直让人无法忍受，结果分支功能成了摆设，大家都不去用。

但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。

# 创建与合并分支

在重拾篇中，我们已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。

一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：

{% img http://www.liaoxuefeng.com/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0 HEAD与master的关系 %}

每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长，当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：
{% img http://www.liaoxuefeng.com/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0 dev新分支 %}

你看，Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：
{% img http://www.liaoxuefeng.com/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0 HEAD指向dev %}

假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：
{% img http://www.liaoxuefeng.com/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0 master追到dev %}

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：
{% img http://www.liaoxuefeng.com/files/attachments/001384908867187c83ca970bf0f46efa19badad99c40235000/0 删除了dev分支 %}

# 解决冲突

人生不如意之事十之八九，合并分支往往也不是一帆风顺的。

准备新的feature1分支，继续我们的新分支开发：
{% blockquote %}
$ git checkout -b feature1
Switched to a new branch 'feature1'
{% endblockquote %}
修改readme.txt最后一行，改为：
{% blockquote %}
Creating a new branch is quick AND simple.
{% endblockquote %}
在feature1分支上提交：
{% blockquote %}
$ git add readme.txt 
$ git commit -m "AND simple"
[feature1 75a857c] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)
 {% endblockquote %}
 切换到master分支：
 {% blockquote %}
 $ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
{% endblockquote %}
Git还会自动提示我们当前master分支比远程的master分支要超前1个提交。

在master分支上把readme.txt文件的最后一行改为：
{% blockquote %}
Creating a new branch is quick & simple.
{% endblockquote %}
提交：
{% blockquote %}
$ git add readme.txt 
$ git commit -m "& simple"
[master 400b400] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
 {% endblockquote %}
 现在，master分支和feature1分支各自都分别有新的提交，变成了这样：
 {% img http://www.liaoxuefeng.com/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0 feature1与master %}
 这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：
 {% blockquote %}
 $ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
{% endblockquote %}
果然冲突了！Git告诉我们，readme.txt文件存在冲突，必须手动解决冲突后再提交。git status也可以告诉我们冲突的文件：
```
$ git status
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#
# Unmerged paths:
#   (use "git add/rm <file>..." as appropriate to mark resolution)
#
#       both modified:      readme.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```
我们可以直接查看readme.txt的内容：
```
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes of files.
    <<<<<<< HEAD
    Creating a new branch is quick & simple.
    =======
    Creating a new branch is quick AND simple.
    >>>>>>> feature1
```
Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们修改如下后保存：
{% blockquote %}
Creating a new branch is quick and simple.
{% endblockquote %}
再提交：
{% blockquote %}
$ git add readme.txt 
$ git commit -m "conflict fixed"
[master 59bc1cb] conflict fixed
{% endblockquote %}
现在，master分支和feature1分支变成了下图所示：
{% img http://www.liaoxuefeng.com/files/attachments/00138490913052149c4b2cd9702422aa387ac024943921b000/0 处理完问题后 %}
用带参数的git log也可以看到分支的合并情况：
```
  *   2671404 conflict fixed
  |\
  | * 791d353 AND simple
  * | 68c7116 & simple
  |/
  * 2605498 branch test
  * 8dc924a add test.txt
  * 17f0247 add of files
  * 381cacb git tracks changes
  * 7e2984e learn git stage is important
  * 4db1f01 append GPL
  * 7c17cad add distributed
  * bb08438 wrote a readme file
```
最后，删除feature1分支：
{% blockquote %}
$ git branch -d feature1
Deleted branch feature1 (was 75a857c).
{% endblockquote %}