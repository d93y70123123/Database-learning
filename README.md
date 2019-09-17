# 在Centos上安裝OracleDB  
Oracle19c聽說是12c的終極版，至少提供高級支援2023年1月底。  
oracle的資料庫安裝相對麻煩，以我查詢到的資料有:  
1. 圖形介面安裝
2. 不用圖形介面安裝(使用rpm安裝)  

我認為第二種方法比較懶人一點，省去很多設定的動作。

## 安裝需求
因上課要求，所以安裝在centos系統。  
* swap至少512MB、1G
* 記憶體至少2G

## 安裝所需軟體
```
# yum -y update
# yum install -y binutils.x86_64 compat-libcap1.x86_64 gcc.x86_64 gcc-c++.x86_64 glibc.i686 glibc.x86_64 glibc-devel.i686 glibc-devel.x86_64 ksh compat-libstdc++-33 libaio.i686 libaio.x86_64 libaio-devel.i686 libaio-devel.x86_64 libgcc.i686 libgcc.x86_64 libstdc++.i686 libstdc++.x86_64 libstdc++-devel.i686 libstdc++-devel.x86_64 libXi.i686 libXi.x86_64 libXtst.i686 libXtst.x86_64 make.x86_64 sysstat.x86_64
```

## 到官網下載RPM
< Oracledb > : https://www.oracle.com/database/technologies/oracle-database-software-downloads.html  
< Oracle-preinstall下載處 > : https://yum.oracle.com/repo/OracleLinux/OL7/latest/x86_64/   
需要下載上述兩項東西  ((*注意，要在oracle官網上下載需要註冊會員。  

兩個安裝包會順便建立使用者跟群組還有設定其他系統參數。
```
# yum -y install oracle-database-preinstall-19c-1.0-1.el7.x86_64.rpm
# yum -y install oracle-database-ee-19c-1.0-1.x86_64.rpm
```

## 建立資料庫
安裝完oracledb跟preinstall之後，可以使用oracle提供的腳本建立資料庫。
*下面過程會建立蠻長一段時間，耐心等待  
```
# /etc/init.d/oracledb_ORCLCDB-19c configure
Configuring Oracle Database ORCLCDB.
準備資料庫作業
8% 完成
複製資料庫檔案
31% 完成
建立並啟動 Oracle 執行處理
32% 完成
36% 完成
40% 完成
43% 完成                                                                                                                                  46% 完成
完成資料庫建立
51% 完成
54% 完成
建立可插式資料庫
58% 完成
77% 完成
執行後續組態設定動作
100% 完成
資料庫建立完畢. 請參閱儲存在以下位置的日誌檔瞭解詳細資訊:
 /opt/oracle/cfgtoollogs/dbca/ORCLCDB.
資料庫資訊:
全域資料庫名稱:ORCLCDB
系統 ID (SID):ORCLCDB
請參閱日誌檔 "/opt/oracle/cfgtoollogs/dbca/ORCLCDB/ORCLCDB.log", 取得解進一步的詳細資訊.

Database configuration completed successfully. The passwords were auto generated, you must change them by connecting to the database using 'sqlplus / as sysdba' as the oracle user.
```

## 設定環境變數
接著需要將指令加入bash_profile
```
# su - oracle

# vim .bash_profile
export ORACLE_SID=ORCLCDB
export ORACLE_BASE=/opt/oracle/oradata
export ORACLE_HOME=/opt/oracle/product/19c/dbhome_1
export PATH=$PATH:$ORACLE_HOME/bin

# . .bash_profile
```

## 進入資料庫
安裝完也設定完環境變數，就可以進入資料庫了。
```
[oracle@oracle-kai ~]$ sqlplus / as sysdba

SQL*Plus: Release 19.0.0.0.0 - Production on Sat Sep 14 17:45:27 2019
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.


???:
Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Version 19.3.0.0.0

SQL>
```

參考資料
< Centoswiki-在CentOS7上安裝Oracle資料庫12c > : https://wiki.centos.org/zh-tw/HowTos/Oracle12onCentos7#head-8ae990a0a280db12a14fc62f8701aaafeb4f81ff  
< Oracle Database 18c:Install from RPM Package > : https://www.server-world.info/en/note?os=CentOS_7&p=oracle18c&f=6  
< OracleDB圖形化安裝影片教學 > : https://www.youtube.com/watch?v=ZvzFNqLRK7w  
