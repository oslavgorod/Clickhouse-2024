# ДЗ 12  
### 1. Создать пользователя jhon с паролем «qwerty»  
>CREATE USER jhon IDENTIFIED WITH sha256_password BY 'qwerty'  
  
### 2. Создать роль devs  
>CREATE ROLE devs  
  
### 3. Выдать роли devs права на SELECT на любую таблицу  
>GRANT SELECT ON *.* TO devs  
  
### 4. Выдать роль devs пользователю jhon  
>GRANT devs TO jhon  
  
### 5. Предоставить результаты SELECT из system-таблиц соответсвующих созданным сущностям  
