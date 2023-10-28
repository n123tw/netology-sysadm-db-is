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

![Скриншот-4](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-05/img/2.jpg)

EXPLAIN ANALYZE оптимизированного запроса (200 строк): 1.452 с

![Скриншот-5](https://github.com/n123tw/netology-sysadm-db-is/blob/main/12-05/img/2.jpg)


