# 介紹安裝oracledb時，核心參數的含義  
先看一下需要增加那些基本參數
```
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 4294967295
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
```

## 接著各個看每個參數的功能  
1. `fs.aio-max-nr`: 所有非同步I/O情況的事件最大處理數量。  
2. `fs.file-max`: 所有系統程序的檔案處理最大數量。  
3. `kernel.shmall`: 在範圍內可以使用的shared memory pages 最大值，需要大於或等於shmmax。  
4. `kernel.shmmax`: 最大可以使用的單個shared memory，建議是實體記憶體的一半。以Byte為單位。  
5. `kernel.shmmni`: 範圍內可使用的最大數值，預設為4096。  
6. `kernel.sem`:負責分配各個程序可以使用的共用資源。底下有四種類別要設定。
7. `net.ipv4.ip_local_port_range`: port可以使用的範圍大小。  
8. `net.core.rmem_default`:預設接受的封包大小。  
9. `net.core.rmem_max`:最大可以接受的封包大小。  
10. `net.core.wmem_default`:預設傳送的封包大小。  
11. `net.core.wmem_max`:最大可以傳送的封包大小。  

## 參考文獻
Oracle 19c官網文獻: https://docs.oracle.com/en/database/oracle/oracle-database/19/cwlin/changing-kernel-parameter-values.html#GUID-FB0CC366-61C9-4AA2-9BE7-233EB6810A31
