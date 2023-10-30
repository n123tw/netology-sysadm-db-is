# Домашнее задание к занятию «Индексы» Лазарев Даниил
## Задание 1

```
-- Пусть Q - процентное отношение общего размера всех индексов к общему размеру всех таблиц
SELECT CONCAT(ROUND(SUM(INDEX_LENGTH) / SUM(DATA_LENGTH) * 100, 1), '%') as Q
FROM INFORMATION_SCHEMA.TABLES
```
![Скриншот-1](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-05/img/1.jpg)

## Задание 2

Выполним запрос (200 строк), чтобы сравнить время выполнения с оптимизированным запросом
Время выполнения: 354 с

![Скриншот-2](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-05/img/2.jpg)

Выполним EXPLAIN ANALYZE, время выполнения: -. Я просто не дождался, пришлось остановить выполнение запроса на 2241 с.

![Скриншот-3](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-05/img/3.jpg)

Оптимизируем запрос:

1) Создадим индексы idx_payment_date (payment.payment_date), idx_customer_id (rental.customer_id);
2) Используем 2 JOINа вместо перечисления таблиц в FROM, исключаем избыточные таблицы inventory, film из FROM;
3) Используем группировку результата по customer_name;
4) Исключим оконную функцию, так как она избыточна;

```
CREATE INDEX idx_payment_date ON payment (payment_date);
CREATE INDEX idx_customer_id ON rental (customer_id);
SELECT CONCAT(c.last_name, ' ', c.first_name) AS customer_name, SUM(p.amount) AS total_payment
FROM payment p
JOIN rental r ON p.payment_date = r.rental_date
JOIN customer c ON r.customer_id = c.customer_id
WHERE DATE(p.payment_date) = '2005-07-30'
GROUP BY customer_name;
```



Первые 200 строк выполнения оптимизированного запроса: 659 мс, т.е. в 537 раз

![Скриншот-4](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-05/img/4.jpg)

EXPLAIN ANALYZE оптимизированного запроса (200 строк): 1.452 с

![Скриншот-5](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-05/img/5.jpg)


### Доработка

Кажется, сложилось впечатление, что индекс idx_payment_date не использовался в оптимизированном запросе из-за некорректных скриншотов, я обновил пути к скриншотам 4, 5.
На скриншоте 5 видно, что idx_payment_date используется:
```
Index lookup on p using idx_payment_date (payment_date=r.rental_date), with index condition: (cast(p.payment_date as date) = '2005-07-30')  (cost=0.25 rows=1) (actual time=0.00133..0.00134 rows=0.04 loops=16044)
```

Сравним EXPLAIN ANALYZE с разными WHERE:

```
WHERE DATE(p.payment_date) = '2005-07-30'

-> Limit: 200 row(s)  (actual time=32.8..32.8 rows=200 loops=1)
    -> Table scan on <temporary>  (actual time=32.7..32.8 rows=200 loops=1)
        -> Aggregate using temporary table  (actual time=32.7..32.7 rows=391 loops=1)
            -> Nested loop inner join  (cost=11268 rows=16010) (actual time=0.181..32.2 rows=642 loops=1)
                -> Nested loop inner join  (cost=5665 rows=16010) (actual time=0.0709..12.8 rows=16044 loops=1)
                    -> Table scan on c  (cost=61.2 rows=599) (actual time=0.0331..0.176 rows=599 loops=1)
                    -> Index lookup on r using idx_fk_customer_id (customer_id=c.customer_id)  (cost=6.69 rows=26.7) (actual time=0.0168..0.0202 rows=26.8 loops=599)
                -> Index lookup on p using idx_payment_date (payment_date=r.rental_date), with index condition: (cast(p.payment_date as date) = '2005-07-30')  (cost=0.25 rows=1) (actual time=0.0011..0.00112 rows=0.04 loops=16044)
```

![Скриншот-6](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-05/img/6.jpg)

```
WHERE payment_date >= '2005-07-30' and payment_date < DATE_ADD('2005-07-30', INTERVAL 1 DAY)

-> Limit: 200 row(s)  (actual time=2.5..2.52 rows=200 loops=1)
    -> Table scan on <temporary>  (actual time=2.5..2.52 rows=200 loops=1)
        -> Aggregate using temporary table  (actual time=2.49..2.49 rows=391 loops=1)
            -> Nested loop inner join  (cost=573 rows=634) (actual time=0.0299..2.01 rows=642 loops=1)
                -> Nested loop inner join  (cost=351 rows=634) (actual time=0.0218..0.75 rows=634 loops=1)
                    -> Filter: ((r.rental_date >= TIMESTAMP'2005-07-30 00:00:00') and (r.rental_date < <cache>(('2005-07-30' + interval 1 day))))  (cost=129 rows=634) (actual time=0.0145..0.236 rows=634 loops=1)
                        -> Covering index range scan on r using rental_date over ('2005-07-30 00:00:00' <= rental_date < '2005-07-31 00:00:00')  (cost=129 rows=634) (actual time=0.0128..0.155 rows=634 loops=1)
                    -> Single-row index lookup on c using PRIMARY (customer_id=r.customer_id)  (cost=0.25 rows=1) (actual time=706e-6..721e-6 rows=1 loops=634)
                -> Index lookup on p using idx_payment_date (payment_date=r.rental_date)  (cost=0.25 rows=1) (actual time=0.00158..0.00186 rows=1.01 loops=634)
```

![Скриншот-7](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-05/img/7.jpg)

Видим, что общее время выполнение запроса при применении более сложного WHERE уменьшается с 32.8 до ~2.51 мс