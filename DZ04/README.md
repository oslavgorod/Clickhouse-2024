# ДЗ 04  
## Вариант 1  
Создаем таблицу:  
>CREATE TABLE transactions  
(  
    &emsp;transaction_id UInt32,  
    &emsp;user_id UInt32,  
    &emsp;product_id UInt32,  
    &emsp;quantity UInt8,  
    &emsp;price Float32,  
    &emsp;transaction_date Date  
)  
ENGINE = MergeTree  
ORDER BY transaction_id  

Вставляем немного данных из файла dz04.csv:  
>INSERT INTO hw04.transactions SELECT  
    &emsp;transaction_id,  
    &emsp;user_id,  
    &emsp;product_id,  
    &emsp;quantity,  
    &emsp;price,  
    &emsp;toDateTime(transaction_date) AS transaction_date  
FROM file("/var/lib/clickhouse/user_files/dz04.csv")

### 1. Агрегатные функци  
1.1 Рассчитайте общий доход от всех операций:  
