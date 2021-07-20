---
layout: post
title: "Galaxy安装"
date: 2020-7-13 
description: "galaxy"
tags: ['galaxy']
---  
**Galaxy安装:**

安装环境： 
- UNIX/Linux or Mac OSX
- Python 3.6 or 3.5
	
如何在centos安装python3.6 或python3.5 请看教程。


1.先创建一个文件夹

```
mkdir Galaxy
```

2.去到Galaxy

```
cd Galaxy
```

3.clone Galaxy from Git:(对于个人)

```
git clone -b release_20.01 https://github.com/galaxyproject/galaxy.git
```

4.进入到gakaxy repository

```
cd galaxy
```
	
5.启动Galaxy

```
sh run.sh
```

**这个代码第一次使用的时候，会等待很久的，因为需要下载和配置很多东西**

6.sh run.sh这个代码会启动Galaxy server 在http://localhost:8080
	
因为我们使用的是实验室服务器，所以，我们要将8080这个端口在防火墙打开，以便我们访问
	
```
sudo firewall-cmd --zone=public --permanent --add-port=8080/tcp
sudo firewall-cmd --reload #一定不要忘记这句话
sudo firewall-cmd --list-ports # 查看端口是否打开成功
```
	
7.如果需要暂停Galaxy，同时按住：`Ctrl` + `C`
	
8.为了能够设置端口号，将configure信息复制
```
cp config/galaxy.yml.sample config/galaxy.yml
```

9.修改galaxy.yml， 使galaxy可以在远程查看   

 输入代码： `vim config/galaxy.yml`     
 修改内容：  
	将`http: 127.0.0.0.1:8080`改成 `http: 0.0.0.0:8080`   
 保存退出  
	
（远程查看使用的地址是 `http://主机ip地址:8080`)
	
10.重启galaxy(注意： 需要先进入到主目录galaxy下，才可以使用)
```
sh run.sh
```

11.后台运行galaxy（一般运行Linux上的程序都是执行.sh文件）

```
nohup sh run.sh &
```

12.后台停止galaxy
```
>>> 查看8080这个端口号是哪个进程在使用： lsof -i:8080
>>> 然后使用kill命令终止： kill -9 进程号
```

参考：
	[get-galaxy](https://galaxyproject.org/admin/get-galaxy/)