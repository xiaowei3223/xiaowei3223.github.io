---
layout: post
title: "Galaxy Learning 5-- PostgreSQL database connect Galaxy "
date: 2020-9-3
description: "Galaxy Learning"
tags: Galaxy_learning
--- 

## 1. Install PostgreSQL
```
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo yum install -y postgresql12-server
sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
sudo systemctl enable postgresql-12
sudo systemctl start postgresql-12
```

<img src="/images/posts/galaxy_learning/postgreSQL_galaxy/1.png" height="auto" width="80%">
<img src="/images/posts/galaxy_learning/postgreSQL_galaxy/2.png" height="auto" width="80%">
<img src="/images/posts/galaxy_learning/postgreSQL_galaxy/3.png" height="auto" width="80%">

## 2. Set up the PostgreSQL postgres user password:
```
sudo -u postgres psql postgres
\password postgres
```
and give the password `postgres` when prompted.  
<img src="/images/posts/galaxy_learning/postgreSQL_galaxy/4.png" height="auto" width="80%">

## 3. Create database named "galaxy" and make 'postgres' the owner. 
```
sudo -u postgres psql postgres
\l
CREATE DATABASE galaxy owner postgres;
\l
\q
```
<img src="/images/posts/galaxy_learning/postgreSQL_galaxy/7.png" height="auto" width="80%">

## 4. Edit the `postgresql.conf` file to allow connection from all computers:
```
sudo vim /var/lib/pgsql/12/data/postgresql.conf
```

<img src="/images/posts/galaxy_learning/postgreSQL_galaxy/8.png" height="auto" width="80%">

## 5. Edit the `pg_hba.conf` file to allow everyone access postgreSQL:
```
sudo vim /var/lib/pgsql/12/data/pg_hba.conf
```
<img src="/images/posts/galaxy_learning/postgreSQL_galaxy/9.png" height="auto" width="80%">

## 6. Restart postgreSQL 12
```
sudo systemctl restart postgresql-12
```

## 7. Edit `config/galaxy.yml` file for setting database connection:
```
cd galaxy
vim config/galaxy.yml
```

Add following command or edit as following command in `config/galaxy.yml` file:
```
database_connection: postgres://postgres:postgres@localhost:5432/galaxy  
database_engine_option_server_side_cursors: true  
database_engine_option_pool_size: 10 
database_engine_option_max_overflow: 20  
slow_query_log_threshold: 2 
```



<img src="/images/posts/galaxy_learning/postgreSQL_galaxy/10.png" height="auto" width="80%">
Save your modification.

## 8. Restart galaxy
```
sh run.sh
```