---
layout: post
title: "nginx安装"
date: 2020-8-3
description: "nginx"
tags: "nginx"
---
参考： [https://www.cnblogs.com/lich1x/p/11225528.html](https://www.cnblogs.com/lich1x/p/11225528.html)

(（建议下述安装过程全程root）)
1. 安装PCRE库
```
cd /usr/local/
curl -O ftp://ftp.pcre.org/pub/pcre/pcre2-10.35.tar.gz
tar -zxvf pcre2-10.35.tar.gz
cd pcre2-10.35/
./configure
make && make install
```

2. 安装openssl、zlib、gcc依赖
```
yum -y install make zlib zlib-devel gcc-c++ libtool openssl openssl-devel
```

3. 安装nginx
```
cd /usr/local/
wget http://nginx.org/download/nginx-1.8.0.tar.gz
tar -zxvf nginx-1.8.0.tar.gz
cd nginx-1.8.0 
./configure
make && make install
```

4. Nginx的常用命令      
	进入nginx目录    
	`cd /usr/local/nginx/sbin`   

	(1)查看nginx版本号   
	`./nginx -v`    
	(2)启动nginx    
	`./nginx`   
	(3)停止nginx   
	`./nginx -s stop`   
	(4)重新加载nginx   
	`./nginx -s reload`    

5. 端口号开放   
	(1)查看开放的端口号   
	`firewall-cmd --list-all`      
	(2)设置开放的端口号    
	`firewall-cmd --add-service=http --permanent`    
	`firewall-cmd --add-port=80/tcp --permanent`    
	(3)重启防火墙     
	`firewall-cmd --reload`   
