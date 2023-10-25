# Домашнее задание к занятию «SQL. Часть 1» Лазарев Даниил
## Задание 1

```
SELECT DISTINCT district
FROM address a
WHERE district	LIKE 'K%' AND district LIKE '%a' AND district NOT LIKE '% %';
```

![Скриншот-1](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-03/img/1.jpg)

## Задание 2

```
SELECT payment_id, payment_date, amount, CAST(payment_date AS DATE)
FROM payment p
WHERE amount > 10 AND CAST(payment_date AS DATE) BETWEEN '2005-06-15' AND '2005-06-18';
```

![Скриншот-2](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-03/img/2.jpg)