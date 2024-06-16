# ДЗ 03

### 1. Создать новую базу данных и перейти в нее.

> CREATE DATABASE restaurant;  
USE restaurant;  

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ03/001.png)

### 2. Создать таблицу для бизнес-кейса "Меню ресторана" с 5+ полями, наполнить ее данными. Обязательно указывать, где нужно, модификаторы Nullable, LowCardinality и пр. Добавить комментарии.  

> CREATE TABLE menu
(
    `id` Int32 COMMENT 'Идентификатор (целое число)',
    `name` String COMMENT 'Название (строка)',
    `desc` String COMMENT 'Описание (строка)',
    `category` LowCardinality(String) COMMENT 'Категория (строка). Низкая кардинальность в данном случае используется т.к. категорий блюд будет заведомо малое количество',
    `price` Decimal32(2) COMMENT 'Цена (число). Знаков после точки не может быть больше 2, т.к. минимальное значение не может быть ниже 1 копейки',
    `date` DateTime COMMENT 'Дата добавления позиции в меню (датавремя)'
)
ENGINE = MergeTree
ORDER BY id  

