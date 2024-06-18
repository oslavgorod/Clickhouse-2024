# ДЗ 07  
### Найти жанры для каждого фильма  
> SELECT  
    films.name AS name,  
    genres.genre AS genre  
FROM movies AS films  
INNER JOIN genres AS genres ON films.id = genres.movie_id  
ORDER BY films.name ASC  
LIMIT 10

