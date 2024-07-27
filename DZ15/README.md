# ДЗ 15  
### 1.  
Включаем text_log:  
> SET send_logs_level = 'trace'  
  
Выполняем запрос без использования primary key:  
> SELECT *  
FROM movies  
WHERE rank > 8  
LIMIT 20  
  
Выполняем запрос с использованием primary key:  
> SELECT *  
FROM movies  
WHERE id >= 1328  
LIMIT 20  
  
### 2.  
Выполняем EXPLAIN для запроса с использованием primary key:  
> EXPLAIN indexes = 1  
SELECT *  
FROM movies  
WHERE id >= 1328  
LIMIT 20  
  
