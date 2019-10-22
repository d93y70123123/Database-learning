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
|    MySQL   |[MySQL運算子](https://dev.mysql.com/doc/refman/8.0/en/non-typed-operators.html)|
|   Oracle   ||  

|    資料庫   |    函式   |
|:----------:|:-----------:|
|     SQL    |AVG、COUNT、FIRST、MAX、MIN、STDEV、STDEVP、SUM、VAR、VARP、UCASE、LCASE、MID、LEN、INSTR、LEFT、RIGHT、ROUND、MOD、NOW、FORMAT、DATEDIFF|
|    MySQL   |[MySQL函式](https://dev.mysql.com/doc/refman/8.0/en/func-op-summary-ref.html)|
|   Oracle   |         |  



## 參考資料  
SQLwiki[function]：https://zh.wikipedia.org/wiki/SQL#%E6%A8%99%E6%BA%96%E5%8C%96
SQL92standard：https://www.contrib.andrew.cmu.edu/~shadow/sql/sql1992.txt  
MySQL-operators：https://dev.mysql.com/doc/refman/8.0/en/non-typed-operators.html  
MySQL-function：https://dev.mysql.com/doc/refman/8.0/en/func-op-summary-ref.html  
MySQL-datatypes：https://dev.mysql.com/doc/refman/8.0/en/data-types.html  


*僅作紀錄用
