---
layout: post
title: "planemo 安装"
date: 2020-7-14
description: "planemo"
tags: ['planemo']
---  
我的配置环境是：
- centos 7
- python 2.7
- python 3.5

使用python时，直接打开的时python3.5.

对于利用python安装Planemo，第一步首先设置virtualenv(虚拟环境）
一开始我也没有意识到这要求的python一定得是python3以上的来实现虚拟环境，
傻傻地使用[教程](https://planemo.readthedocs.io/en/latest/readme.html)来安装Planemo， 
然后是一路艰辛、一路bug。折腾了一整天才勉强安装上planemo。
结果后续使用planemo提示需要使用的是python3.才意识到即使在虚拟环境中还是被默认为python2.7的版本。

所以这个坑踩的有点深。

以下便是安装planemo的过程：
1. 创建虚拟环境：
```bash
python3 -m venv venv_dir  # 创建虚拟环境venv_dir， 会自动生成venv_dir文件夹
```
<img src="/images/posts/2020_7_14_planemo_install/virtural_environment_1.png" height="auto" width="80%">  

2. 激活环境变量 `source ./bin/activate` 这个命令也是再次进入虚拟环境的命令。
```bash
cd venv_dir/
source ./bin/activate  #激活环境后所有的操作都在该虚拟环境中进行，不会到全局的python环境和其它python虚拟环境。 
```
3. pip升级
```
pip install "pip>=7"
pip install --upgrade pip
```
<img src="/images/posts/2020_7_14_planemo_install/virtural_environment_2.png" height="auto" width="80%">  
4. 安装planemo
```
pip install planemo #如果这个代码出错的话，试试： pip install -U git+git://github.com/galaxyproject/planemo.git
					#如果还是不行，试试输入这个看看： pip install --upgrade setuptools 
```

参考： 
1. [python虚拟环境设置](https://www.cnblogs.com/yourblog/p/12676906.html)
2. [planemo Quick Start](https://planemo.readthedocs.io/en/latest/readme.html)