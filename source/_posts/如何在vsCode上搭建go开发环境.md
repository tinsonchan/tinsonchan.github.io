---
title: 如何在vsCode上搭建go开发环境
comments: false
categories:
  - 技术
date: 2018-08-16 07:36:21
tags:
  - golang
---

最近在学习golang，但是如果有一个比较友好的ide，就可以提高不少效率，以前一直在用sublime的我，现在决定用vscode了，接下里的步骤，可以部署一个在vscode下直接运行的go ide。
<!--more-->

### 安装golang ###

	https://studygolang.com/dl/golang/go1.10.3.windows-amd64.msi
我用的是win10，直接下载安装就好了，系统会自动增加环境变量

另外还要增加一个工作的目录
![](1.png)
如果不设置，默认会在当前用户目录下，有一个go文件夹
如果当前用户设置环境变量，会把你设置的覆盖
	

### vsCode ###
1. 下载安装vs code
2. 安装插件“go”

### 安装go需要的插件 ###
	go get -u -v github.com/nsf/gocode
	go get -u -v github.com/rogpeppe/godef
	go get -u -v github.com/golang/lint/golint
	go get -u -v github.com/lukehoban/go-find-references
	go get -u -v github.com/lukehoban/go-outline
	go get -u -v sourcegraph.com/sqs/goreturns
	go get -u -v golang.org/x/tools/cmd/gorename
	go get -u -v github.com/tpng/gopkgs
	go get -u -v github.com/newhook/go-symbols
最后编译出来的文件如图所示
![](2.png)



----------
追求卓越 成功就会在不经意间追上你