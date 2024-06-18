# ДЗ 07  
### Найти жанры для каждого фильма  
> SELECT  
    films.name AS name,  
    genres.genre AS genre  
FROM movies AS films  
INNER JOIN genres AS genres ON films.id = genres.movie_id  
ORDER BY films.name ASC  

Результат:  
Первые 10 позиций
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ07/img/001.png)  

### Запросить все фильмы, у которых нет жанра  
> SELECT films.name,
genres.genre AS genre    
FROM movies AS films  
LEFT JOIN genres AS genres ON films.id = genres.movie_id  
WHERE genres.movie_id = 0  
ORDER BY films.name ASC

Результат:  
Первые 10 позиций  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ07/img/002.png)  

### Объединить каждую строку из таблицы "Фильмы" с каждой строкой из таблицы "Жанры"  
> SELECT  
    films.name,  
    films.id,  
    genres.movie_id,  
    genres.genre  
FROM movies AS films  
CROSS JOIN genres AS genres

Результат:  
Первые 10 позиций 
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ07/img/003.png)  

### Найти жанры для каждого фильма, НЕ используя INNER JOIN  
> SELECT  
    films.name AS name,  
    genres.genre AS genre  
FROM movies AS films  
CROSS JOIN genres AS genres  
WHERE films.id = genres.movie_id  
ORDER BY films.name ASC  

Результат:  
Первые 10 позиций  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ07/img/004.png)  

### Найти всех актеров и актрис, снявшихся в фильме в 2023 году  
>SELECT  
    actor.first_name,  
    actor.last_name  
FROM actors AS actor  
LEFT SEMI JOIN roles AS role ON actor.id = role.actor_id  
WHERE toYear(created_at) = '2023'  
ORDER BY id ASC  

Найти снявшихся в 2023 не удалось, т.к. дата в таблице roles заполнилась значением по умолчанию now(), в моем случае 2024-06-18 20:07:32.  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ07/img/005.png)  

Результат выборки по 2024 году:  
Первые 10 позиций  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ07/img/006.png)  

### Запросить все фильмы, у которых нет жанра, через ANTI JOIN  
> SELECT  
    films.name,  
    genres.genre  
FROM movies AS films  
ANTI LEFT JOIN genres AS genres ON films.id = genres.movie_id  
ORDER BY name ASC  

Результат:  
Первые 10 позиций  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ07/img/007.png)
