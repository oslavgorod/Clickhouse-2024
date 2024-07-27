# ДЗ 15  
### 1.  
Включаем text_log:  
> SET send_logs_level = 'trace'  
  
Выполняем запрос без использования primary key:  
> SELECT *  
FROM movies  
WHERE rank > 8  
LIMIT 20  
  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ15/img/001.png)  
На снимке экрана видно, что primary key не используется:  
`[click01] 2024.07.27 18:23:56.276264 [ 1156 ] {c02b8da8-c089-42d4-9ce6-a82476134cf4} <Debug> imdb.movies (0adf21db-96a0-428a-8226-8ab35e9b8215) (SelectExecutor): Key condition: unknown`  
  
Выполняем запрос с использованием primary key:  
> SELECT *  
FROM movies  
WHERE id >= 1328  
LIMIT 20  
  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ15/img/002.png)  
Тут видно, что ключ используется и значение берется из запроса:  
`[click01] 2024.07.27 18:25:03.979377 [ 1156 ] {29fa5287-0c22-43ee-acb2-66b4ae3ac129} <Debug> imdb.movies (0adf21db-96a0-428a-8226-8ab35e9b8215) (SelectExecutor): Key condition: (column 0 in [1328, +Inf))`  
  
### 2.  
Выполняем EXPLAIN для запроса с использованием primary key:  
> EXPLAIN indexes = 1  
SELECT *  
FROM movies  
WHERE id >= 1328  
LIMIT 20  
  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ15/img/003.png)  
