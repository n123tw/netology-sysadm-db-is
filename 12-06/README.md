# Домашнее задание к занятию «Базы данных» Лазарев Даниил
## Задание 1

```SELECT user from mysql.user;```

![Скриншот-1](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-06/img/1.jpg)

```GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'localhost';```

```SHOW GRANTS FOR 'sys_temp'@'localhost';```

![Скриншот-2](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-06/img/2.jpg)

```
docker exec -i netology-mysql sh -c 'exec mysql -usys_temp -p"password"' < /var/homework/sakila-db/sakila-schema.sql
docker exec -i netology-mysql sh -c 'exec mysql -usys_temp -p"password"' < /var/homework/sakila-db/sakila-data.sql
```

![Скриншот-3](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-06/img/3.jpg)

## Задание 2

```
SELECT TABLE_NAME, COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'sakila' AND COLUMN_KEY = 'PRI' ORDER BY table_name;
```

```
+---------------+--------------+
| TABLE_NAME    | COLUMN_NAME  |
+---------------+--------------+
| actor         | actor_id     |
| address       | address_id   |
| category      | category_id  |
| city          | city_id      |
| country       | country_id   |
| customer      | customer_id  |
| film          | film_id      |
| film_actor    | actor_id     |
| film_actor    | film_id      |
| film_category | category_id  |
| film_category | film_id      |
| film_text     | film_id      |
| inventory     | inventory_id |
| language      | language_id  |
| payment       | payment_id   |
| rental        | rental_id    |
| staff         | staff_id     |
| store         | store_id     |
+---------------+--------------+
18 rows in set (0.01 sec)
```