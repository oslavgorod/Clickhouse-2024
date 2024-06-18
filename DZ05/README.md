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
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/img/001.png)  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/img/002.png)  

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
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/img/003.png)  

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
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/img/004.png)  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/img/005.png)  

### 4.  
> CREATE TABLE tbl4  
(  
    CounterID UInt8,  
    StartDate Date,  
    UserID UInt64  
)  
ENGINE = MergeTree  
PARTITION BY toYYYYMM(StartDate)  
ORDER BY (CounterID, StartDate)  

Вставляем данные:  
> INSERT INTO tbl4 VALUES(0, '2019-11-11', 1);  
INSERT INTO tbl4 VALUES(1, '2019-11-12', 1);  
_____________________________________________________________________
> CREATE TABLE tbl5  
(  
    CounterID UInt8,  
    StartDate Date,  
    UserID AggregateFunction(uniq, UInt64)  
)  
ENGINE = AggregatingMergeTree  
PARTITION BY toYYYYMM(StartDate)  
ORDER BY (CounterID, StartDate)

Вставляем данные:  
> INSERT INTO tbl5  
select CounterID, StartDate, uniqState(UserID)  
from tbl4  
group by CounterID, StartDate;

> INSERT INTO tbl5 VALUES (1,'2019-11-12',1);

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/img/006.png)  

Результат:  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/img/007.png)  

### 5.  
> CREATE TABLE tbl6  
(  
    id Int32,  
    status String,  
    price String,  
    comment String,  
    sign Int8  
)  
ENGINE = CollapsingMergeTree(sign)  
PRIMARY KEY id  
ORDER BY (id, status)

Вставляем данные:  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/img/008.png)  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ05/img/009.png)  
> INSERT INTO tbl6 VALUES (23, 'success', '1000', 'Confirmed', 1);
INSERT INTO tbl6 VALUES (23, 'success', '1000', 'Confirmed', -1), (23, 'success', '2000', 'Cancelled', 1);

Результат:  

