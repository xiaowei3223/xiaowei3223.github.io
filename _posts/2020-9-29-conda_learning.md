---
layout: post
title: "conda 相关内容"
date: 2020-9-29
description: "conda 虚拟环境"
tags: 'conda'
--- 
Conda主页： [https://docs.conda.io/projects/conda/en/latest/index.html](https://docs.conda.io/projects/conda/en/latest/index.html)
### conda 介绍
- conda是在windows、macOS和Linux上运行的开源软件包管理系统和环境管理系统。
- conda可以快速安装、运行和更新软件包及其依赖项。
- conda可以轻松地在本地计算机上的环境中创建、保存、加载和切换。
- conda是为python程序创建的，可以打包和分发适用于任何语言的软件。

**conda是管理各种软件包安装的主要界面：**

- 查询和搜索Anaconda软件包索引和当前的Anaconda安装
- 创建新的conda环境
- 将软件包安装并更新到现有的conda环境中。



### Centos7安装Conda

Linux安装Conda官方教程： [https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html) 

1.下载Anaconda及安装： [https://www.anaconda.com/products/individual](https://www.anaconda.com/products/individual)    
```
curl -O https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh
bash Anaconda3-2020.02-Linux-x86_64.sh #因为下载的是Anaconda3-2020.02-Linux-x86_64.sh，所以直接bash这个文件
```

然后就按着它的提示继续安装    

2.激活conda    
安装完之后，要执行下面的命令才能使用conda     
找到安装目录：    
```
cd /root/anaconda3/bin/  #比如我这里是在/root/anaconda3/
chmod 777 activate 
. ./activate #启动conda
```

### 配置镜像
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda
conda config --set show_channel_urls yes
conda config --show
```


### 软件包管理
查看conda的版本 `conda --version`     
conda更新 `conda update conda`     
搜索package`conda search [软件包]`     
安装软件包 `conda install [软件包]`    
安装软件包时指定通道 （使用--channel）`conda install scipy --channel conda-forge`    
检查是否安装： `conda list` 

`conda install numpy=1.11`：即安装能模糊匹配到numpy版本为1.11    
`conda install numpy==1.11`：即精确安装numpy为1.11的版本     


### 管理环境
**虚拟环境**    
虚拟环境是一种工具，可通过为每个项目创建隔离的空间（其中包含每个项目的依赖项）来将它们分开，从而将它们分开。   
开始使用conda时，已经有一个名为base的默认环境，如果不想将程序放入基本环境，创建单独的环境以使程序彼此隔离。    

1.创建一个新环境并在其中安装软件包    
将命名环境snowflakes并安装软件包BioPython。    
`conda create --name snowflakes biopython`    
	
2.激活环境    
windows使用命令： `conda activate snowflakes` 或 `activate snowflakes`    
macOS和Linux使用： `conda activate snowflakes` 或 `source activate snowflakes`

3.要查看所有环境的列表    
`conda info --envs`

### 管理python
创建新环境时，conda将安装与下载和安装Anaconda时使用的Python版本相同的Python版本，如果要使用其他版本的Python，例如Python3.5，只许创建一个新环境并指定所需的Python版本      
1.创建一个包含Python3.5的名为sankes的新环境：   
`conda create --name snakes python=3.5`
	
2.激活新环境    
windows使用命令： `conda activate snowflakes` 或 `activate snowflakes`    
macOS和Linux使用： `conda activate snowflakes` 或 `source activate snowflakes`    
	
3.验证snakes环境是否已添加并处于活动状态：     
`conda info --envs`    
Conda在活动环境的名称后显示所有环境的列表，并带有星号（*）    
	
4.验证当前环境中使用哪个版本的Python    
`python --version`

5.停用snake环境并返回基本环境：    
`conda activate`


