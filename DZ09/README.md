# ДЗ 09  
### 1  
Для выполнения задания будет использоваться таблица trips, загруженная из этого [датасета](https://clickhouse.com/docs/en/getting-started/example-datasets/nyc-taxi).  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ09/img/001.png)  
### 2  
Создаем новую таблицу:  
>CREATE TABLE trips_rep AS trips  
ENGINE = ReplicatedMergeTree('/clickhouse/tables/{shard}/trips_v1', '{replica}')  
PRIMARY KEY (pickup_datetime, dropoff_datetime)  
ORDER BY (pickup_datetime, dropoff_datetime)  
SETTINGS index_granularity = 8192  
  
Находим partition_id от старой таблицы:  
>SELECT DISTINCT partition_id  
FROM system.parts  
WHERE concat(database, '.', 'table') = 'default.trips'

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ09/img/002.png)  
  
Присоединяем найденные партиции к новой таблице:  
>ALTER TABLE trips_rep  
    &emsp;(ATTACH PARTITION ID 'all' FROM trips)

Удаляем старую таблицу и переименовывем созданную:  
>DROP TABLE trips;  
RENAME TABLE trips_rep TO trips;

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ09/img/003.png)  
  
### 3  
Добавляем две реплики. Пример настройки clickhouse-keeper:  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ09/img/004.png)  
В этом конфиге server_id должен быть уникален для каждой реплики. На скриншоте представлен конфиг со второй реплики из трех.  

Часть config.xml clickhouse-server:  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ09/img/005.png)  
Здесь задаем макросы и указываем zookeeperы. Тэг replica должен быть уникален для каждой реплики.  
### 4  
Выгружаем результат первого запроса в файл [001.json](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ09/001.json) :  
>clickhouse-client -q "SELECT getMacro(‘replica’), * FROM remote('click01,click02,click03',system.parts) FORMAT JSONEachRow;" > 001.json
  
Выгружаем результат второго запроса в файл [002.json](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ09/002.json) :  
>clickhouse-client -q "SELECT * FROM system.replicas FORMAT JSONEachRow;" > 002.json  
  
### 5  
>ALTER TABLE trips  
    &emsp;(MODIFY TTL dropoff_datetime + toIntervalDay(7))

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ09/img/006.png)  
  
