
# GROUP BY в SQL

## Содержание ^c8g7r2p5m
- [Введение](#^i9k8l7j6h)
- [Синтаксис GROUP BY](#^s5t4r3e2w)
- [Примеры использования](#^e1q2w3e4r)
  - [Простой пример с одной колонкой](#^p7o8i9u0y)
  - [GROUP BY с несколькими колонками](#^m2n3b4v5c)
  - [GROUP BY с агрегатными функциями](#^a6x7z8q9w)
- [Агрегатные функции](#^f0r1t2y3u)
- [Фильтрация групп с HAVING](#^h4j5k6l7m)
- [Отличие WHERE и HAVING](#^d8e9f0g1h)
- [Заключение](#^c3e4r5t6y)

## Введение ^i9k8l7j6h

Оператор `GROUP BY` в SQL используется для группировки строк с одинаковыми значениями в указанных столбцах. Это один из наиболее важных операторов для анализа данных, который позволяет выполнять агрегатные вычисления над группами записей.

`GROUP BY` часто используется вместе с агрегатными функциями (`COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`) для получения сводной информации по группам.

[↑ К содержанию](#^c8g7r2p5m)

## Синтаксис GROUP BY ^s5t4r3e2w

Базовый синтаксис оператора `GROUP BY`:

```sql
SELECT column1, column2, aggregate_function(column3)
FROM table_name
WHERE condition
GROUP BY column1, column2
ORDER BY column1;
```

**Компоненты:**
- `SELECT` - список колонок для вывода (должны быть либо в `GROUP BY`, либо агрегатные функции)
- `FROM` - указание таблицы
- `WHERE` - фильтрация строк ДО группировки (необязательно)
- `GROUP BY` - список колонок для группировки
- `ORDER BY` - сортировка результатов (необязательно)

<small>Примечание: В стандартном SQL все неагрегированные колонки в SELECT должны присутствовать в GROUP BY.</small>

[↑ К содержанию](#^c8g7r2p5m)

## Примеры использования ^e1q2w3e4r

### Простой пример с одной колонкой ^p7o8i9u0y

Группировка по одному столбцу с подсчетом количества записей в каждой группе:

```sql
SELECT department, COUNT(*) as employee_count
FROM employees
GROUP BY department;
```

**Результат:**
```
| department | employee_count |
|------------|----------------|
| IT         | 15             |
| HR         | 8              |
| Sales      | 22             |
| Marketing  | 12             |
```

<small>Этот запрос показывает количество сотрудников в каждом отделе.</small>

[↑ К содержанию](#^c8g7r2p5m)

### GROUP BY с несколькими колонками ^m2n3b4v5c

Группировка по нескольким столбцам позволяет создавать более детальные группы:

```sql
SELECT department, job_title, COUNT(*) as count
FROM employees
GROUP BY department, job_title;
```

**Результат:**
```
| department | job_title     | count |
|------------|---------------|-------|
| IT         | Developer     | 10    |
| IT         | Manager       | 2     |
| IT         | Analyst       | 3     |
| HR         | Recruiter     | 5     |
| HR         | Manager       | 2     |
| HR         | Specialist    | 1     |
```

<small>Здесь мы видим распределение должностей внутри каждого отдела.</small>

[↑ К содержанию](#^c8g7r2p5m)

### GROUP BY с агрегатными функциями ^a6x7z8q9w

Использование различных агрегатных функций в одной группировке:

```sql
SELECT 
    department,
    COUNT(*) as total_employees,
    AVG(salary) as avg_salary,
    MAX(salary) as max_salary,
    MIN(salary) as min_salary,
    SUM(salary) as total_salary_budget
FROM employees
GROUP BY department;
```

<small>Этот запрос предоставляет comprehensive статистику по зарплатам в каждом отделе.</small>

[↑ К содержанию](#^c8g7r2p5m)

## Агрегатные функции ^f0r1t2y3u

Оператор `GROUP BY` работает в связке с агрегатными функциями. Основные агрегатные функции в SQL:

<table>
<tr>
    <td><strong>Функция</strong></td>
    <td><strong>Описание</strong></td>
    <td><strong>Пример</strong></td>
</tr>
<tr>
    <td><code>COUNT()</code></td>
    <td>Подсчет количества записей</td>
    <td><code>COUNT(*)</code> - все записи</td>
</tr>
<tr>
    <td><code>SUM()</code></td>
    <td>Сумма значений столбца</td>
    <td><code>SUM(salary)</code></td>
</tr>
<tr>
    <td><code>AVG()</code></td>
    <td>Среднее значение</td>
    <td><code>AVG(age)</code></td>
</tr>
<tr>
    <td><code>MAX()</code></td>
    <td>Максимальное значение</td>
    <td><code>MAX(price)</code></td>
</tr>
<tr>
    <td><code>MIN()</code></td>
    <td>Минимальное значение</td>
    <td><code>MIN(date)</code></td>
</tr>
</table>

```sql
-- Примеры использования агрегатных функций
SELECT 
    category,
    COUNT(*) as product_count,
    AVG(price) as average_price,
    MAX(price) as most_expensive,
    MIN(price) as cheapest_product
FROM products
GROUP BY category;
```

[↑ К содержанию](#^c8g7r2p5m)

## Фильтрация групп с HAVING ^h4j5k6l7m

Оператор `HAVING` используется для фильтрации групп после выполнения `GROUP BY`. В отличие от `WHERE`, который фильтрует строки до группировки, `HAVING` фильтрует уже сформированные группы.

```sql
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

**Результат:**
```
| department | avg_salary |
|------------|------------|
| IT         | 75000      |
| Finance    | 65000      |
```

<small>Этот запрос показывает только те отделы, где средняя зарплата превышает 50,000.</small>

```sql
-- Более сложный пример с несколькими условиями
SELECT 
    department, 
    COUNT(*) as employee_count,
    AVG(salary) as avg_salary
FROM employees
WHERE hire_date > '2020-01-01'
GROUP BY department
HAVING COUNT(*) > 5 AND AVG(salary) < 80000
ORDER BY avg_salary DESC;
```

[↑ К содержанию](#^c8g7r2p5m)

## Отличие WHERE и HAVING ^d8e9f0g1h

<table>
<tr>
    <td><strong>Аспект</strong></td>
    <td><strong>WHERE</strong></td>
    <td><strong>HAVING</strong></td>
</tr>
<tr>
    <td>Время выполнения</td>
    <td>До группировки</td>
    <td>После группировки</td>
</tr>
<tr>
    <td>Объект фильтрации</td>
    <td>Отдельные строки</td>
    <td>Группы строк</td>
</tr>
<tr>
    <td>Использование с агрегатами</td>
    <td>Не может использовать агрегатные функции</td>
    <td>Может использовать агрегатные функции</td>
</tr>
<tr>
    <td>Обязательность</td>
    <td>Необязателен</td>
    <td>Необязателен</td>
</tr>
</table>

**Пример, демонстрирующий разницу:**

```sql
-- WHERE фильтрует строки ДО группировки
SELECT department, COUNT(*) as count
FROM employees
WHERE salary > 50000          -- Фильтр отдельных сотрудников
GROUP BY department;

-- HAVING фильтрует группы ПОСЛЕ группировки
SELECT department, COUNT(*) as count
FROM employees
GROUP BY department
HAVING COUNT(*) > 10;         -- Фильтр групп отделов
```

<small>Можно использовать оба оператора вместе в одном запросе.</small>

[↑ К содержанию](#^c8g7r2p5m)

## Заключение ^c3e4r5t6y

Оператор `GROUP BY` является фундаментальным инструментом для анализа данных в SQL. Он позволяет:

- Группировать данные по одному или нескольким столбцам
- Выполнять агрегатные вычисления над группами
- Получать сводную статистику
- Фильтровать группы с помощью `HAVING`

Правильное использование `GROUP BY` в сочетании с агрегатными функциями и оператором `HAVING` позволяет решать сложные аналитические задачи и получать ценные insights из данных.

<big>Важно помнить:</big> Все неагрегированные колонки в `SELECT` должны присутствовать в `GROUP BY`, иначе возникнет ошибка.

[↑ К содержанию](#^c8g7r2p5m)

**Теги:** #sql #group-by #агрегатные-функции #having #база-данных #анализ-данных
```