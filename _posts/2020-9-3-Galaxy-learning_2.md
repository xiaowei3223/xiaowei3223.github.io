---
layout: post
title: "Galaxy Learning 2-- Install Galaxy on centos 7 system"
date: 2020-9-3
description: "Galaxy Learning"
tags: Galaxy_learning
--- 
> Python version default as python 3.7 in Conda environment, so first thing is install [conda](https://docs.conda.io/en/latest/).


## 1. Install Conda
```
mkdir Galaxy
cd Galaxy
curl -O https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh
bash Anaconda3-2020.02-Linux-x86_64.sh
```

<img src="/images/posts/galaxy_learning/1.png" height="auto" width="80%">
<img src="/images/posts/galaxy_learning/2.png" height="auto" width="80%">
<img src="/images/posts/galaxy_learning/3.png" height="auto" width="80%">

## 2. Activate conda base environment
```
cd /home/admin/anaconda3/bin
. ./activate
```
<img src="/images/posts/galaxy_learning/4.png" height="auto" width="80%">

<img src="/images/posts/galaxy_learning/5.png" height="auto" width="80%">

## 3. Install git
```
cd /home/admin/Galaxy
sudo yum install git
```
<img src="/images/posts/galaxy_learning/6.png" height="auto" width="80%">

## 4. Download galaxy with git
```
git clone -b release_20.05 https://github.com/galaxyproject/galaxy.git
```

<img src="/images/posts/galaxy_learning/7.png" height="auto" width="80%">

If it slowly download galaxy with git, download [galaxy tar.gz]((https://github.com/galaxyproject/galaxy/archive/master.tar.gz)
) package is an option.

```
wget https://github.com/galaxyproject/galaxy/archive/master.tar.gz
tar -zxvf master.tar.gz
```

## 5. Test default python version
```
python --version
```
<img src="/images/posts/galaxy_learning/8.png" height="auto" width="80%">


## 6. Start Galaxy
```
ls
cd galaxy/
ls
sh run.sh
```

<img src="/images/posts/galaxy_learning/9.png" height="auto" width="80%">
<img src="/images/posts/galaxy_learning/10.png" height="auto" width="80%">

## 7. Test Galaxy: open `http://localhost:8080` with firefox browser.
<img src="/images/posts/galaxy_learning/11.png" height="auto" width="80%">

