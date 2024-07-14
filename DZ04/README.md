# ДЗ 04  
Задание лежит по [ссылке.](https://docs.google.com/document/d/1hHqRv7lUUDmDkhzXrd0GIx7deF9Ez4mKpoYzHtDjoqQ/edit)  
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

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ04/img/001.png)  
  
Вставляем немного данных из файла [dz04.csv](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ04/dz04.csv):  
>INSERT INTO hw04.transactions SELECT  
    &emsp;transaction_id,  
    &emsp;user_id,  
    &emsp;product_id,  
    &emsp;quantity,  
    &emsp;price,  
    &emsp;toDateTime(transaction_date) AS transaction_date  
FROM file("/var/lib/clickhouse/user_files/dz04.csv")

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ04/img/002.png)  

### 1. Агрегатные функци  
1.1 Рассчитайте общий доход от всех операций:  
>SELECT toDecimal32(sum(quantity * price), 2) AS summa  
FROM transactions

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ04/img/003.png)  
  
1.2 Найдите средний доход с одной сделки:  
>SELECT toDecimal32(avg(quantity * price), 2) AS summa  
FROM transactions

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ04/img/004.png)
  
1.3 Определите общее количество проданной продукции:  
>SELECT sum(quantity)  
FROM transactions

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ04/img/005.png)
 
1.4 Подсчитайте количество уникальных пользователей, совершивших покупку:  
>SELECT countDistinct(user_id)  
FROM transactions

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ04/img/006.png)  
  
### 2. Функции для работы с типами данных  
2.1 Преобразуйте "transaction_date" в строку формата "YYYY-MM-DD":  
>SELECT toString(transaction_date)  
FROM transactions

2.2 Извлеките год и месяц из "transaction_date":  
>SELECT  
    &emsp;toYear(transaction_date) AS Year,  
    &emsp;toMonth(transaction_date) AS Month  
FROM transactions  
  
2.3 Округлите "price" до ближайшего целого числа:  
>SELECT round(price, 0) AS Rounded  
FROM transactions

2.4 Преобразуйте "transaction_id" в строку:  
>SELECT toString(transaction_id)  
FROM transactions

### 3. User-Defined Functions (UDFs)  
3.1 Создайте простую UDF для расчета общей стоимости транзакции:  
>CREATE FUNCTION total_price AS (quantity, price) -> (quantity * price)
  
3.2 Используйте созданную UDF для расчета общей цены для каждой транзакции:  
>SELECT  
    &emsp;transaction_id,  
    &emsp;toDecimal32(total_price(quantity, price), 2) AS total_price  
FROM transactions  
  
3.3 Создайте UDF для классификации транзакций на «высокоценные» и «малоценные» на основе порогового значения (например, 100):  
>CREATE FUNCTION category AS total_price -> if(total_price > 100, 'High', 'Low')

3.4 Примените UDF для категоризации каждой транзакции:  
>SELECT  
    &emsp;transaction_id,  
    &emsp;toDecimal32(total_price(quantity, price), 2) AS total_price,  
    &emsp;category(total_price) AS category  
FROM transactions

