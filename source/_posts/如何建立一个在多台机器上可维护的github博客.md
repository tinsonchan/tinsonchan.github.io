---
title: 如何建立一个在多台机器上可维护的github博客
comments: false
categories:
  - 技术
tags:
  - github
date: 2021-06-14 20:38:37
---

前几天跟着网上的教程，已经把博客搭建起来了。但是考虑到自己有一台主机和一台笔记本，如果能做到随时随地的写博客，而不用一定要回到家里才能过写，那才是真正的方便。而且，这样有利于把整个博客工程备份起来，不然如果哪一天出问题了，那就后悔都来不及了，因为你已经找不回自己的文件了。

当然百度是无所不能的，能够解决你在技术上的一切问题。而记录此篇文章，纯粹是为了以后可以自己翻查，重新再次部署时可以快速切入，而不用再一点点的边度了。
<!--more-->

本文主要为分为几部分，一一描述。

## 单机博客安装 ##
### 环境准备 ###

1.Node.js

    https://nodejs.org/en/
	一般选择LTS版本

2.git
	
    https://git-scm.com/
	这是git官网，windows需要自己去下载，而mac则是自带的

### hexo安装 ###

1.安装hexo
	
	这是windows的命令行
	npm install -g hexo

	mac则使用下面的命令行
	sudo npm install -g hexo

2.初始化hexo

    以下这个命令行可以初始化整个hexo的工程
	hexo init
	
	这个是启动网页服务器，方便你在本地查看的效果
	hexo s
	
3.上传到github

    先要安装上传的插件
	npm install hexo-deployer-git --save

	然后调用以下命令行就可以了
	hexo d -g

## github备份 ##
接下来这部分就是把整个工程在github上备份，然后就可以在其他机器同步，这样每次想写博客的时候，都去pull一份最新的工程，然后直接在上面修改就可以了。

### 创建分支 ###
1.checkout 你的github工程

    git clone git@github.com:tinsonchan/tinsonchan.github.io.git

2.创建分支

    git branch hexo

3.切换分支

    git checkout hexo

4.提交到github

    git add .
	git commit -m 'back up hexo files'
	git push --set-upstream origin hexo

5.修改github的默认分支
因为默认是master分支，但是我们的博客目录是在master，而我们是把工程放在branch hexo的，所以需要到GitHub上面修改一下

	主要是到下面这个地址去修改
	https://github.com/tinsonchan/tinsonchan.github.io/settings/branches

### 编写新的日志 ###
1. git pull -更新日志目录
2. hexo new "内容标题"
3. 编辑 博客内容
4. hexo d -g -生成博客网页内容
5. git add . -添加版本管理
6. git commit -m "提交版本内容"
7. git push -更新到远程库

### 修改 2018-08-16###
在博客中增加图片

1. 直接把图片放在source/images下面

	(/images/image.jpg)

2. 修改配置

	//_config.yml

	post_asset_folder: true

	//图片路径

	image.jpg


### 结束 ###
完成以上步骤之后，我们就把整个工程备份到网络上了，然后再其他机器上只需要完成单机博客安装就可以了。

----------
追求卓越 成功就会在不经意间追上你

