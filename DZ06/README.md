# ДЗ 06  
### 1.  
Создать таблицу с полями:  
 user_id UInt64,  
 action String,  
 expense UInt64  

>CREATE TABLE hw_06  
(  
    user_id UInt64,  
    action String,  
    expense UInt64  
)  
ENGINE = MergeTree  
PRIMARY KEY user_id  
ORDER BY user_id  
SETTINGS index_granularity = 8192

### 2.  
Создать словарь, в качестве ключа user_id, в качестве атрибута email String:  

>CREATE DICTIONARY hw06_dict  
(  
    user_id UInt64,  
    email String  
)  
PRIMARY KEY user_id  
SOURCE(FILE(PATH '/var/lib/clickhouse/user_files/dict.csv' FORMAT 'CSV'))  
LIFETIME(MIN 0 MAX 3600)  
LAYOUT(FLAT())

### 3.  
Наполнить таблицу и источник любыми данными, с низкоардинальными значениями для поля action и хотя бы по несколько повторяюихся строк для каждого user_id:  

>insert into hw_06 SELECT floor(randUniform(10000, 10999)) as user_id,  'create' as action, floor(randUniform(10000000, 19999999)) as expanse from numbers(5)  
insert into hw_06 SELECT floor(randUniform(10000, 10999)) as user_id,  'remove' as action, floor(randUniform(10000000, 19999999)) as expanse from numbers(5)  
insert into hw_06 SELECT floor(randUniform(10000, 10999)) as user_id,  'play' as action, floor(randUniform(10000000, 19999999)) as expanse from numbers(5)

Создаем повторяющиеся записи (повторил запрос 3 раза):  
>insert into hw_06 select * from hw_06

### 4.  
Написать SELECT, возвращающий:  
* email при помощи dictGet,  
* аккамулятивную сумму expense, c окном по action,  
* сортировка по email

>SELECT DISTINCT  
    user_id,  
    dictGet('hw06_dict', 'email', user_id) AS email,  
    sum(expense) OVER (PARTITION BY action) AS summa  
FROM hw_06  
ORDER BY email ASC  
