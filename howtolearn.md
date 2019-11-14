# Mysql/mariadb資料庫  
資料庫管理系統(Database Management System,DBMS)是一種管理資料庫的大型軟體，是用於建立、使用和維護資料庫。  
具有代表性的資料管理系統有：Oracle、Microsoft SQL Server、Access、MySQL及PostgreSQL等，這邊使用的是MySQL做介紹。  
說說為什麼會選擇MySQL:  
* 體積小  
* 安裝速度快  
* 成本低  
* 開源(免錢的意思啦)  
* 支援多種作業系統。  

## 安裝環境  
作業系統:centos7  
MySQL版本:5.5.64  

## 安裝步驟  
1. 安裝前先進行全系統更新  
```
# yum -y update
```  
2. 接著安裝MySQL  
```
# yum install mysql
```
3. 利用mysql安全安裝設定  
```
# mysql_secure_installation
```
4. 最後就可以進入資料庫了
```bash
[root@oracle-kai ~]# mysql -u root -p
Enter password:
```  

## 建立資料庫  
先介紹如何建立資料庫:  
CREATE {DATABASE | SCHEMA} IF NOT EXISTS db_name;
```bash
MariaDB [(none)]> create database dic_order;
Query OK, 1 row affected (0.00 sec)
```

使用剛剛建立的資料庫:
use [dbname];
```
MariaDB [(none)]> use dic_order;
Database changed
MariaDB [dic_order]>
```

查看建立的資料表:
show databases;
```
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| dbname             |
| dic_order          |
| kaidb              |
| mysql              |
| performance_schema |
+--------------------+
6 rows in set (0.00 sec)
```
有必要的話可以使用刪除:  
```bash
MariaDB [(none)]> drop database order1;
Query OK, 0 rows affected (0.00 sec)
```  

接著是使用者的部分，分別介紹三種功能。  
新增:  
CREATE USER 'Username'@'localhost' IDENTIFIED BY 'NewPasswd';
```bash
MariaDB [(none)]> CREATE USER 'kai'@'localhost' IDENTIFIED BY 'ispassword';
Query OK, 0 rows affected (0.00 sec)
```
查看:  
select Host,User from mysql.user;
```
MariaDB [(none)]> select Host,User from mysql.user;
+-----------+------+
| Host      | User |
+-----------+------+
| 127.0.0.1 | root |
| ::1       | root |
| localhost | kai  |
| localhost | root |
+-----------+------+
4 rows in set (0.00 sec)
```  
刪除:  
delete from mysql.user where User='[USERNAME]';
```bash
MariaDB [(none)]> delete from mysql.user where User='kai@localhost';
Query OK, 1 row affected (0.00 sec)
``` 

## 建立資料表  
資料庫完成後就是資料表，這是最頻繁使用的地方了，這邊分成建立資料表、新增資料、刪除資料、修改資料。  
建立資料表:  
Create table 表格名 (
欄位名1 資料型態,  
欄位名2 資料型態,  
欄位名3 資料型態 
)  
```
Create table test1 (
t1 int,  
t2 char(10),  
t3 varcahr(10)  
)  
```  
查看資料表:  
show tables;  
```
```  
新增資料:  
insert into [tablename] values('資料1','資料2'...);
```
```  
查看資料:  
select * from [tablename];
```
```  
刪除資料:  
delete  
```
```

修改資料:  
alter   
```
```  

有必要的話也可以用刪除:  
drop table [tablename];  
```
```  

## 談談select的用法  
