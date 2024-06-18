# ДЗ 05  
### 1.  
> CREATE TABLE tbl1  
(  
    UserID UInt64,  
    PageViews UInt8,  
    Duration UInt8,  
    Sign Int8,  
    Version UInt8  
)  
ENGINE = CollapsingMergeTree(Sign)  
ORDER BY UserID

Вставляем данные:  
> INSERT INTO tbl1 VALUES (4324182021466249494, 5, 146, -1, 1);  
INSERT INTO tbl1 VALUES (4324182021466249494, 5, 146, 1, 1),(4324182021466249494, 6, 185, 1, 2);

Результат:  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/001.png)  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/002.png)  

### 2.  
> CREATE TABLE tbl2  
(  
    key UInt32,  
    value UInt32  
)  
ENGINE = SummingMergeTree  
ORDER BY key

Вставляем данные:
> INSERT INTO tbl2 Values(1,1),(1,2),(2,1);

Результат:  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/003.png)  

### 3.  
> CREATE TABLE tbl3  
(  
    id Int32,  
    status String,  
    price String,  
    comment String  
)  
ENGINE = ReplacingMergeTree  
PRIMARY KEY id  
ORDER BY (id, status)  

Вставляем данные:  
> INSERT INTO tbl3 VALUES (23, 'success', '1000', 'Confirmed');  
INSERT INTO tbl3 VALUES (23, 'success', '2000', 'Cancelled');

Результат:  
