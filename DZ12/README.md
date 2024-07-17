# ДЗ 12  
### 1. Создать пользователя jhon с паролем «qwerty»  
>CREATE USER jhon IDENTIFIED WITH sha256_password BY 'qwerty'  
  
### 2. Создать роль devs  
>CREATE ROLE devs  
  
### 3. Выдать роли devs права на SELECT на любую таблицу  
>GRANT SELECT ON *.* TO devs WITH GRANT OPTION  
  
### 4. Выдать роль devs пользователю jhon  
>GRANT devs TO jhon  
  
### 5. Предоставить результаты SELECT из system-таблиц соответсвующих созданным сущностям  
>SELECT * FROM system.users WHERE name = 'jhon' \G  
  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ12/img/001.png)  
  
>SELECT * FROM system.roles WHERE name = 'devs' \G  
  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ12/img/002.png)  
  
>SELECT * FROM system.role_grants WHERE granted_role_name = 'devs' \G  
  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ12/img/003.png)  
  
>SELECT * FROM system.grants WHERE role_name = 'devs' \G  
  
![](https://github.com/oslavgorod/Clickhouse-2024/blob/main/DZ12/img/004.png)  
