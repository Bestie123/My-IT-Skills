
# HAVING в SQL

## Содержание ^h8k3m7p2x
- [Введение](#^d4f7g2k9n)
- [Что такое HAVING?](#^j5m8v3b6c)
- [Синтаксис HAVING](#^r9t2q5w8z)
  - [Базовый синтаксис](#^s4x6n8p3m)
  - [Порядок выполнения](#^y7c9k2v5b)
- [Примеры использования](#^e3m8p6r9t)
  - [HAVING с COUNT](#^a5b7d9f2h)
  - [HAVING с SUM](#^k4j8m3p6q)
  - [HAVING с AVG](#^w2t5v8y3c)
- [Отличие HAVING от WHERE](#^n6b9q3r7s)
- [Заключение](#^p8k2m5v9x)

## Введение ^d4f7g2k9n
HAVING — это важное ключевое слово в SQL, которое используется для фильтрации результатов группировки, созданной с помощью оператора GROUP BY. В отличие от WHERE, который фильтрует строки до группировки, HAVING фильтрует группы после их формирования.

<small>Этот документ предназначен для разработчиков и аналитиков, работающих с базами данных.</small>

[↑ К содержанию](#^h8k3m7p2x)

## Что такое HAVING? ^j5m8v3b6c
HAVING — это условие фильтрации, которое применяется к сгруппированным данным. Оно работает аналогично WHERE, но предназначено для агрегированных значений, а не для отдельных строк.

**Ключевые особенности:**
- Применяется после группировки данных (GROUP BY)
- Работает с агрегированными функциями (COUNT, SUM, AVG, MAX, MIN)
- Может использоваться только в SELECT-запросах с GROUP BY

[↑ К содержанию](#^h8k3m7p2x)

## Синтаксис HAVING ^r9t2q5w8z

### Базовый синтаксис ^s4x6n8p3m
```sql
SELECT column1, aggregate_function(column2)
FROM table_name
WHERE condition
GROUP BY column1
HAVING aggregate_function(column2) condition value;
```

### Порядок выполнения ^y7c9k2v5b
<small>SQL выполняет запросы в следующем порядке:</small>

```sql
-- 1. FROM/JOIN - выбор таблиц
-- 2. WHERE - фильтрация строк
-- 3. GROUP BY - группировка данных
-- 4. HAVING - фильтрация групп
-- 5. SELECT - выбор столбцов
-- 6. ORDER BY - сортировка результатов
```

[↑ К содержанию](#^h8k3m7p2x)

## Примеры использования ^e3m8p6r9t

### HAVING с COUNT ^a5b7d9f2h
```sql
-- Найти отделы с более чем 5 сотрудниками
SELECT department, COUNT(*) as employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

<small>В этом примере мы группируем сотрудников по отделам и оставляем только те отделы, где количество сотрудников превышает 5.</small>

### HAVING с SUM ^k4j8m3p6q
```sql
-- Найти клиентов с общей суммой заказов более 1000
SELECT customer_id, SUM(order_amount) as total_orders
FROM orders
GROUP BY customer_id
HAVING SUM(order_amount) > 1000;
```

### HAVING с AVG ^w2t5v8y3c
```sql
-- Найти категории товаров со средней ценой выше 50
SELECT category, AVG(price) as average_price
FROM products
GROUP BY category
HAVING AVG(price) > 50;
```

[↑ К содержанию](#^h8k3m7p2x)

## Отличие HAVING от WHERE ^n6b9q3r7s
<table>
<tr>
  <td><strong>Критерий</strong></td>
  <td><strong>WHERE</strong></td>
  <td><strong>HAVING</strong></td>
</tr>
<tr>
  <td>Время применения</td>
  <td>До группировки</td>
  <td>После группировки</td>
</tr>
<tr>
  <td>Работа с агрегатами</td>
  <td>Не может использовать агрегатные функции</td>
  <td>Может использовать агрегатные функции</td>
</tr>
<tr>
  <td>Уровень применения</td>
  <td>Фильтрация отдельных строк</td>
  <td>Фильтрация групп строк</td>
</tr>
<tr>
  <td>Обязательность GROUP BY</td>
  <td>Не требует GROUP BY</td>
  <td>Требует GROUP BY (в большинстве случаев)</td>
</tr>
</table>

```sql
-- Пример совместного использования WHERE и HAVING
SELECT department, AVG(salary) as avg_salary
FROM employees
WHERE hire_date > '2020-01-01'  -- фильтрация ДО группировки
GROUP BY department
HAVING AVG(salary) > 50000;     -- фильтрация ПОСЛЕ группировки
```

[↑ К содержанию](#^h8k3m7p2x)

## Заключение ^p8k2m5v9x
HAVING является мощным инструментом для работы с группированными данными в SQL. Он позволяет фильтровать результаты агрегации, что делает его незаменимым при аналитической обработке данных. Правильное понимание разницы между WHERE и HAVING — ключ к написанию эффективных SQL-запросов.

<big>Основной вывод: используйте WHERE для фильтрации строк, HAVING — для фильтрации групп.</big>

[↑ К содержанию](#^h8k3m7p2x)

**Теги:** #sql #having #базы-данных #group-by #агрегатные-функции #sql-фильтрация
```