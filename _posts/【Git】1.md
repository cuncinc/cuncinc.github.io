---
date: 2019-04-04 18:14
status: public
tag: 技术
title: 【Git】Git之始
---
![](./_image/git-1.png)

Git的教材介绍已经讲得很清楚[【传送门】](http://git.github.io/htmldocs/gittutorial.html)，直接去看就已经够用，写这个也是为了梳理自己所学。

前期的安装我就不说了，在Windows下需要下载Git，而在Linux下可以直接用命令行来安装。安装好后，在power shell下输入git，如果出现Git的使用信息，则说明Git已经安装成功。
##使用：
在使用之前，先设置自己的姓名和邮箱，命令如下：
```git
git config --global user.name "要设置的名字"
git config --global user.email "要设置的邮箱"
```

然后，用cd命令把工作区转到目标目录，再初始化git：
```git
git init
```
此时，如果用文件管理器打开工作目录，会发现多了一个.git文件夹，
![](~/git-1.png)