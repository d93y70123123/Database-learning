# Engines 
各個資料庫都有不同的引擎，用來針對不同的需求，現在要做不同引擎的使用下，效能會如何~  

## 查看引擎  
看現在有哪些引擎可以使用~  
```mysql
MariaDB [(none)]> show engines;
+--------------------+---------+-------------------------------------------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                                                         | Transactions | XA   | Savepoints |
+--------------------+---------+-------------------------------------------------------------------------------------------------+--------------+------+------------+
| CSV                | YES     | Stores tables as CSV files                                                                      | NO           | NO   | NO         |
| MRG_MyISAM         | YES     | Collection of identical MyISAM tables                                                           | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables                                       | NO           | NO   | NO         |
| Aria               | YES     | Crash-safe tables with MyISAM heritage. Used for internal temporary tables and privilege tables | NO           | NO   | NO         |
| MyISAM             | YES     | Non-transactional engine with good performance and small data footprint                         | NO           | NO   | NO         |
| SEQUENCE           | YES     | Generated tables filled with sequential values                                                  | YES          | NO   | YES        |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, foreign keys and encryption for tables                | YES          | YES  | YES        |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                                                              | NO           | NO   | NO         |
+--------------------+---------+-------------------------------------------------------------------------------------------------+--------------+------+------------+
8 rows in set (0.000 sec)
```

## 更換引擎  
語法：ALTER TABLE 表格名 ENGINE=InnoDB;
```mysql
MariaDB [dic_order]> show create table aaa;
+-------+----------------------------------------------------------------------------------------+
| Table | Create Table                                                                           |
+-------+----------------------------------------------------------------------------------------+
| aaa   | CREATE TABLE `aaa` (
  `num` int(11) DEFAULT NULL
) ENGINE=MEMORY DEFAULT CHARSET=utf8 |
+-------+----------------------------------------------------------------------------------------+
1 row in set (0.000 sec)

MariaDB [dic_order]> alter table aaa engine=aria;
Query OK, 0 rows affected (0.194 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [dic_order]> show create table aaa;
+-------+------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                         |
+-------+------------------------------------------------------------------------------------------------------+
| aaa   | CREATE TABLE `aaa` (
  `num` int(11) DEFAULT NULL
) ENGINE=Aria DEFAULT CHARSET=utf8 PAGE_CHECKSUM=1 |
+-------+------------------------------------------------------------------------------------------------------+
1 row in set (0.000 sec)
```

## 效能測試
接下來要進行各個引擎的效能測試，要做的事情有下面幾件：  
1. 換引擎  
2. 資料量一樣  
3. 乾淨的環境  
4. 時間紀錄
5. 數量要多
6. 包括了(insert、update、select)  
select又涵蓋了(=、>、<、>3 and <5、like %..%、like %.. 、 like ..%)  

了解需求後，就開始測試，這邊測試步驟如下：  
1. 先選擇資料結構  
2. 塞資料  
3. 刪除  
4. 重新塞資料
5. 做索引  
