# Домашнее задание к занятию «Базы данных» Лазарев Даниил
## Задание 1
```
Сотрудники:
  Идентификатор: первичный ключ, serial, TINYINT
  Фамилия: VARCHAR(50)
  Имя: VARCHAR(50)
  Отчество: VARCHAR(50)
  Дата приема: DATE
  Идентификатор должности: внешний ключ, TINYINT
  Идентификатор проекта: внешний ключ, TINYINT
  Идентификатор структурного подразделения: внешний ключ, TINYINT
  Идентификатор филиала: внешний ключ, TINYINT
  Оклад: DECIMAL

Должности:
  Идентификатор: первичный ключ, serial, TINYINT
  Наименование: VARCHAR(50)

Структурные подразделения:
  Идентификатор: первичный ключ, serial, TINYINT
  Наименование: VARCHAR(50)
  Тип подразделения: VARCHAR(20)

Регионы:
  Идентификатор: первичный ключ, serial, TINYINT
  Наименование: VARCHAR(40)

Филиалы:
  Идентификатор: первичный ключ, serial, TINYINT
  Идентификатор региона: внешний ключ, TINYINT
  Адрес: VARCHAR(50)

Проекты:
  Идентификатор: первичный ключ, serial, TINYINT
  Наименование: TEXT

Сотрудники на проектах:
  Идентификатор проекта: внешний ключ, TINYINT
  Идентификатор сотрудника: внешний ключ, TINYINT
```