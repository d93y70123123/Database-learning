# 簡介
資料庫有個功能「replication」，目的是讓兩台資料庫的資料可以同步~  
這邊使用Mariadb10來當作範例。  
可以把replication當作一個叢集來看，分成Master、Slave，主要使用Master這台主機，Slave這邊主要同步資料。  
所以安裝的過程分成Master跟Slave。  

# Master  
## 1. 安裝  
新增repo  
```mysql
[mariadb]
name=mariadb
baseurl=https://mirrors.nxthost.com/mariadb//mariadb-10.4.11/yum/centos73-amd64/
gpgcheck=0
```

安裝Mariadb-server的同時會把其他相依性的軟體一起安裝  
```mysql
[root@ha1 ~]# yum install MariaDB-server
....
========================================================================================================================================================================
 Package                                         Arch                           Version                                           Repository                       Size
========================================================================================================================================================================
Installing:
 MariaDB-compat                                  x86_64                         10.4.11-1.el7.centos                              mariadb                         2.8 M
     replacing  mariadb-libs.x86_64 1:5.5.64-1.el7
 MariaDB-server                                  x86_64                         10.4.11-1.el7.centos                              mariadb                          26 M
Installing for dependencies:
 MariaDB-client                                  x86_64                         10.4.11-1.el7.centos                              mariadb                          12 M
 MariaDB-common                                  x86_64                         10.4.11-1.el7.centos                              mariadb                          81 k
 boost-program-options                           x86_64                         1.53.0-27.el7                                     base                            156 k
 galera-4                                        x86_64                         26.4.3-1.rhel7.el7.centos                         mariadb                         9.3 M
 lsof                                            x86_64                         4.87-6.el7                                        base                            331 k
 perl-Compress-Raw-Bzip2                         x86_64                         2.061-3.el7                                       base                             32 k
 perl-Compress-Raw-Zlib                          x86_64                         1:2.061-4.el7                                     base                             57 k
 perl-DBI                                        x86_64                         1.627-4.el7                                       base                            802 k
 perl-Data-Dumper                                x86_64                         2.145-3.el7                                       base                             47 k
 perl-IO-Compress                                noarch                         2.061-2.el7                                       base                            260 k
 perl-Net-Daemon                                 noarch                         0.48-5.el7                                        base                             51 k
 perl-PlRPC                                      noarch                         0.2020-14.el7                                     base                             36 k
 rsync                                           x86_64                         3.1.2-6.el7_6.1                                   base                            404 k

Transaction Summary
========================================================================================================================================================================
Install  2 Packages (+13 Dependent packages)

Total download size: 52 M
Is this ok [y/d/N]: y
...
Complete!
```

啟動、開機啟動  
```bash
[root@ha1 ~]# systemctl start mariadb.service
[root@ha1 ~]# systemctl enable mariadb.service
Created symlink from /etc/systemd/system/mysql.service to /usr/lib/systemd/system/mariadb.service.
Created symlink from /etc/systemd/system/mysqld.service to /usr/lib/systemd/system/mariadb.service.
Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.
```

## 2. 資料庫設定  
第一個問題輸入root密碼，預設為空，所以直接enter，然後輸入新的密碼  
其他通常都是選y，除非有特殊要求，這邊只是當作練習，所以全部都y  
```bash
[root@ha1 ~]# mysql_secure_installation
...
```

**修改my.cnf**  
這邊可能是版本問題，檔案裏面都是空的，不用擔心直接新增就好
```bash 
vim /etc/my.cnf

#
# This group is read both both by the client and the server
# use it for options that affect everything
#
[client-server]

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d

[mariadb]
# 開啟log-bin功能，讓slave可以同步  
log-bin 
# 指定id，並且在區網內不能重複  
server-id=1
# 這個只是記錄到log內的前贅字而已，沒設定的話就是會使用主機名稱，當然如果主機名稱有更改的話會有問題發生
log-basename=master1
```

接著重新啟動服務  
```bash
[root@ha1 ~]# systemctl restart mariadb
```

## 3. 新增使用者  
這個使用者是用來給replication使用的  
語法:CREATE USER 'youraccount'@'%' IDENTIFIED BY 'yourpasswd';  
GRANT REPLICATION SLAVE ON *.* TO 'youraccount'@'%';  
```mysql  
[root@ha1 ~]# mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 9
Server version: 10.4.11-MariaDB-log MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> CREATE USER 'ha_user' IDENTIFIED BY '123123';
Query OK, 0 rows affected (0.046 sec)

MariaDB [(none)]> GRANT REPLICATION SLAVE ON *.* TO 'ha_user';
Query OK, 0 rows affected (0.044 sec)  
```  

試著登入看看  
```mysql
[root@ha1 ~]# mysql -u ha_user -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 10
Server version: 10.4.11-MariaDB-log MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

現在把資料庫的資料dump出來，等等直接匯入到slave主機。  
```bash  
[root@ha1 ~]# mysqldump -u root -p --master-data --all-databases > hadb.sql
Enter password:
```
到這邊為止Master的部分設定完成了。  


# 接著設定Slave的部分  
重複Master的1、2步驟  

需要特別注意的是第2步驟，server-id在你的區網內不能有重複的不然會發生錯誤  
```bash
vim /etc/my.cnf

...
[mariadb]
log-bin 
server-id=2
log-basename=master2
```
重新啟動
```mysql
[root@ha2 ~]# systemctl restart mariadb 
```

## 輸入資料庫  
```mysql
[root@ha1 ~]# mysql -u root < hadb.sql
Enter password:
```

## 將資料庫加入叢集裡  
```mysql
[root@ha2 ~]# mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 8
Server version: 10.4.11-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> CHANGE MASTER TO
    ->   MASTER_HOST='ha1',
    ->   MASTER_USER='ha_user',
    ->   MASTER_PASSWORD='2727175#356',
    ->   MASTER_PORT=3306,
    ->   MASTER_LOG_FILE='master1-bin.000001',
    ->   MASTER_LOG_POS=944,
    ->   MASTER_CONNECT_RETRY=10;
Query OK, 0 rows affected (0.192 sec)

MariaDB [(none)]> START SLAVE;
Query OK, 0 rows affected (0.004 sec)
```

## 檢一下狀態  
```mysql
MariaDB [(none)]> SHOW SLAVE STATUS \G
*************************** 1. row ***************************
                Slave_IO_State: Connecting to master
                   Master_Host: ha1
                   Master_User: ha_user
                   Master_Port: 3306
                 Connect_Retry: 10
               Master_Log_File: master1-bin.000001
           Read_Master_Log_Pos: 944
                Relay_Log_File: ha2-relay-bin.000001
                 Relay_Log_Pos: 4
         Relay_Master_Log_File: master1-bin.000001
              Slave_IO_Running: Connecting
             Slave_SQL_Running: Yes
               Replicate_Do_DB:
           Replicate_Ignore_DB:
            Replicate_Do_Table:
        Replicate_Ignore_Table:
       Replicate_Wild_Do_Table:
   Replicate_Wild_Ignore_Table:
                    Last_Errno: 0
                    Last_Error:
                  Skip_Counter: 0
           Exec_Master_Log_Pos: 944
               Relay_Log_Space: 256
               Until_Condition: None
                Until_Log_File:
                 Until_Log_Pos: 0
            Master_SSL_Allowed: No
            Master_SSL_CA_File:
            Master_SSL_CA_Path:
               Master_SSL_Cert:
             Master_SSL_Cipher:
                Master_SSL_Key:
         Seconds_Behind_Master: NULL
 Master_SSL_Verify_Server_Cert: No
                 Last_IO_Errno: 2003
                 Last_IO_Error: error connecting to master 'ha_user@ha1:3306' - retry-time: 10  maximum-retries: 86400  message: Can't connect to MySQL server on 'ha1' (113 "No route to host")
                Last_SQL_Errno: 0
                Last_SQL_Error:
   Replicate_Ignore_Server_Ids:
              Master_Server_Id: 0
                Master_SSL_Crl:
            Master_SSL_Crlpath:
                    Using_Gtid: No
                   Gtid_IO_Pos:
       Replicate_Do_Domain_Ids:
   Replicate_Ignore_Domain_Ids:
                 Parallel_Mode: conservative
                     SQL_Delay: 0
           SQL_Remaining_Delay: NULL
       Slave_SQL_Running_State: Slave has read all relay log; waiting for the slave I/O thread to update it
              Slave_DDL_Groups: 0
Slave_Non_Transactional_Groups: 0
    Slave_Transactional_Groups: 0
1 row in set (0.000 sec)
```

可以看到Last_IO_Error: error connecting to master...，這邊有個錯誤  
原因是兩邊的防火牆沒有開啟3306的port，開了就沒事了~  


# 測試
ha1
```mysql
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.001 sec)
```

ha2  
```mysql
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.001 sec)
```  


## 在ha1這個Master新增資料表  
```mysql  
MariaDB [(none)]> create database user;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]> use user;
Database changed
MariaDB [user]> create table user(
    -> uid int,
    -> unum char(8),
    -> pwd char(64),
    -> uname char(20),
    -> cel char(10)
    -> );
Query OK, 0 rows affected (0.131 sec)
```

看一下ha2  
```bash
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
| user               |
+--------------------+
5 rows in set (0.001 sec)

MariaDB [user]> select * from user;
+------+----------+------+--------+------------+
| uid  | unum     | pwd  | uname  | cel        |
+------+----------+------+--------+------------+
|    1 | 4040C003 | 123  | asdasd | 09XXXXXXXX |
+------+----------+------+--------+------------+
```

## 接著新增幾筆資料  
ha1  
```mysql
MariaDB [user]> select * from user;
+------+----------+------+--------+------------+
| uid  | unum     | pwd  | uname  | cel        |
+------+----------+------+--------+------------+
|    1 | 4040C003 | 123  | asdasd | 09XXXXXXXX |
+------+----------+------+--------+------------+
1 row in set (0.000 sec)

MariaDB [user]> insert into user values(1,'4040C003','123','asdasd','09XXXXXXXX');
Query OK, 1 row affected (0.058 sec)

MariaDB [user]> insert into user values(1,'4040C003','123','asdasd','09XXXXXXXX');
Query OK, 1 row affected (0.020 sec)

MariaDB [user]> insert into user values(1,'4040C003','123','asdasd','09XXXXXXXX');
Query OK, 1 row affected (0.032 sec)
....
..
```

看一下ha2有沒有同步  
```mysql
[root@ha2 ~]# mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 12
Server version: 10.4.11-MariaDB-log MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> use user;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [user]> select * from user;
+------+----------+------+--------+------------+
| uid  | unum     | pwd  | uname  | cel        |
+------+----------+------+--------+------------+
|    1 | 4040C003 | 123  | asdasd | 09XXXXXXXX |
|    1 | 4040C003 | 123  | asdasd | 09XXXXXXXX |
|    1 | 4040C003 | 123  | asdasd | 09XXXXXXXX |
|    1 | 4040C003 | 123  | asdasd | 09XXXXXXXX |
|    1 | 4040C003 | 123  | asdasd | 09XXXXXXXX |
|    1 | 4040C003 | 123  | asdasd | 09XXXXXXXX |
|    1 | 4040C003 | 123  | asdasd | 09XXXXXXXX |
+------+----------+------+--------+------------+
14 rows in set (0.000 sec)
```



