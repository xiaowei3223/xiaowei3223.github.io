---
layout: post
title: "在window安装planemo"
date: 2020-7-14
description: "planemo"
tags: ['planemo']
---  
 起初我是没有想到planemo可以安装在window系统上的。只是抱着试一试的心态，竟然可以。
 可怜我当初还傻傻地想着在centos7安装，费了贼大劲，才知道自己的bug在哪。能在window使用planemo,可以放心一逼。（往后看）
 

 
1.**确认window已经安装了Conda**，其实我的电脑是安装了Anaconda,所以有各种环境都匹配了。
 
2.用管理员身份运行
 
<img src="/images/posts/2020_7_14_planemo_install_windows/1.png" height="auto" width="80%">  

3.输入以下代码安装planemo即可。

```
$ conda config --add channels bioconda
$ conda config --add channels conda-forge
$ conda install planemo

```

<img src="/images/posts/2020_7_14_planemo_install_windows/2.png" height="auto" width="80%">

**后记：**

可是后来发现不能很好地使用。
<img src="/images/posts/2020_7_14_planemo_install_windows/3.png" height="auto" width="80%">

参考：
[Planemo installation](https://planemo.readthedocs.io/en/latest/installation.html)