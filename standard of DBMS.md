## 下面會以資料型態、運算子、函式，三個部分做比較  
1. 首先看資料型態，資料型態這邊大概分兩種  
   * 字串：char、varchar...。  
   * 數字：integer、float、long...。  
   * 時間：DATE、TIME...。  
2. 接下來是運算子，這邊分三種  
   * 一般加減(binary)：+、-...。  
   * 比較運算：> 、 < 、 <>...。  
   * 邏輯運算：AND、OR...。  
3. 最後是功能(function)　
   * MAX：取選擇區域內最大的數值。　　
   * AVG：將區域內的所有數值加起來平均。　　
   
|    資料庫   |   資料型態  |
|:----------:|:-----------:|
|     SQL    |CHARACTER、CHARACTER VARYING、NATIONAL CHARACTER、NATIONAL CHARACTER VARYING、BIT、BIT VARYING|
|  SQL-數字  |NUMERIC、DECIMAL、INTEGER、SMALLINT、FLOAT、REAL、DOUBLE PRECISION|
|  SQL-時間  |DATE、TIME、TIMESTAMP、TIME WITH TIME ZONE、TIMESTAMP WITH TIME ZONE、INTERVAL|
|SQL-間隔資料類型|INTERVAL|
|    MySQL   |TINYINT、MEDIUMINT、BIGINT、DOUBLE[8.0.17後不推薦]|
|   Oracle   |VARCHAR2、(Rowid Data Types)|  

|    資料庫   |    運算子   |
|:----------:|:-----------:|
|     SQL    |％、＆、｜、｜｜、＊、＋、，、－、。、／、＜、＝、＞、ˍ、＜＞、＞＝、＜＝、AND、OR、NOT、BETWEEN、IN、LIKE、IS、NULL、ALL、（ANY SOME）、EXISTS、MATCH、OVERLAPS、UNIQUE、：、（、）、；、？、〔、〕、。。、”、’|
|    MySQL   |<=>、!=、:=、REGEXP、CASE、WHEN、THEN、ELSE、XOR|
|   Oracle   |MULTISET EXCEPT、MULTISET INTERSECT、MULTISET UNION、IS OF、Compound|  

|    資料庫   |    函式   |
|:----------:|:-----------:|
|     SQL    |AVG、COUNT、FIRST、MAX、MIN、STDEV、STDEVP、SUM、VAR、VARP、UCASE、LCASE、MID、LEN、INSTR、LEFT、RIGHT、ROUND、MOD、NOW、FORMAT、DATEDIFF|
|    MySQL   |[MySQL函式](https://dev.mysql.com/doc/refman/8.0/en/func-op-summary-ref.html)|
|   Oracle   |[Oracle函式](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/Functions.html#GUID-D079EFD3-C683-441F-977E-2C9503089982)|  



## 參考資料  
SQLwiki[function]：https://zh.wikipedia.org/wiki/SQL#%E6%A8%99%E6%BA%96%E5%8C%96  
SQL92standard[4.1-4.5,5.1,5.2]：https://www.contrib.andrew.cmu.edu/~shadow/sql/sql1992.txt  
MySQL-operators：https://dev.mysql.com/doc/refman/8.0/en/non-typed-operators.html  
MySQL-function：https://dev.mysql.com/doc/refman/8.0/en/func-op-summary-ref.html  
MySQL-datatypes：https://dev.mysql.com/doc/refman/8.0/en/data-types.html  
Oracle資料型態：https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/Data-Types.html#GUID-A3C0D836-BADB-44E5-A5D4-265BA5968483  
Oracle運算子：https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/Operators.html#GUID-874EFABC-F473-44A3-BC93-CDCAC28B131A  
Oracle函式：https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/Functions.html#GUID-D079EFD3-C683-441F-977E-2C9503089982
  
*僅作紀錄用
