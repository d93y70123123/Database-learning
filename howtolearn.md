# sql語法教學  
資料庫管理系統(Database Management System,DBMS)是一種管理資料庫的大型軟體，是用於建立、使用和維護資料庫。  
具有代表性的資料管理系統有：Oracle、Microsoft SQL Server、Access、MySQL及PostgreSQL等，這邊使用的是(MySQL/MariaDB)做介紹。  
```diff 
- 除了sql語法外，如：show databases 、 descripe [tablesname]......非針對資料表欄位操作的指令，在各個資料庫系統都不一定一樣
```

## 安裝環境  
作業系統:centos7  
(MySQL/MariaDB)版本:10.4.8

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
create table test1 (
t1 int,  
t2 char(10),  
t3 varchar(10)  
);  
```  
查看資料表:  
show tables;  
```mysql
MariaDB [dic_order]> show tables;
+---------------------+
| Tables_in_dic_order |
+---------------------+
| store_name          |
| test1               |
+---------------------+
2 rows in set (0.000 sec)
```  
新增資料:  
insert into [tablename] values('資料1','資料2'...);
```mysql
MariaDB [dic_order]> insert into test1 values(1,'apple','pieapple');
Query OK, 1 row affected (0.030 sec)
```  
列出資料:  
select * from [tablename];
```
MariaDB [dic_order]> select * from test1;
+------+-------+----------+
| t1   | t2    | t3       |
+------+-------+----------+
|    1 | apple | pieapple |
+------+-------+----------+
1 row in set (0.001 sec)
```  
刪除資料:  
delete  from [tablename] where [欄位]=[值];
```mysql
MariaDB [dic_order]> select * from test1;
+------+--------+----------+
| t1   | t2     | t3       |
+------+--------+----------+
|    1 | apple  | pieapple |
|    2 | banana | babanana |
+------+--------+----------+
2 rows in set (0.002 sec)

刪除第一筆資料

MariaDB [dic_order]> delete from test1 where t1 = 1;
Query OK, 1 row affected (0.080 sec)

重新列出資料
MariaDB [dic_order]> select * from test1;
+------+--------+----------+
| t1   | t2     | t3       |
+------+--------+----------+
|    2 | banana | babanana |
+------+--------+----------+
1 row in set (0.001 sec)
```

修改資料:  
alter   
```mysql
加入欄位
MariaDB [dic_order]> alter table test1 add t4 int;
Query OK, 0 rows affected (0.067 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [dic_order]> select * from test1;
+------+--------+----------+------+
| t1   | t2     | t3       | t4   |
+------+--------+----------+------+
|    2 | banana | babanana | NULL |
+------+--------+----------+------+
1 row in set (0.002 sec)

改變欄位名稱
MariaDB [dic_order]> alter table test1 change t4 tt44 int;
Query OK, 0 rows affected (0.063 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [dic_order]> select * from test1;
+------+--------+----------+------+
| t1   | t2     | t3       | tt44 |
+------+--------+----------+------+
|    2 | banana | babanana | NULL |
+------+--------+----------+------+
1 row in set (0.001 sec)
*注意更改欄位名稱，同時也要更改資料型態

修改欄位的資料型態
MariaDB [dic_order]> describe test1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| t1    | int(11)     | YES  |     | NULL    |       |
| t2    | char(10)    | YES  |     | NULL    |       |
| t3    | varchar(10) | YES  |     | NULL    |       |
| tt44  | int(11)     | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.002 sec)
這個時候tt44還是int。

MariaDB [dic_order]> alter table test1 modify tt44 char(50);
Query OK, 1 row affected (0.315 sec)
Records: 1  Duplicates: 0  Warnings: 0

修改資料型態後
MariaDB [dic_order]> describe test1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| t1    | int(11)     | YES  |     | NULL    |       |
| t2    | char(10)    | YES  |     | NULL    |       |
| t3    | varchar(10) | YES  |     | NULL    |       |
| tt44  | char(50)    | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.001 sec)

刪除資料表的欄位
MariaDB [dic_order]> select * from test1;
+------+--------+----------+------+
| t1   | t2     | t3       | tt44 |
+------+--------+----------+------+
|    2 | banana | babanana | NULL |
+------+--------+----------+------+
1 row in set (0.002 sec)

MariaDB [dic_order]> alter table test1 drop tt44;
Query OK, 0 rows affected (0.075 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [dic_order]> select * from test1;
+------+--------+----------+
| t1   | t2     | t3       |
+------+--------+----------+
|    2 | banana | babanana |
+------+--------+----------+
1 row in set (0.001 sec)

```  
更新資料：
update 
```mysql
```

有必要的話也可以用刪除:  
drop table [tablename];  
```mysql
MariaDB [dic_order]> drop table test1;
Query OK, 0 rows affected (0.059 sec)

MariaDB [dic_order]> show tables;
Empty set (0.000 sec)
```  

## 談談select的用法  
在資料庫裡，最常也最容易使用到的指令就是select!!  
上面有提到select * from [tablename]; 這種簡單呼叫資料表的方法，接著會更深入的了解select的使用方法。  
1. where
2. and & or
3. like
4. group by & order by
5. limit
6. join
7. union
