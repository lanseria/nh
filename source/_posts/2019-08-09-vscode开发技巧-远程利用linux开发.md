---
title: vscode开发技巧-远程利用linux开发
date: 2019-08-09 19:15:42
categories:
- 技术
- 工具
tags:
- Vscode
- remote
- Linux
---
# 前言

> 此篇文章需要你掌握有关 Git | Vscode | Bash 的基础知识。阅读时长约为5分钟。

作为程序开发者，平时开发我想大多数都是在 `Windows` 上开发的，如果平时也不会接触计算机中高级的成分，我想你也不需要用到 `Linux` 等一些系统，其他编译问题可能会遇到，但百度一下都能够解决掉。所以这篇文章自认为是给那些需要装逼，或者需要更不易出错，需要更快的编译时间等那些人看的，在我看来， `Linux` 上编译的速度远远比 `Windows` 高得多，所以我开始将开发工作搬运到了 `Linux` 机器中，一来，我的电脑不会太卡，可以继续流畅地浏览网页。二来，开发速度得到了快速的提升。如果你对此有兴趣的话，我们就马上开始准备吧（这里只讲 `vscode` 编辑器的远程开发配置）

<!--more-->

# 准备

{% img /img/2019-08-09-1.png Vscode编辑器 %}

- Vscode 编辑器(我用的是1.37版本)
- Git windows 安装包 (我的用是git version 2.22.0.windows.1)
- 以及一些Vscode关键词用来搜索(remote是搜索关键字)

{% img /img/2019-08-09-2.png remote是搜索关键字 %}

红框是必须要安装的哦，其他也推荐安装，万一没成功我可不知道😂。

# 配置

安装完这些插件之后，这时你可以看到你的左侧会多一个PC的图标

{% img /img/2019-08-09-3.png PC的图标 %}

点开之后会出现类似截图一样的链接(不过这些链接一般是不可能链接成功的)

{% img /img/2019-08-09-4.png 链接 %}

你可能会问，为什么我没有你出现的这四个链接，因为我git的ssh目录做了配置，这也是我为什么发现这样的可行性。如何打开你的ssh配置？
只要右击打开git-bash命令行输入 code ~/.ssh/ 即可

{% img /img/2019-08-09-5.png git-bash %}

{% img /img/2019-08-09-6.png 配置过的config文件 %}

如果是我的话会打开这样之前已经被我配置过的 `config` 文件，如果你是空白的话，你需要自行创建一个config文件，如何创建我在之前的文章已经提过，你可以直接翻看的文章进行理解与配置


[相关SSH配置链接](/2018/03/07/ssh%E5%AF%86%E9%92%A5%EF%BC%8C%E4%B8%80%E4%B8%AA%E9%92%A5%E5%8C%99%E8%B5%B0%E5%A4%A9%E4%B8%8B/)

然后开始填写你Linux ssh配置, 这些配置信息如何来，看上面链接上的文章即可，
你需要知道你的服务器的IP地址, sshkey 域名(没有可以利用hosts配置)

```conf
#txsshcq
Host ivhik.cn # 域名(绑定你的服务器IP)
  Port 22 #端口
  HostName ivhik.cn 
  StrictHostKeyChecking no
  IdentityFile ~/.ssh/id_rsa_ssh # 你的私钥
  User root #你的登陆用户名称
```

配置之后，还需要移除 `Windows` 自带的 `ssh`  
需要管理员运行
```ps1
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```
然后将 `git` 的 `/bin` 目录添加在 `windows path` 环境变量中

接下来你就可以远程编辑你服务器里的代码了
