# sql語法教學  
資料庫管理系統(Database Management System,DBMS)是一種管理資料庫的大型軟體，是用於建立、使用和維護資料庫。  
具有代表性的資料管理系統有：Oracle、Microsoft SQL Server、Access、MySQL及PostgreSQL等，這邊使用的是MariaDB做介紹。  
```diff 
- 除了sql語法外，如：show databases 、 descripe [tablesname]......非針對資料表欄位操作的指令，在各個資料庫系統都不一定一樣
```

## 安裝環境  
作業系統：centos7  
MariaDB版本：10.4.8

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
#### 先介紹如何建立資料庫:  
CREATE database [DATABASE NAME];
```bash
MariaDB [(none)]> create database dic_order;
Query OK, 1 row affected (0.00 sec)
```

#### 使用剛剛建立的資料庫:
use [dbname];
```
MariaDB [(none)]> use dic_order;
Database changed
MariaDB [dic_order]>
```

#### 查看建立的資料表:
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
#### 刪除資料庫:  
```bash
MariaDB [(none)]> drop database order1;
Query OK, 0 rows affected (0.00 sec)
```  

接著是使用者的部分，分別介紹三種功能。  
#### 新增:  
CREATE USER 'Username'@'localhost' IDENTIFIED BY 'NewPasswd';
```bash
MariaDB [(none)]> CREATE USER 'kai'@'localhost' IDENTIFIED BY 'ispassword';
Query OK, 0 rows affected (0.00 sec)
```
#### 查看:  
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
#### 刪除:  
delete from mysql.user where User='[USERNAME]';
```bash
MariaDB [(none)]> delete from mysql.user where User='kai@localhost';
Query OK, 1 row affected (0.00 sec)
``` 

## 建立資料表  
資料庫完成後就是資料表，這邊分成建立資料表、新增資料、更新資料、修改資料、刪除資料。  
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
#### 查看所有資料表:  
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
#### 新增資料:  
insert into [tablename] values('資料1','資料2'...);
```mysql
MariaDB [dic_order]> insert into test1 values(1,'apple','pieapple');
Query OK, 1 row affected (0.030 sec)
```  
#### 列出資料:  
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
#### 刪除資料:  
delete  from [tablename] where [欄位]='[值]';
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

#### 修改資料:  
alter table [資料表名稱] [add|change|modify|drop] [欲更改欄位] [資料型態];  
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
#### 更新資料表裡的資料：
update [表格名稱] set [欄位名稱1]='[值1]' where [欄位名稱2]='[值2]';  
update 使用方法會比較抽象一點，因為他必須要用另外一個欄位來當作條件，若是沒有另外一個欄位當作條件的話，就會整欄的值都會被改掉。
```mysql
MariaDB [dic_order]> select * from test1;
+------+-----------+------------+
| t1   | t2        | t3         |
+------+-----------+------------+
|    2 | banana    | babanana   |
|    3 | chocolate | chocolate2 |
+------+-----------+------------+
2 rows in set (0.001 sec)

沒有條件的情況下會是怎樣:
MariaDB [dic_order]> update test1 set t3='pork';
Query OK, 2 rows affected (0.029 sec)
Rows matched: 2  Changed: 2  Warnings: 0

MariaDB [dic_order]> select * from test1;
+------+-----------+------+
| t1   | t2        | t3   |
+------+-----------+------+
|    2 | banana    | pork |
|    3 | chocolate | pork |
+------+-----------+------+
2 rows in set (0.001 sec)

若是增加條件又會是怎樣:
MariaDB [dic_order]> update test1 set t3='noodle' where t1=2;
Query OK, 1 row affected (0.064 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [dic_order]> select * from test1;
+------+-----------+--------+
| t1   | t2        | t3     |
+------+-----------+--------+
|    2 | banana    | noodle |
|    3 | chocolate | pork   |
+------+-----------+--------+
2 rows in set (0.001 sec)
```
所以增加條件是非常重要的，如果今天是在處理重要的資料時，沒有設定條件的話後果會是非常慘烈的。

#### 刪除資料表:  
drop table [tablename];  
```mysql
MariaDB [dic_order]> drop table test1;
Query OK, 0 rows affected (0.059 sec)

MariaDB [dic_order]> show tables;
Empty set (0.000 sec)
```  

到這邊已經可以簡單的操作資料庫了，但是要能夠更好的掌握就必須要學習更多的技術，所以往下看吧~  

## 談談select的用法  
在資料庫裡，最常也最容易使用到的指令就是select!!  
上面有提到select * from [tablename]; 這種簡單呼叫資料表的方法，接著會更深入的了解select的使用方法。  
首先，來解析一下語法 select (drink) from [store] ，這句話的意思是從[商店]中選(飲料)，所以當有需要從資料表中選取資料的時候就也必須要這樣做。
從[資料表]中選取[資料]是最基本且重要的語法，看完語法後來瞭解一下資料的結構吧：
如果有在用excel的人就知道，所有資料都儲存於欄位中，欄位分別又在[列]跟[欄]上，像是下面這樣
```
----|列1----|列2----|
欄1-|資料1-1|資料2-1|
欄2-|資料1-2|資料2-2|
```

瞭解完[欄]跟[列]的分配後，就開始好好認識select這把好用的工具吧。  
0. select  
select 本身可以選出你要的資料  
語法：select [欄位名稱] from [資料表名稱];  
```mysql
通常對剛增加資料的表會做一個動作，就是查看有沒有輸入正確，所以就會像下面這樣下指令。
MariaDB [dic_order]> select * from test1;
+------+-----------+--------+
| t1   | t2        | t3     |
+------+-----------+--------+
|    2 | banana    | noodle |
|    3 | chocolate | pork   |
+------+-----------+--------+
2 rows in set (0.002 sec)

可以看到 select 後面的*，這個星號是萬用字元，代表的是所有字他都會運作。
所以這一句的意思就是，選擇所有資料且是從test1這張表列出。

除了 * 我們也可以指定欄位名稱
MariaDB [dic_order]> select t2 from test1;
+-----------+
| t2        |
+-----------+
| banana    |
| chocolate |
+-----------+
2 rows in set (0.002 sec)

像這樣就可以只選擇這一欄位的所有資料了。
```
1. where  
使用 where 是要給select加上限制條件，達到自己的需求。  
語法：select [要列出來的欄位] from [表格] where [欄位名稱] [運算符號] '[值]';
```mysql
MariaDB [dic_order]> select * from test1;
+------+-----------+--------+
| t1   | t2        | t3     |
+------+-----------+--------+
|    2 | banana    | noodle |
|    3 | chocolate | pork   |
+------+-----------+--------+
2 rows in set (0.004 sec)

加上where後

MariaDB [dic_order]> select * from test1 where t1=3;
+------+-----------+------+
| t1   | t2        | t3   |
+------+-----------+------+
|    3 | chocolate | pork |
+------+-----------+------+
1 row in set (0.002 sec)

可以看到後面 where t1=3 的部分
t1 就是[欄位名稱]
=  就是運算符號
3  就是該欄位的值

運算符號就是可以拿來做加減乘除等等....。
```
2. and & or  
[and]這一個判斷式就是需要兩邊條件都成立才會成功。  
[or]則是兩邊條件有一邊成立就會成功。  
select * from [資料表名稱] where [欄位名稱1] = '[值1]' [and|or] [欄位名稱2] = '[值2]';  
```
MariaDB [dic_order]> select * from test1 where t1=2 and t1=3;
Empty set (0.002 sec)

MariaDB [dic_order]> select * from test1;
+------+-----------+--------+
| t1   | t2        | t3     |
+------+-----------+--------+
|    2 | banana    | noodle |
|    3 | chocolate | pork   |
+------+-----------+--------+
2 rows in set (0.001 sec)

當條件是t1=2而且t1=3
MariaDB [dic_order]> select * from test1 where t1=2 and t1=3;
Empty set (0.000 sec)

當條件是t1=2或t1=3
MariaDB [dic_order]> select * from test1 where t1=2 or t1=3;
+------+-----------+--------+
| t1   | t2        | t3     |
+------+-----------+--------+
|    2 | banana    | noodle |
|    3 | chocolate | pork   |
+------+-----------+--------+
2 rows in set (0.001 sec)

MariaDB [dic_order]>
```  

3. like  
like 在這裡翻譯成[像]，所以它要做的事情就是找出類似的東西。  
like 需要跟許多萬用字元做搭配，所以忘記的話可以到網路上尋找 'sql 萬用字元'，找到自己需要的。
語法：select * from [資料表名稱] where [欄位名稱] like '[要找的東西]';  
這邊列幾個可能會用到的:  
```mysql
一個底線代表一個字，所以這邊有4個底線，也就是說介於b跟a之間有四個不知道什麼字，只要找出類似的就好。如果是5個字就不行了。
MariaDB [dic_order]> select * from test1 where t2 like 'b____a';
+------+--------+--------+
| t1   | t2     | t3     |
+------+--------+--------+
|    2 | banana | noodle |
+------+--------+--------+
1 row in set (0.001 sec)

ba% 的意思要找出是ba開頭的英文字母。
MariaDB [dic_order]> select * from test1 where t2 like 'ba%';
+------+--------+--------+
| t1   | t2     | t3     |
+------+--------+--------+
|    2 | banana | noodle |
+------+--------+--------+
1 row in set (0.001 sec)

%ba 的意思要找出是ba結尾的英文字母。
MariaDB [dic_order]> select * from test1 where t2 like '%ba';
Empty set (0.001 sec)

%na% 的意思要找出包含na的字串。
MariaDB [dic_order]> select * from test1 where t2 like '%na%';
+------+--------+--------+
| t1   | t2     | t3     |
+------+--------+--------+
|    2 | banana | noodle |
+------+--------+--------+
1 row in set (0.002 sec)
```
4. order by  
既然是資料總有需要排序的時候，也就是比大小，這時候就會用到這個功能了。
語法：select * from [資料表名稱] {where 條件} order by [欄位名稱] [ASC, DESC];
ASC是從小排到大，DESC是從大排到小，如果沒有加這個選項預設是ASC，且如果有where子句的話，優先順序會在 order by 前。
#### 排序前
```mysql
MariaDB [dic_order]> select * from shop;
+----------+-------+---------------------+
| name     | price | time                |
+----------+-------+---------------------+
| apple    |    20 | 2019-11-21 12:58:46 |
| apple    |    20 | 2019-11-21 12:58:58 |
| banana   |    20 | 2019-11-21 12:58:58 |
| apple    |    50 | 2019-11-21 12:58:58 |
| egg      |    10 | 2019-11-21 12:58:58 |
| cake     |    60 | 2019-11-21 12:58:58 |
| dumpling |    40 | 2019-11-21 12:58:58 |
| banana   |    30 | 2019-11-21 12:58:59 |
+----------+-------+---------------------+
8 rows in set (0.003 sec)
```
#### 排序後
```mysql
MariaDB [dic_order]> select * from shop order by price;
+----------+-------+---------------------+
| name     | price | time                |
+----------+-------+---------------------+
| egg      |    10 | 2019-11-21 12:58:58 |
| apple    |    20 | 2019-11-21 12:58:46 |
| apple    |    20 | 2019-11-21 12:58:58 |
| banana   |    20 | 2019-11-21 12:58:58 |
| banana   |    30 | 2019-11-21 12:58:59 |
| dumpling |    40 | 2019-11-21 12:58:58 |
| apple    |    50 | 2019-11-21 12:58:58 |
| cake     |    60 | 2019-11-21 12:58:58 |
+----------+-------+---------------------+
8 rows in set (0.001 sec)

由大到小排序
MariaDB [dic_order]> select * from shop order by price desc;
+----------+-------+---------------------+
| name     | price | time                |
+----------+-------+---------------------+
| cake     |    60 | 2019-11-21 12:58:58 |
| apple    |    50 | 2019-11-21 12:58:58 |
| dumpling |    40 | 2019-11-21 12:58:58 |
| banana   |    30 | 2019-11-21 12:58:59 |
| apple    |    20 | 2019-11-21 12:58:46 |
| apple    |    20 | 2019-11-21 12:58:58 |
| banana   |    20 | 2019-11-21 12:58:58 |
| egg      |    10 | 2019-11-21 12:58:58 |
+----------+-------+---------------------+
8 rows in set (0.001 sec)

加上 where條件
MariaDB [dic_order]> select * from shop where name='apple' order by price desc;
+-------+-------+---------------------+
| name  | price | time                |
+-------+-------+---------------------+
| apple |    50 | 2019-11-21 12:58:58 |
| apple |    20 | 2019-11-21 12:58:46 |
| apple |    20 | 2019-11-21 12:58:58 |
+-------+-------+---------------------+
3 rows in set (0.001 sec)
```

5. group by  
當有些資料重複，但是又必須要統計的時候就會用到group by了，也就是說會將重複的資料整合起來變成一筆資料。
語法：select * from [資料表名稱] {where 條件} group by [欄位名稱];
#### 整理前
```
MariaDB [dic_order]> select * from shop;
+----------+-------+---------------------+
| name     | price | time                |
+----------+-------+---------------------+
| apple    |    20 | 2019-11-21 12:58:46 |
| apple    |    20 | 2019-11-21 12:58:58 |
| banana   |    20 | 2019-11-21 12:58:58 |
| apple    |    50 | 2019-11-21 12:58:58 |
| egg      |    10 | 2019-11-21 12:58:58 |
| cake     |    60 | 2019-11-21 12:58:58 |
| dumpling |    40 | 2019-11-21 12:58:58 |
| banana   |    30 | 2019-11-21 12:58:59 |
+----------+-------+---------------------+
8 rows in set (0.002 sec)
```

#### 當我們需要算出每個商品的總金額，且不希望重複出現資料時
```mysql
MariaDB [dic_order]> select * from shop group by name;
+----------+-------+---------------------+
| name     | price | time                |
+----------+-------+---------------------+
| apple    |    20 | 2019-11-21 12:58:46 |
| banana   |    20 | 2019-11-21 12:58:58 |
| cake     |    60 | 2019-11-21 12:58:58 |
| dumpling |    40 | 2019-11-21 12:58:58 |
| egg      |    10 | 2019-11-21 12:58:58 |
+----------+-------+---------------------+
5 rows in set (0.001 sec)

現在可以發現沒有重複的商品資料了，但是價錢並沒有被總合起來。
將商品價錢總和的話，就需要用到sum()這個函式了
MariaDB [dic_order]> select *,sum(price) from shop group by name;
+----------+-------+---------------------+------------+
| name     | price | time                | sum(price) |
+----------+-------+---------------------+------------+
| apple    |    20 | 2019-11-21 12:58:46 |         90 |
| banana   |    20 | 2019-11-21 12:58:58 |         50 |
| cake     |    60 | 2019-11-21 12:58:58 |         60 |
| dumpling |    40 | 2019-11-21 12:58:58 |         40 |
| egg      |    10 | 2019-11-21 12:58:58 |         10 |
+----------+-------+---------------------+------------+
5 rows in set (0.002 sec)
可以看到最右邊的欄位出現了 sum(price) ，這就是我們要的價錢總和
```

6. limit  
有時候資料量很大，只想要抓到自己想看的幾行時，limit就是好幫手了。
語法：select * from [資料表名稱] {where 條件} limit [2|0,3];
#### 沒有限制前
```mysql
MariaDB [dic_order]> select * from shop;
+----------+-------+---------------------+
| name     | price | time                |
+----------+-------+---------------------+
| apple    |    20 | 2019-11-21 12:58:46 |
| apple    |    20 | 2019-11-21 12:58:58 |
| banana   |    20 | 2019-11-21 12:58:58 |
| apple    |    50 | 2019-11-21 12:58:58 |
| egg      |    10 | 2019-11-21 12:58:58 |
| cake     |    60 | 2019-11-21 12:58:58 |
| dumpling |    40 | 2019-11-21 12:58:58 |
| banana   |    30 | 2019-11-21 12:58:59 |
+----------+-------+---------------------+
8 rows in set (0.001 sec)
```

限制行數後
```mysql
MariaDB [dic_order]> select * from shop limit 2;
+-------+-------+---------------------+
| name  | price | time                |
+-------+-------+---------------------+
| apple |    20 | 2019-11-21 12:58:46 |
| apple |    20 | 2019-11-21 12:58:58 |
+-------+-------+---------------------+
2 rows in set (0.001 sec)

MariaDB [dic_order]> select * from shop limit 3,5;
+----------+-------+---------------------+
| name     | price | time                |
+----------+-------+---------------------+
| apple    |    50 | 2019-11-21 12:58:58 |
| egg      |    10 | 2019-11-21 12:58:58 |
| cake     |    60 | 2019-11-21 12:58:58 |
| dumpling |    40 | 2019-11-21 12:58:58 |
| banana   |    30 | 2019-11-21 12:58:59 |
+----------+-------+---------------------+
5 rows in set (0.001 sec)
```
7. join  

8. union  

## 關於建立資料表的約束和檢查  
語法：constraint [約束名稱] check (限制條件);  
也可以不用constraint就直接使用check，下面會介紹。  
這邊個人建議使用約束名稱，因為這樣發生錯誤的話會有提示，比較容易找錯誤。  
假設有一張表，裡面有價錢，但是限制價錢就是要大於10的話。  
create table [資料表名稱](
price int,
price2 int check(price2>100),
.....
.....
constraint check_price check (price>10);
);
#### 先做一張練習用的表
```mysql
create table shop2(
name char(20),
price int,
constraint check_price check (price>10)
);

MariaDB [dic_order]> create table shop2(
    -> name char(20),
    -> price int,
    -> constraint check_price check (price>0)
    -> );
Query OK, 0 rows affected (0.099 sec)
```
#### 實驗條件有沒有成立
```mysql
MariaDB [dic_order]> insert into shop2 values('apple',9);
ERROR 4025 (23000): CONSTRAINT `check_price` failed for `dic_order`.`shop2`

MariaDB [dic_order]> insert into shop2 values('apple',10);
ERROR 4025 (23000): CONSTRAINT `check_price` failed for `dic_order`.`shop2`

MariaDB [dic_order]> insert into shop2 values('apple',11);
Query OK, 1 row affected (0.115 sec)
```


## view

