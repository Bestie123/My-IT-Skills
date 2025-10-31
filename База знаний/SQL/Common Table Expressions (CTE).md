
# Common Table Expressions (CTE) в SQL

## Содержание ^k9p2r4m7x
- [Введение](#^a2b4c6d8)
- [Что такое CTE?](#^e1f3g5h7)
- [Синтаксис CTE](#^i9j1k3l5)
- [Типы CTE](#^m7n9o1p3)
  - [Рекурсивные CTE](#^q5r7s9t1)
  - [Нерекурсивные CTE](#^u3v5w7x9)
- [Преимущества использования CTE](#^y1z2a3b4)
- [Примеры использования](#^c5d6e7f8)
- [Ограничения CTE](#^g9h0i1j2)
- [Заключение](#^k1l2m3n4)

## Введение ^a2b4c6d8
Common Table Expressions (CTE) — это мощный инструмент в SQL, который позволяет создавать временные именованные результирующие наборы, доступные в пределах выполнения одного SQL-запроса. CTE значительно улучшают читаемость и поддерживаемость сложных SQL-запросов, особенно тех, которые содержат множественные подзапросы или рекурсивные структуры.

<small>CTE были введены в стандарт SQL:1999 и сейчас поддерживаются большинством современных СУБД.</small>

[↑ К содержанию](#^k9p2r4m7x)

## Что такое CTE? ^e1f3g5h7
CTE (Common Table Expression) — это временный результирующий набор, который можно ссылаться в пределах оператора SELECT, INSERT, UPDATE или DELETE. Основные характеристики CTE:

- Существует только во время выполнения запроса
- Может ссылаться на саму себя (рекурсивные CTE)
- Улучшает читаемость сложных запросов
- Может использоваться многократно в основном запросе

<big>**Ключевые особенности:**</big>

- CTE определяется с помощью ключевого слова WITH
- Имеет область видимости в пределах одного запроса
- Может заменять временные таблицы в многих сценариях
- Особенно полезен для рекурсивных запросов

[↑ К содержанию](#^k9p2r4m7x)

## Синтаксис CTE ^i9j1k3l5
Базовый синтаксис для создания CTE:

```sql
WITH cte_name (column1, column2, ...) AS (
    -- Подзапрос, определяющий CTE
    SELECT column1, column2, ...
    FROM table_name
    WHERE conditions
)
-- Основной запрос, использующий CTE
SELECT *
FROM cte_name;
```

### Расширенный синтаксис для нескольких CTE:
```sql
WITH 
cte1 AS (
    SELECT ... FROM ...
),
cte2 AS (
    SELECT ... FROM cte1 ...
)
SELECT ...
FROM cte2
JOIN cte1 ON ...;
```

<small>Имена столбцов в CTE могут быть указаны явно или унаследованы из подзапроса.</small>

[↑ К содержанию](#^k9p2r4m7x)

## Типы CTE ^m7n9o1p3
CTE делятся на два основных типа в зависимости от возможности ссылаться на самих себя.

### Рекурсивные CTE ^q5r7s9t1
Рекурсивные CTE могут ссылаться на самих себя, что позволяет обрабатывать иерархические данные.

```sql
WITH RECURSIVE hierarchy_cte AS (
    -- Якорная часть (начало рекурсии)
    SELECT id, name, parent_id, 1 as level
    FROM categories
    WHERE parent_id IS NULL
    
    UNION ALL
    
    -- Рекурсивная часть
    SELECT c.id, c.name, c.parent_id, h.level + 1
    FROM categories c
    INNER JOIN hierarchy_cte h ON c.parent_id = h.id
)
SELECT * FROM hierarchy_cte;
```

<small>Рекурсивные CTE требуют ключевого слова RECURSIVE и состоят из якорной и рекурсивной частей.</small>

[↑ К содержанию](#^k9p2r4m7x)

### Нерекурсивные CTE ^u3v5w7x9
Обычные CTE, которые не ссылаются на самих себя, используются для упрощения сложных запросов.

```sql
WITH sales_summary AS (
    SELECT 
        product_id,
        SUM(quantity) as total_quantity,
        AVG(price) as average_price
    FROM sales
    GROUP BY product_id
)
SELECT 
    p.product_name,
    s.total_quantity,
    s.average_price
FROM products p
JOIN sales_summary s ON p.product_id = s.product_id;
```

<small>Нерекурсивные CTE наиболее распространены и используются для улучшения читаемости запросов.</small>

[↑ К содержанию](#^k9p2r4m7x)

## Преимущества использования CTE ^y1z2a3b4
Использование CTE предоставляет несколько значительных преимуществ:

<table>
<tr>
<td><strong>Преимущество</strong></td>
<td><strong>Описание</strong></td>
<td><strong>Пример использования</strong></td>
</tr>
<tr>
<td>Улучшенная читаемость</td>
<td>Разбивает сложные запросы на логические блоки</td>
<td>Заменяет вложенные подзапросы именованными CTE</td>
</tr>
<tr>
<td>Повторное использование</td>
<td>CTE можно ссылаться многократно в основном запросе</td>
<td>Один CTE используется в нескольких JOIN</td>
</tr>
<tr>
<td>Рекурсивные возможности</td>
<td>Обработка иерархических данных</td>
<td>Организационные структуры, деревья категорий</td>
</tr>
<tr>
<td>Обход ограничений</td>
<td>Решение проблем, где подзапросы не работают</td>
<td>Оконные функции в CTE</td>
</tr>
</table>

[↑ К содержанию](#^k9p2r4m7x)

## Примеры использования ^c5d6e7f8
### Пример 1: Иерархия сотрудников (рекурсивный CTE)
```sql
WITH RECURSIVE employee_hierarchy AS (
    -- Базовая часть: начальник отдела
    SELECT 
        employee_id,
        name,
        manager_id,
        0 as level,
        CAST(name AS VARCHAR(1000)) as path
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Рекурсивная часть: подчиненные
    SELECT 
        e.employee_id,
        e.name,
        e.manager_id,
        eh.level + 1,
        CAST(eh.path || ' -> ' || e.name AS VARCHAR(1000))
    FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.employee_id
)
SELECT 
    employee_id,
    name,
    level,
    path
FROM employee_hierarchy
ORDER BY path;
```

### Пример 2: Анализ продаж с несколькими CTE
```sql
WITH 
monthly_sales AS (
    SELECT 
        product_id,
        EXTRACT(MONTH FROM sale_date) as month,
        SUM(amount) as total_sales
    FROM sales
    WHERE EXTRACT(YEAR FROM sale_date) = 2024
    GROUP BY product_id, EXTRACT(MONTH FROM sale_date)
),
product_ranking AS (
    SELECT 
        product_id,
        month,
        total_sales,
        RANK() OVER (PARTITION BY month ORDER BY total_sales DESC) as rank
    FROM monthly_sales
)
SELECT 
    p.product_name,
    pr.month,
    pr.total_sales,
    pr.rank
FROM product_ranking pr
JOIN products p ON pr.product_id = p.product_id
WHERE pr.rank <= 3
ORDER BY pr.month, pr.rank;
```

### Пример 3: Пошаговая агрегация данных
```sql
WITH 
raw_data AS (
    SELECT user_id, order_date, amount
    FROM orders
    WHERE order_date >= '2024-01-01'
),
user_totals AS (
    SELECT 
        user_id,
        COUNT(*) as order_count,
        SUM(amount) as total_amount
    FROM raw_data
    GROUP BY user_id
),
user_segments AS (
    SELECT 
        user_id,
        order_count,
        total_amount,
        CASE 
            WHEN total_amount > 1000 THEN 'VIP'
            WHEN total_amount > 500 THEN 'Loyal'
            ELSE 'Regular'
        END as segment
    FROM user_totals
)
SELECT 
    segment,
    COUNT(*) as user_count,
    AVG(total_amount) as avg_spent
FROM user_segments
GROUP BY segment;
```

[↑ К содержанию](#^k9p2r4m7x)

## Ограничения CTE ^g9h0i1j2
Несмотря на мощь CTE, существуют определенные ограничения:

- **Производительность**: CTE не всегда материализуются и могут выполняться многократно
- **Сложность отладки**: Ошибки в рекурсивных CTE сложно диагностировать
- **Ограничения рекурсии**: Максимальная глубина рекурсии ограничена (обычно 100-1000 уровней)
- **Поддержка СУБД**: Синтаксис и возможности могут различаться между разными СУБД

```sql
-- Пример установки максимальной глубины рекурсии в SQL Server
OPTION (MAXRECURSION 1000)
```

<small>Всегда тестируйте производительность CTE на реальных данных.</small>

[↑ К содержанию](#^k9p2r4m7x)

## Заключение ^k1l2m3n4
CTE являются неотъемлемой частью современного SQL, предоставляя мощные возможности для структурирования сложных запросов. Основные преимущества:

- Значительное улучшение читаемости и поддерживаемости кода
- Возможность обработки иерархических данных через рекурсию
- Гибкость в построении сложных аналитических запросов
- Сокращение дублирования кода через многократное использование

<big>**Рекомендации по использованию:**</big>
- Используйте CTE для замены сложных вложенных подзапросов
- Применяйте рекурсивные CTE для работы с древовидными структурами
- Тестируйте производительность на больших объемах данных
- Соблюдайте ограничения глубины рекурсии для вашей СУБД

[↑ К содержанию](#^k9p2r4m7x)

**Теги:** #sql #cte #common-table-expressions #рекурсивные-запросы #базы-данных
```