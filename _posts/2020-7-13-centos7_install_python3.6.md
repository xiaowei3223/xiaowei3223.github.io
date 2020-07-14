---
layout: post
title: "卸载centos自带的python2.7 | 安装python3.6.4"
date: 2020-7-13 
description: "python"
tags: ['python']
---  

# 卸载centos自带的python2.7
（其实多少有些不建议的，比如说yum这个安装软件的工具，默认使用的就是python2.7版本的。把python2.7卸载后，yum就无法使用了。）

使用以下的代码卸载centos7中自带的python2.7：

```
rpm -qa|grep python|xargs rpm -ev --allmatches --nodeps  #卸载自动Python
whereis python |xargs rm -frv  #删除残余文件
whereis python #验证删除
```

# 安装python3.6.4

Step1: 在CentOS上按如下命令下载：

```
wget https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz
```

Step2: 下载完后，进行解压

```
tar -zxvf Python-3.6.4.tgz
```

Step3: 进入解压后的Python-3.6.4的文件夹，编译和安装Python 3.6.4

```
cd Python-3.6.4
./configure && make &&make install
```

Step4: 安装完成之后，输入python3看看是否安装成功

Step5: 最后，执行python -V验证，如图版本变成了3.x:(将python3设置为python)
```
ln -s /usr/local/bin/python3 /usr/bin/python
```

