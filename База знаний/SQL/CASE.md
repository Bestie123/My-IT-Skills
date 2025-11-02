
# Выражение CASE в SQL

## Содержание ^k9p2r4m7x
- [Введение](#^a1b2c3d4e5)
- [Что такое выражение CASE?](#^f6g7h8i9j0)
- [Синтаксис выражения CASE](#^k1l2m3n4o5)
- [Типы выражений CASE](#^p6q7r8s9t0)
  - [Простое выражение CASE](#^u1v2w3x4y5)
  - [Поисковое выражение CASE](#^z6a7b8c9d0)
- [Использование CASE в различных контекстах](#^e1f2g3h4i5)
  - [CASE в SELECT](#^j6k7l8m9n0)
  - [CASE в WHERE](#^o1p2q3r4s5)
  - [CASE в ORDER BY](#^t6u7v8w9x0)
  - [CASE в UPDATE](#^y1z2a3b4c5)
  - [CASE в агрегатных функциях](#^d6e7f8g9h0)
  - [CASE в HAVING](#^i1j2k3l4m5)
- [Вложенные CASE выражения](#^n6o7p8q9r0)
- [Практические примеры](#^s1t2u3v4w5)
- [Лучшие практики](#^x3y4z5a6b7)
- [Заключение](#^c8d9e0f1g2)

## Введение ^a1b2c3d4e5
Выражение CASE - это одно из самых мощных и универсальных средств в SQL, позволяющее реализовать условную логику непосредственно в запросах. Оно предоставляет возможность выполнять различные действия в зависимости от условий, аналогично операторам `if-else` или `switch` в традиционных языках программирования.

<big>**Основные возможности выражения CASE:**</big>

- Условное преобразование данных
- Динамическое вычисление значений
- Сложная фильтрация и сортировка
- Категоризация и группировка данных
- Обработка NULL значений

<small>Выражение CASE является частью стандарта SQL и поддерживается всеми современными реляционными СУБД.</small>

[↑ К содержанию](#^k9p2r4m7x)

## Что такое выражение CASE? ^f6g7h8i9j0
Выражение CASE - это конструкция SQL, которая позволяет выполнять условную логику, оценивая одно или несколько условий и возвращая соответствующее значение. Оно работает по принципу "если-то-иначе", позволяя создавать сложные ветвления непосредственно в SQL-запросах.

**Ключевые характеристики CASE:**

- Возвращает одно значение на основе условий
- Может использоваться в любом месте, где допускается выражение
- Поддерживает множественные условия
- Всегда должно заканчиваться ключевым словом END
- Может иметь необязательное условие ELSE

**Базовый принцип работы:**
```sql
CASE
    WHEN условие1 THEN результат1
    WHEN условие2 THEN результат2
    ...
    ELSE результат_по_умолчанию
END
```

**Разбор принципа работы:**
- Выражение оценивает условия последовательно сверху вниз
- При выполнении первого истинного условия возвращается соответствующий результат
- Если ни одно условие не выполняется, возвращается результат из ELSE
- Если ELSE отсутствует и ни одно условие не истинно, возвращается NULL

```sql
-- Простой пример
SELECT 
    product_name,
    price,
    CASE 
        WHEN price > 1000 THEN 'Дорогой'
        WHEN price > 500 THEN 'Средний'
        ELSE 'Бюджетный'
    END as price_category
FROM products;
```

[↑ К содержанию](#^k9p2r4m7x)

## Синтаксис выражения CASE ^k1l2m3n4o5
Выражение CASE имеет два основных варианта синтаксиса: простой и поисковый. Оба варианта следуют определенным правилам структуры.

### Общий синтаксис:
```sql
-- Поисковый синтаксис (более гибкий)
CASE
    WHEN условие1 THEN результат1
    WHEN условие2 THEN результат2
    ...
    [ELSE результат_по_умолчанию]
END

-- Простой синтаксис (для сравнения с одним выражением)
CASE выражение
    WHEN значение1 THEN результат1
    WHEN значение2 THEN результат2
    ...
    [ELSE результат_по_умолчанию]
END
```

**Разбор компонентов синтаксиса:**
- `CASE` - начало выражения
- `WHEN` - условие для проверки
- `THEN` - результат, возвращаемый при истинности условия
- `ELSE` - необязательный результат по умолчанию
- `END` - обязательное завершение выражения

### Правила использования:
```sql
-- Все ветки THEN должны возвращать совместимые типы данных
SELECT 
    CASE 
        WHEN condition1 THEN 100          -- INTEGER
        WHEN condition2 THEN 'Текст'      -- TEXT
        ELSE 0                            -- INTEGER
    END as mixed_types; -- Может вызвать ошибку в некоторых СУБД

-- Правильно: согласованные типы
SELECT 
    CASE 
        WHEN condition1 THEN 'Высокий'
        WHEN condition2 THEN 'Средний'
        ELSE 'Низкий'
    END as consistent_types; -- Все ветки возвращают TEXT
```

**Особенности выполнения:**
- Условия оцениваются последовательно сверху вниз
- Выполнение прекращается при первом истинном условии
- Рекомендуется располагать более специфичные условия выше
- ELSE рекомендуется всегда указывать явно

[↑ К содержанию](#^k9p2r4m7x)

## Типы выражений CASE ^p6q7r8s9t0
### Простое выражение CASE ^u1v2w3x4y5
Простое выражение CASE используется для сравнения одного выражения с несколькими возможными значениями. Это аналог оператора `switch` в других языках программирования.

**Синтаксис:**
```sql
CASE выражение_для_сравнения
    WHEN значение1 THEN результат1
    WHEN значение2 THEN результат2
    ...
    [ELSE результат_по_умолчанию]
END
```

**Примеры простого CASE:**
```sql
-- Категоризация по коду статуса
SELECT 
    order_id,
    status_code,
    CASE status_code
        WHEN 1 THEN 'Новый'
        WHEN 2 THEN 'В обработке'
        WHEN 3 THEN 'Выполнен'
        WHEN 4 THEN 'Отменен'
        ELSE 'Неизвестный статус'
    END as status_description
FROM orders;

-- Определение дня недели по номеру
SELECT 
    event_date,
    CASE EXTRACT(DOW FROM event_date)
        WHEN 0 THEN 'Воскресенье'
        WHEN 1 THEN 'Понедельник'
        WHEN 2 THEN 'Вторник'
        WHEN 3 THEN 'Среда'
        WHEN 4 THEN 'Четверг'
        WHEN 5 THEN 'Пятница'
        WHEN 6 THEN 'Суббота'
    END as day_of_week
FROM events;
```

**Разбор простого CASE:**
- Сравнивает одно выражение с несколькими значениями
- Более лаконичен когда нужно сравнить с конкретными значениями
- Менее гибок чем поисковый CASE
- Подходит для замены множественных OR условий

**Ограничения простого CASE:**
```sql
-- Нельзя использовать сложные условия
-- Это НЕ сработает:
CASE 
    WHEN price > 100 AND quantity < 10 THEN 'Высокий спрос'
    ...
END

-- Вместо этого используйте поисковый CASE
CASE
    WHEN price > 100 AND quantity < 10 THEN 'Высокий спрос'
    ...
END
```

[↑ К содержанию](#^k9p2r4m7x)

### Поисковое выражение CASE ^z6a7b8c9d0
Поисковое выражение CASE позволяет использовать сложные условия с различными выражениями и операторами сравнения. Это наиболее гибкая и часто используемая форма.

**Синтаксис:**
```sql
CASE
    WHEN условие1 THEN результат1
    WHEN условие2 THEN результат2
    ...
    [ELSE результат_по_умолчанию]
END
```

**Примеры поискового CASE:**
```sql
-- Категоризация по диапазонам значений
SELECT 
    employee_id,
    salary,
    CASE
        WHEN salary < 30000 THEN 'Низкий'
        WHEN salary BETWEEN 30000 AND 60000 THEN 'Средний'
        WHEN salary > 60000 THEN 'Высокий'
        ELSE 'Не указан'
    END as salary_level
FROM employees;

-- Сложные условия с несколькими столбцами
SELECT 
    product_id,
    price,
    units_in_stock,
    CASE
        WHEN price > 100 AND units_in_stock < 10 THEN 'Дорогой, мало на складе'
        WHEN price > 100 AND units_in_stock >= 10 THEN 'Дорогой, достаточно на складе'
        WHEN price <= 100 AND units_in_stock < 20 THEN 'Бюджетный, мало на складе'
        ELSE 'Бюджетный, достаточно на складе'
    END as product_status
FROM products;
```

**Разбор поискового CASE:**
- Поддерживает любые условия (сравнения, BETWEEN, IN, LIKE, IS NULL и т.д.)
- Может использовать разные столбцы в разных условиях
- Обеспечивает максимальную гибкость
- Рекомендуется для большинства сценариев

**Особенности условий:**
```sql
-- Использование различных операторов
SELECT 
    customer_id,
    total_orders,
    last_order_date,
    CASE
        WHEN total_orders = 0 THEN 'Новый клиент'
        WHEN total_orders > 10 THEN 'Постоянный клиент'
        WHEN last_order_date < CURRENT_DATE - INTERVAL '6 months' THEN 'Неактивный'
        WHEN email LIKE '%@gmail.com%' THEN 'Gmail пользователь'
        WHEN phone IS NULL THEN 'Телефон не указан'
        ELSE 'Обычный клиент'
    END as customer_segment
FROM customers;
```

[↑ К содержанию](#^k9p2r4m7x)

## Использование CASE в различных контекстах ^e1f2g3h4i5
### CASE в SELECT ^j6k7l8m9n0
Наиболее частое использование CASE - в списке SELECT для вычисления значений на лету.

**Примеры в SELECT:**
```sql
-- Категоризация данных при выборке
SELECT 
    order_id,
    customer_id,
    total_amount,
    CASE
        WHEN total_amount > 1000 THEN 'Крупный заказ'
        WHEN total_amount > 500 THEN 'Средний заказ'
        ELSE 'Мелкий заказ'
    END as order_size,
    -- Несколько CASE выражений в одном запросе
    CASE
        WHEN EXTRACT(HOUR FROM order_time) < 12 THEN 'Утренний'
        WHEN EXTRACT(HOUR FROM order_time) < 18 THEN 'Дневной'
        ELSE 'Вечерний'
    END as order_time_period
FROM orders;

-- Динамические вычисления
SELECT 
    product_id,
    price,
    discount,
    price * quantity as subtotal,
    CASE
        WHEN discount > 0 THEN price * quantity * (1 - discount)
        ELSE price * quantity
    END as final_price
FROM order_items;
```

**Разбор использования в SELECT:**
- Позволяет создавать вычисляемые столбцы на основе условий
- Может использоваться несколько раз в одном SELECT
- Результаты могут быть использованы в GROUP BY, ORDER BY
- Не влияет на производительность фильтрации

[↑ К содержанию](#^k9p2r4m7x)

### CASE в WHERE ^o1p2q3r4s5
Использование CASE в условиях WHERE для создания динамических условий фильтрации.

**Примеры в WHERE:**
```sql
-- Динамическая фильтрация в зависимости от параметров
SELECT *
FROM products
WHERE 
    category_id = CASE 
        WHEN @user_type = 'VIP' THEN 1  -- Показываем премиум категории VIP
        ELSE 2                          -- Обычные категории для остальных
    END;

-- Сложные условия с CASE
SELECT *
FROM employees
WHERE 
    salary > CASE 
        WHEN department_id = 1 THEN 50000  -- Высокие требования для отдела 1
        WHEN department_id = 2 THEN 40000  -- Средние для отдела 2
        ELSE 30000                         -- Базовые для остальных
    END;

-- Фильтрация с учетом приоритетов
SELECT *
FROM tasks
WHERE 
    status = 'active'
    AND priority = CASE 
        WHEN @urgent_mode = true THEN 'high'
        ELSE priority  -- Без изменения условия
    END;
```

**Разбор использования в WHERE:**
- Позволяет создавать динамические условия фильтрации
- Полезно когда условия зависят от других столбцов или параметров
- Может быть менее эффективно чем статические условия
- Следует использовать с осторожностью для больших таблиц

[↑ К содержанию](#^k9p2r4m7x)

### CASE в ORDER BY ^t6u7v8w9x0
Использование CASE для создания сложных и динамических условий сортировки.

**Примеры в ORDER BY:**
```sql
-- Приоритетная сортировка по статусу
SELECT 
    task_id,
    task_name,
    status,
    due_date
FROM tasks
ORDER BY 
    CASE status
        WHEN 'urgent' THEN 1
        WHEN 'high' THEN 2
        WHEN 'normal' THEN 3
        WHEN 'low' THEN 4
        ELSE 5
    END,
    due_date ASC;

-- Сортировка по разным столбцам в зависимости от условия
SELECT 
    product_id,
    product_name,
    price,
    units_in_stock
FROM products
ORDER BY 
    CASE 
        WHEN @sort_by = 'price' THEN price
        WHEN @sort_by = 'name' THEN NULL  -- Для строковой сортировки нужен другой подход
        ELSE product_id
    END ASC,
    -- Дополнительная сортировка по имени когда нужно
    CASE WHEN @sort_by = 'name' THEN product_name END ASC;

-- Сложная сортировка с направлениями
SELECT 
    employee_id,
    name,
    department_id,
    salary
FROM employees
ORDER BY 
    department_id ASC,
    CASE 
        WHEN @salary_order = 'desc' THEN salary 
    END DESC,
    CASE 
        WHEN @salary_order = 'asc' THEN salary 
    END ASC;
```

**Разбор использования в ORDER BY:**
- Позволяет создавать пользовательские порядки сортировки
- Может использоваться для реализации сложной бизнес-логики сортировки
- Полезно для приоритизации определенных категорий данных
- Может влиять на производительность на больших наборах данных

[↑ К содержанию](#^k9p2r4m7x)

### CASE в UPDATE ^y1z2a3b4c5
Использование CASE в операторах UPDATE для условного обновления данных.

**Примеры в UPDATE:**
```sql
-- Условное обновление на основе текущих значений
UPDATE employees
SET 
    salary = CASE
        WHEN performance_rating = 'excellent' THEN salary * 1.15
        WHEN performance_rating = 'good' THEN salary * 1.10
        WHEN performance_rating = 'average' THEN salary * 1.05
        ELSE salary  -- Без изменений
    END,
    last_review_date = CURRENT_DATE
WHERE department_id = 1;

-- Массовое обновление с различными условиями
UPDATE products
SET 
    price = CASE
        WHEN category_id = 1 THEN price * 1.20  -- Увеличить на 20%
        WHEN category_id = 2 THEN price * 0.90  -- Уменьшить на 10%
        ELSE price * 1.05                       -- Увеличить на 5%
    END,
    last_price_change = CURRENT_DATE;

-- Обновление с проверкой ограничений
UPDATE inventory
SET 
    reorder_level = CASE
        WHEN units_sold > 100 THEN 50
        WHEN units_sold > 50 THEN 30
        ELSE 20
    END,
    reorder_flag = CASE
        WHEN units_in_stock < reorder_level THEN true
        ELSE false
    END;
```

**Разбор использования в UPDATE:**
- Позволяет выполнять сложные условные обновления в одном запросе
- Эффективнее чем множественные UPDATE с разными условиями
- Может использоваться для поддержания целостности данных
- Важно тестировать на небольшом наборе данных перед применением

[↑ К содержанию](#^k9p2r4m7x)

### CASE в агрегатных функциях ^d6e7f8g9h0
Использование CASE внутри агрегатных функций для условной агрегации.

**Примеры с агрегатными функциями:**
```sql
-- Условный COUNT
SELECT 
    department_id,
    COUNT(*) as total_employees,
    COUNT(CASE WHEN salary > 50000 THEN 1 END) as high_earners,
    COUNT(CASE WHEN salary <= 50000 THEN 1 END) as standard_earners
FROM employees
GROUP BY department_id;

-- Условный SUM
SELECT 
    product_category,
    SUM(quantity_sold) as total_sold,
    SUM(CASE WHEN EXTRACT(MONTH FROM sale_date) = 1 THEN quantity_sold ELSE 0 END) as jan_sales,
    SUM(CASE WHEN EXTRACT(MONTH FROM sale_date) = 2 THEN quantity_sold ELSE 0 END) as feb_sales
FROM sales
GROUP BY product_category;

-- Условный AVG
SELECT 
    region,
    AVG(CASE WHEN product_type = 'premium' THEN price END) as avg_premium_price,
    AVG(CASE WHEN product_type = 'standard' THEN price END) as avg_standard_price
FROM products
GROUP BY region;
```

**Разбор использования с агрегатными функциями:**
- Позволяет создавать сводные отчеты без использования PIVOT
- Эффективно для анализа данных по различным категориям
- Может заменить множественные подзапросы
- Широко используется в аналитических отчетах

[↑ К содержанию](#^k9p2r4m7x)

### CASE в HAVING ^i1j2k3l4m5
Использование CASE в предложении HAVING для сложной фильтрации групп.

**Примеры в HAVING:**
```sql
-- Фильтрация групп по сложным условиям
SELECT 
    department_id,
    AVG(salary) as avg_salary,
    COUNT(*) as employee_count
FROM employees
GROUP BY department_id
HAVING 
    CASE 
        WHEN COUNT(*) > 10 THEN AVG(salary)
        ELSE 0 
    END > 50000;

-- Условная фильтрация на основе агрегатов
SELECT 
    customer_id,
    COUNT(*) as order_count,
    SUM(total_amount) as total_spent
FROM orders
GROUP BY customer_id
HAVING 
    CASE 
        WHEN COUNT(*) >= 5 THEN SUM(total_amount) / COUNT(*)
        ELSE 0 
    END > 1000;
```

**Разбор использования в HAVING:**
- Позволяет создавать сложные условия фильтрации групп
- Может использоваться для реализации бизнес-правил агрегации
- Менее распространено но очень мощно в определенных сценариях

[↑ К содержанию](#^k9p2r4m7x)

## Вложенные CASE выражения ^n6o7p8q9r0
CASE выражения могут быть вложенными для реализации сложной многоуровневой логики.

**Примеры вложенных CASE:**
```sql
-- Многоуровневая логика категоризации
SELECT 
    employee_id,
    salary,
    performance_rating,
    CASE
        WHEN salary > 80000 THEN
            CASE
                WHEN performance_rating = 'excellent' THEN 'Звезда'
                WHEN performance_rating = 'good' THEN 'Стабильный'
                ELSE 'Дорогой'
            END
        WHEN salary BETWEEN 50000 AND 80000 THEN
            CASE
                WHEN performance_rating = 'excellent' THEN 'Перспективный'
                WHEN performance_rating = 'good' THEN 'Надежный'
                ELSE 'Требует внимания'
            END
        ELSE
            CASE
                WHEN performance_rating = 'excellent' THEN 'Растущий'
                ELSE 'Начальный'
            END
    END as employee_category
FROM employees;

-- Сложная бизнес-логика
SELECT 
    order_id,
    total_amount,
    customer_segment,
    CASE
        WHEN total_amount > 1000 THEN
            CASE
                WHEN customer_segment = 'VIP' THEN 'Крупный VIP заказ'
                WHEN customer_segment = 'Regular' THEN 'Крупный регулярный заказ'
                ELSE 'Крупный новый заказ'
            END
        ELSE
            CASE
                WHEN customer_segment = 'VIP' THEN 'Стандартный VIP заказ'
                ELSE 'Стандартный заказ'
            END
    END as order_classification
FROM orders;
```

**Рекомендации по вложенным CASE:**
- Избегайте излишней вложенности (более 2-3 уровней)
- Используйте комментарии для сложной логики
- Рассмотрите возможность вынесения логики в представления или функции
- Тестируйте все возможные ветки выполнения

[↑ К содержанию](#^k9p2r4m7x)

## Практические примеры ^s1t2u3v4w5
Реальные сценарии использования выражения CASE в бизнес-задачах.

### Пример 1: Анализ продаж
```sql
-- Детальный анализ продаж по категориям и периодам
SELECT 
    product_category,
    COUNT(*) as total_orders,
    SUM(CASE WHEN EXTRACT(QUARTER FROM order_date) = 1 THEN total_amount ELSE 0 END) as q1_sales,
    SUM(CASE WHEN EXTRACT(QUARTER FROM order_date) = 2 THEN total_amount ELSE 0 END) as q2_sales,
    SUM(CASE WHEN EXTRACT(QUARTER FROM order_date) = 3 THEN total_amount ELSE 0 END) as q3_sales,
    SUM(CASE WHEN EXTRACT(QUARTER FROM order_date) = 4 THEN total_amount ELSE 0 END) as q4_sales,
    CASE
        WHEN SUM(total_amount) > 100000 THEN 'Высокий'
        WHEN SUM(total_amount) > 50000 THEN 'Средний'
        ELSE 'Низкий'
    END as sales_performance
FROM orders
JOIN products ON orders.product_id = products.product_id
GROUP BY product_category
ORDER BY SUM(total_amount) DESC;
```

### Пример 2: Система грейдов сотрудников
```sql
-- Комплексная система оценки сотрудников
SELECT 
    e.employee_id,
    e.name,
    e.department_name,
    e.salary,
    p.performance_score,
    CASE
        WHEN e.salary >= 80000 AND p.performance_score >= 90 THEN 'A1'
        WHEN e.salary >= 80000 AND p.performance_score >= 80 THEN 'A2'
        WHEN e.salary >= 60000 AND p.performance_score >= 90 THEN 'B1'
        WHEN e.salary >= 60000 AND p.performance_score >= 80 THEN 'B2'
        WHEN e.salary >= 40000 AND p.performance_score >= 90 THEN 'C1'
        WHEN e.salary >= 40000 AND p.performance_score >= 80 THEN 'C2'
        ELSE 'D'
    END as grade_level,
    CASE grade_level
        WHEN 'A1' THEN 'Высший уровень'
        WHEN 'A2' THEN 'Продвинутый уровень'
        WHEN 'B1' THEN 'Высокий уровень'
        WHEN 'B2' THEN 'Стабильный уровень'
        WHEN 'C1' THEN 'Развивающийся уровень'
        WHEN 'C2' THEN 'Базовый уровень'
        ELSE 'Требует улучшения'
    END as grade_description
FROM employees e
LEFT JOIN performance p ON e.employee_id = p.employee_id;
```

### Пример 3: Интеллектуальная система скидок
```sql
-- Динамический расчет скидок на основе истории покупок
SELECT 
    c.customer_id,
    c.customer_name,
    COUNT(o.order_id) as order_count,
    SUM(o.total_amount) as lifetime_value,
    CASE
        WHEN SUM(o.total_amount) > 10000 THEN 
            CASE 
                WHEN COUNT(o.order_id) > 20 THEN 0.15  -- 15% для VIP
                ELSE 0.10                              -- 10% для постоянных
            END
        WHEN SUM(o.total_amount) > 5000 THEN 0.07      -- 7% для лояльных
        WHEN COUNT(o.order_id) > 5 THEN 0.05           -- 5% для активных
        ELSE 0.02                                      -- 2% для новых
    END as discount_rate,
    CASE
        WHEN discount_rate >= 0.10 THEN 'VIP клиент'
        WHEN discount_rate >= 0.07 THEN 'Постоянный клиент'
        WHEN discount_rate >= 0.05 THEN 'Активный клиент'
        ELSE 'Новый клиент'
    END as customer_tier
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```

[↑ К содержанию](#^k9p2r4m7x)

## Лучшие практики ^x3y4z5a6b7
Эффективное использование выражения CASE требует соблюдения определенных правил и рекомендаций.

### Производительность:
```sql
-- ✅ Эффективно: условия в порядке вероятности
SELECT 
    CASE
        WHEN status = 'completed' THEN 1  -- Самый частый статус
        WHEN status = 'pending' THEN 2    -- Частый статус
        WHEN status = 'cancelled' THEN 3  -- Редкий статус
        ELSE 4
    END as status_code
FROM orders;

-- ❌ Менее эффективно: редкие условия первыми
SELECT 
    CASE
        WHEN status = 'cancelled' THEN 3  -- Редкий статус первым
        WHEN status = 'pending' THEN 2
        WHEN status = 'completed' THEN 1
        ELSE 4
    END as status_code
FROM orders;
```

### Читаемость кода:
```sql
-- ✅ Хорошо: отформатированный и комментированный код
SELECT 
    employee_id,
    salary,
    -- Категоризация зарплат для отчетности
    CASE
        WHEN salary > 100000 THEN 'Executive'
        WHEN salary > 70000 THEN 'Senior'
        WHEN salary > 50000 THEN 'Mid'
        WHEN salary > 30000 THEN 'Junior'
        ELSE 'Entry'
    END as salary_band,  -- Уровень зарплаты
    ...
FROM employees;

-- ❌ Плохо: нечитаемый код
SELECT emp_id,sal,CASE WHEN sal>100000 THEN 'E' WHEN sal>70000 THEN 'S' WHEN sal>50000 THEN 'M' WHEN sal>30000 THEN 'J' ELSE 'N' END FROM emp;
```

### Обработка NULL:
```sql
-- ✅ Правильно: явная обработка NULL
SELECT 
    product_name,
    price,
    CASE
        WHEN price IS NULL THEN 'Цена не указана'
        WHEN price > 100 THEN 'Дорогой'
        ELSE 'Доступный'
    END as price_category
FROM products;

-- ✅ Использование COALESCE с CASE
SELECT 
    customer_name,
    COALESCE(
        CASE 
            WHEN total_orders > 10 THEN 'VIP'
            WHEN total_orders > 5 THEN 'Regular'
        END,
        'New'
    ) as customer_type
FROM customers;
```

### Совместимость типов:
```sql
-- ✅ Правильно: согласованные типы в THEN
SELECT 
    CASE
        WHEN condition1 THEN 'Текст'
        WHEN condition2 THEN 'Другой текст'
        ELSE 'Текст по умолчанию'
    END as consistent_text;

-- ✅ Явное приведение типов при необходимости
SELECT 
    CASE
        WHEN condition1 THEN CAST(100 AS VARCHAR)
        WHEN condition2 THEN 'Текст'
        ELSE '0'
    END as explicit_conversion;
```

<table>
<tr>
<td><strong>Практика</strong></td>
<td><strong>Рекомендация</strong></td>
<td><strong>Пример</strong></td>
</tr>
<tr>
<td>Порядок условий</td>
<td>Располагайте частые условия первыми</td>
<td>WHEN common THEN ... WHEN rare THEN ...</td>
</tr>
<tr>
<td>Форматирование</td>
<td>Используйте отступы и переносы строк</td>
<td>Выровненные WHEN/THEN</td>
</tr>
<tr>
<td>Обработка NULL</td>
<td>Всегда учитывайте возможные NULL</td>
<td>WHEN column IS NULL THEN ...</td>
</tr>
<tr>
<td>ELSE условие</td>
<td>Всегда указывайте ELSE явно</td>
<td>ELSE 'default'</td>
</tr>
<tr>
<td>Тестирование</td>
<td>Проверяйте все ветки условий</td>
<td>Тестовые данные для всех WHEN</td>
</tr>
</table>

[↑ К содержанию](#^k9p2r4m7x)

## Заключение ^c8d9e0f1g2
Выражение CASE является одним из самых мощных и универсальных инструментов в SQL, предоставляющим возможности для реализации сложной условной логики непосредственно в запросах.

<big>**Ключевые преимущества выражения CASE:**</big>

- **Гибкость**: Поддержка сложных условий и многоуровневой логики
- **Универсальность**: Возможность использования в любом контексте SQL-выражений
- **Читаемость**: Прямое отражение бизнес-логики в структуре запроса
- **Производительность**: Часто эффективнее множественных запросов или JOIN

<big>**Основные сценарии применения:**</big>

- **Категоризация данных**: Группировка и классификация записей
- **Условные вычисления**: Динамические расчеты на основе условий
- **Сложная фильтрация**: Условные WHERE и HAVING условия
- **Динамическая сортировка**: Пользовательские порядки ORDER BY
- **Агрегатный анализ**: Условные COUNT, SUM, AVG и другие агрегаты

**Критические рекомендации:**
- Всегда указывайте ELSE условие для обработки непредвиденных случаев
- Располагайте условия в порядке убывания вероятности для оптимизации
- Используйте поисковый CASE для максимальной гибкости
- Избегайте излишней вложенности - максимум 2-3 уровня
- Тестируйте все возможные ветки выполнения

Выражение CASE, при правильном использовании, значительно расширяет возможности SQL, позволяя создавать сложные, эффективные и поддерживаемые запросы для решения разнообразных бизнес-задач.

[↑ К содержанию](#^k9p2r4m7x)

**Теги:** #sql #case #условная-логика #выражения-sql #аналитика-данных #базы-данных
```