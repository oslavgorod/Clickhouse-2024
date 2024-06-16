# ДЗ 03

### 1. Создать новую базу данных и перейти в нее.

> CREATE DATABASE restaurant;  
USE restaurant;  

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ03/001.png)

### 2. Создать таблицу для бизнес-кейса "Меню ресторана" с 5+ полями, наполнить ее данными. Обязательно указывать, где нужно, модификаторы Nullable, LowCardinality и пр. Добавить комментарии.  

Создаем таблицу:  

> CREATE TABLE menu  
(  
    id Int32 COMMENT 'Идентификатор (целое число)',  
    name String COMMENT 'Название (строка)',  
    desc String COMMENT 'Описание (строка)',  
    category LowCardinality(String) COMMENT 'Категория (строка). Низкая кардинальность в данном случае используется т.к. категорий блюд будет заведомо малое количество',  
    price Decimal32(2) COMMENT 'Цена (число). Знаков после точки не может быть больше 2, т.к. минимальное значение не может быть ниже 1 копейки',  
    date DateTime COMMENT 'Дата добавления позиции в меню (датавремя)'  
)  
ENGINE = MergeTree  
ORDER BY id

Смотрим результат предыдущего запроса:  

> describe menu FORMAT Vertical  

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ03/002.png)  

Вставляем данные:  

> insert into menu (*) VALUES  
  (1,'Филе лосося с кабачками гриль и сливочно- цитрусовым соусом','С кабачками гриль и сливочно-цитрусовым соусом','Блюда из рыбы','600.50','2019-01-01 09:15:00'),  
  (2,'Командорский кальмар','Приготовленный на гриле с овощами и соусом сливочный хрен','Блюда из рыбы','420.00','2021-12-31 19:25:00'),  
  (3,'Фланк-стейк','С овощами гриль и соусом из черного перца','Горячие блюда','1450.99','2019-01-01 09:15:00'),  
  (4,'Рубленый бифштекс','С сыром, яйцом пашот, малосольными огурчиками и картофелем с чесночком','Горячие блюда','420.05','2024-05-30 16:00:00'),  
  (5,'Хлебная корзина','','Хлеб и выпечка','120.00','2019-01-01 09:15:00'),  
  (6,'Салат с адыгейским сыром гриль','С маринованным болгарским перцем, кабачками гриль и томатами под лимонно-оливковым дрессингом','Салаты','300.26','2024-05-30 16:00:00')  

### 3. Протестировать CRUD на созданной таблице.  

> select * from menu \G  

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ03/003.png)  

> alter table menu update price = '125.50' where id = 5

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ03/004.png)  

> alter table menu delete where date = '2021-12-31 19:25:00'  

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ03/005.png)  

### 4. Добавить несколько новых полей, удалить пару старых.  
Добавляем столбцы:  

> alter table menu add column added_1 Int32 FIRST;  
  alter table menu add column added_2 Float64 AFTER desc;  
  alter table menu add column added_3 DateTime64;

Проверяем результат:  
> describe menu FORMAT TSV;

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ03/006.png)  

Удаляем столбцы:  

> alter table menu drop column added_3;  
  alter table menu drop column date;

Проверяем:  
> describe menu FORMAT TSV;

![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ03/007.png)  

### 5. Заселектить таблицу (любую) из sample dataset [Menus](https://clickhouse.com/docs/en/getting-started/example-datasets/menus)  
