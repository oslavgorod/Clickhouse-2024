# ДЗ 02

Для установки Clickhouse была создана виртуальная машина на базе Ubuntu Server 24.04 следующей конфигурации:  
> - 4 CPU  
> - 4 GB RAM  
> - 40 GB SSD

Тестовый датасет был загружен и установлен из [официальной документации Clickhouse](https://clickhouse.com/docs/en/getting-started/example-datasets/nyc-taxi).  

Результат выполнения запроса и статус службы clickhouse-server:  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ02/clh.png)  

## Тестирование производительности  

Для измерения прроизводительности использовалась штатная утилита _clickhouse-benchmark_.  

Изменение парметров было сделано в файле [/etc/clickhouse-server/users.d/01_tune.xml](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ02/01_tune.xml)  

Параметрам _max_memory_usage_ и _max_memory_usage_for_all_queries_ установлено значение 4000000000. Данные праметры отвечают за макссимальный возможный объём оперативной памяти для выполнения запроса на одном сервере. По умолчанию значение 0, что подразумевает отсутствие ограничений.  

Параметру _max_concurrent_queries_for_user_ установлено значение 8 (количество ядер процессора * 2). Данный параметр определяет максимальное количество одновременно обрабатываемых запросов.  

Результаты тестирование представлены в [отчете](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ02/%D0%94%D0%9702.pdf)
