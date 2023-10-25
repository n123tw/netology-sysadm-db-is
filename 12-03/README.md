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

## Задание 3

Вижу два решения. Первое, если считать "последней" аренду с наибольшим rental_id:

```
SELECT * from rental r 
ORDER BY rental_id DESC
LIMIT 5;
```
![Скриншот-3](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-03/img/3.jpg)

Второе, если считать "последней" аренду по rental_date:

```
SELECT *, CAST(rental_date AS DATE), CAST(rental_date AS TIME)
FROM rental r 
ORDER BY CAST(rental_date AS DATE) DESC, CAST(rental_date AS TIME) DESC
LIMIT 5;
```
![Скриншот-4](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-03/img/4.jpg)

## Задание 4

```
SELECT LOWER(last_name), REPLACE(LOWER(first_name), 'll', 'pp')
FROM customer c 
WHERE active = 1 AND first_name LIKE 'Kelly' OR first_name LIKE 'Willie';
```
![Скриншот-5](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-03/img/5.jpg)