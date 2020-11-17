---
layout: post
title: "Galaxy Learning 3 -- Set galaxy administrator "
date: 2020-9-3
description: "Galaxy Learning"
tags: Galaxy_learning
--- 
## 1. Connect centos 7 with ssh in teminal windows on windows system.

### Step1: Find out the centos7 ip using `ifconfig` command. In this example, centos7 ip is `192.168.175.128`.
<img src="/images/posts/galaxy_learning/galaxy_administrator/1.png" height="auto" width="80%">

### Step2: Connect centos7 with `ssh` in local windows system.
Press key `Win` + `R`, and input `cmd` to open windows terminal.
<img src="/images/posts/galaxy_learning/galaxy_administrator/2.png" height="auto" width="80%">
 
### Step3: Input following command to connect centos 7 system: 
```
ssh admin@192.168.175.128
```
<img src="/images/posts/galaxy_learning/galaxy_administrator/3.png" height="auto" width="80%">

## 2. Setting remote access galaxy

### Step1: Go to galaxy root directory.
```
cd Galaxy/galaxy
ls
```
<img src="/images/posts/galaxy_learning/galaxy_administrator/4.png" height="auto" width="80%">

### Step2: Copy galaxy.yml configure file for modifying. 
```
ls config
cp config/galaxy.yml.sample config/galaxy.yml
ls config
```
<img src="/images/posts/galaxy_learning/galaxy_administrator/5.png" height="auto" width="80%">

### Step3: Make `http: 127.0.0.1:8080` as `http: 0.0.0.0:8080` in `config/galaxy.yml` file.
```
vim config/galaxy.yml
```
<img src="/images/posts/galaxy_learning/galaxy_administrator/7.png" height="auto" width="80%">
<img src="/images/posts/galaxy_learning/galaxy_administrator/8.png" height="auto" width="80%">

Save your modification.

### Step4: Open `8080` port:
```
sudo firewall-cmd --zone=public --permanent --add-port=8080/tcp
sudo firewall-cmd --reload #一定不要忘记这句话
sudo firewall-cmd --list-ports # 查看端口是否打开成功
```
<img src="/images/posts/galaxy_learning/galaxy_administrator/9.png" height="auto" width="80%">

### Step5: Restart galaxy.
```
sh run.sh
```
<img src="/images/posts/galaxy_learning/galaxy_administrator/10.png" height="auto" width="80%">

### Step6: Open the [http://192.168.175.128:8080/](http://192.168.175.128:8080/) with your local browser in windows system.
<img src="/images/posts/galaxy_learning/galaxy_administrator/11.png" height="auto" width="80%">

## 3. Create a galaxy user.
### Step1: Click `Login or Register` and `Register here`.
<img src="/images/posts/galaxy_learning/galaxy_administrator/12.png" height="auto" width="80%">
### Step2: Input your email address, password and public name in galaxy.
<img src="/images/posts/galaxy_learning/galaxy_administrator/13.png" height="auto" width="80%">
### Now you are a galaxy user. This example is `h150162111223@163.com` user.
<img src="/images/posts/galaxy_learning/galaxy_administrator/14.png" height="auto" width="80%">

## 4. Become an Administrator

### Step1: Press `Ctrl` + `C` at the same time to stop galaxy.

### Step2: Add `admin_users: h15016211223@163.com` in `config/galaxy/yml`.
```
vim config/galaxy.yml
```

<img src="/images/posts/galaxy_learning/galaxy_administrator/6.png" height="auto" width="80%">

Save your modification.

### Step3: Restart galaxy with `sh run.sh` and Open the [http://192.168.175.128:8080/](http://192.168.175.128:8080/) with your local browser in windows system.
<img src="/images/posts/galaxy_learning/galaxy_administrator/15.png" height="auto" width="80%">

Now, you can see the admin interface.

<img src="/images/posts/galaxy_learning/galaxy_administrator/16.png" height="auto" width="80%">

