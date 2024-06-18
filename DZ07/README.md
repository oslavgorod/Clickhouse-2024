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
> SELECT films.name  
FROM movies AS films  
LEFT JOIN genres AS genres ON films.id = genres.movie_id  
WHERE genres.movie_id = 0  
ORDER BY films.name ASC

Результат:  
Первые 10 позиций  
