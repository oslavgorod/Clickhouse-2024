# ДЗ 09  
### 1  
Для выполнения задания будет использоваться таблица trips, загруженная ранее из этого [датасета](https://clickhouse.com/docs/en/getting-started/example-datasets/nyc-taxi).  
### 2  
Создаем новую таблицу:  
>CREATE TABLE trips_rep AS trips  
ENGINE = ReplicatedMergeTree('/clickhouse/tables/{shard}/trips_v1`', '{replica}')  
PRIMARY KEY (pickup_datetime, dropoff_datetime)  
ORDER BY (pickup_datetime, dropoff_datetime)  
SETTINGS index_granularity = 8192

Находим partition_id от старой таблицы:  
>SELECT DISTINCT partition_id  
FROM system.parts  
WHERE concat(database, '.', 'table') = 'default.trips'

Присоединяем найденные партиции к новой таблице:  
>ALTER TABLE trips_rep  
    (ATTACH PARTITION ID 'all' FROM trips)

Удаляем старую таблицу и переименовывем созданную:  
>DROP TABLE trips;  
RENAME TABLE trips_rep TO trips;  
  
### 3  
