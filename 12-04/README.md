# Домашнее задание к занятию «SQL. Часть 2» Лазарев Даниил
## Задание 1

```
SELECT CONCAT(staff.last_name, ' ', staff.first_name) as StaffSurnameName, city.city as StoreCity, TopStore.CustomerQuantity
FROM (
SELECT store_id, COUNT(customer_id) as CustomerQuantity
FROM customer c
GROUP BY store_id
HAVING CustomerQuantity > 300) AS TopStore
LEFT JOIN staff on staff.store_id = TopStore.store_id
LEFT JOIN store on store.store_id = TopStore.store_id
LEFT JOIN address on address.address_id = store.address_id 
LEFT JOIN city on city.city_id = address.city_id
```

![Скриншот-1](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-04/img/1.jpg)

## Задание 2

```
SELECT COUNT(1)
FROM film f 
WHERE length  > (SELECT AVG(`length`)
				FROM film f2);
```

![Скриншот-2](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-04/img/2.jpg)

## Задание 3

```
SELECT DATE_FORMAT(payment_date, '%M %Y') AS BestMonth, SUM(amount) AS MaxSum, COUNT(rental_id) as RentalQuantity
FROM payment p 
GROUP BY BestMonth
ORDER BY MaxSum DESC
LIMIT 1;
```

![Скриншот-3](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-04/img/3.jpg)