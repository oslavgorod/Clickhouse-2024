# ДЗ 04  
## Вариант 1  
Создаем таблицу:  
>CREATE TABLE transactions  
(  
    &ensp;transaction_id UInt32,  
    &emsp;user_id UInt32,  
    &emsp;product_id UInt32,  
    &emsp;quantity UInt8,  
    &emsp;price Float32,  
    &emsp;transaction_date Date  
)  
ENGINE = MergeTree  
ORDER BY transaction_id
