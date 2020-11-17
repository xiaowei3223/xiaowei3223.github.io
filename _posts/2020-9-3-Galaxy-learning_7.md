---
layout: post
title: "Galaxy Learning 7 -- Install R and Rstudio "
date: 2020-9-3
description: "Galaxy Learning"
tags: Galaxy_learning
--- 
### Step1: Preparation before installation
```
yum -y install gcc glibc-headers gcc-c++ gcc-gfortran readline-devel libXt-devel bzip2-devel epel-release
```
<img src="/images/posts/galaxy_learning/install_R/1.png" height="auto" width="80%">

### Step2: Install R
```
yum install yum -y install R
```
<img src="/images/posts/galaxy_learning/install_R/2.png" height="auto" width="80%">

### Step3: Check R version
```
R --version
```
<img src="/images/posts/galaxy_learning/install_R/3.png" height="auto" width="80%">

### Step4: Download and install rstudio server
```
wget https://download2.rstudio.org/server/centos6/x86_64/rstudio-server-rhel-1.3.1073-x86_64.rpm
sudo yum install rstudio-server-rhel-1.3.1073-x86_64.rpm
```

### Step5: Open the `8787` port 
```
sudo firewall-cmd --zone=public --permanent --add-port=8787/tcp
sudo firewall-cmd --reload #一定不要忘记这句话
sudo firewall-cmd --list-ports # 查看端口是否打开成功
```
<img src="/images/posts/galaxy_learning/install_R/4.png" height="auto" width="80%">
### Step6: Check the centos7 host ip: (IP is `192.168.175.128`)
```
ifconfig
```
<img src="/images/posts/galaxy_learning/install_R/5.png" height="auto" width="80%">

### Step7: Browser at [http://192.168.175.128:8787](http://192.168.175.128:8787):
Username belong to those users on centos7 system.
<img src="/images/posts/galaxy_learning/install_R/6.png" height="auto" width="80%">
