# Домашнее задание к занятию «Базы данных» Лазарев Даниил
## Задание 1

```SELECT user from mysql.user;```

![Скриншот-1](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-06/img/1.jpg)

```GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'localhost';```

```SHOW GRANTS FOR 'sys_temp'@'localhost';```

![Скриншот-2](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-06/img/2.jpg)

```docker exec -i netology-mysql sh -c 'exec mysql -usys_temp -p"password"' < /var/homework/sakila-db/sakila-schema.sql
docker exec -i netology-mysql sh -c 'exec mysql -usys_temp -p"password"' < /var/homework/sakila-db/sakila-data.sql```

![Скриншот-3](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-06/img/3.jpg)

