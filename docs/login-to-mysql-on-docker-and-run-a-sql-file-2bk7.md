# 🐳在 Docker 上登录 MySQL 并运行 SQL 文件

> 原文：<https://dev.to/n350071/login-to-mysql-on-docker-and-run-a-sql-file-2bk7>

## 🤔情况

你用 MySQL 维护一个 app，在 Docker 上开发。您有一个. sql 格式的转储文件，然后您想运行它。

## 👌程序

### 1。将 SQL 文件复制到 Docker 容器中

```
$ docker cp hoge.sql [cotaier-id]:/hoge.sql 
```

### 2。登录到坞站

```
$ docker ps
$ docker exec -it [container-id] /bin/bash 
```

### 3。登录 MySQL

```
$ mysql -u root -p
$ show databases;
$ use [your_database_name];
$ show tables; 
```

💡提示:如果你使用 rails 应用，密码和连接信息应该在`config/database.yml`定义。或许，其实是写在`.env`文件里的。

### 4。运行 SQL 文件

```
$ source hoge.sql 
```

* * *

## 母注

[![n350071 image](img/c9dd0fa7ece3b4a6c3e55d373898cd1e.png)](/n350071) [## 我的码头笔记

### n350071🇯🇵10 月 29 日 191 分钟阅读

#docker](/n350071/my-docker-note-3d0m)