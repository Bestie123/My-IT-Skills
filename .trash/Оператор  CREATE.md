
# Оператор CREATE в SQL

## Содержание ^b3n8t5q1w
- [Введение](#^c4m7v2k9p)
- [Обзор оператора CREATE](#^d5n8w1x2y)
- [CREATE TABLE](#^e6o9y3z4a)
  - [Синтаксис создания таблиц](#^f7p0a5b6c)
  - [Типы данных](#^g8q1c7d8e)
  - [Ограничения (Constraints)](#^h9r2e9f0g)
- [CREATE INDEX](#^i0s3g1h2i)
- [CREATE VIEW](#^j1t4h3j4k)
- [CREATE DATABASE](#^k2u5i5l6m)
- [CREATE PROCEDURE и FUNCTION](#^l3v6j7n8o)
- [Другие объекты CREATE](#^m4w7k9p0q)
- [Лучшие практики](#^n5x8l0r1s)
- [Заключение](#^o6y9m2t3u)

## Введение ^c4m7v2k9p
Оператор CREATE является фундаментальным оператором языка SQL, используемым для создания различных объектов в базе данных. От таблиц и индексов до представлений и хранимых процедур - CREATE позволяет определять структуру данных и функциональность базы данных.

<big>**Основные объекты, создаваемые с помощью CREATE:**</big>

- Таблицы (TABLE)
- Индексы (INDEX)
- Представления (VIEW)
- Базы данных (DATABASE)
- Хранимые процедуры (PROCEDURE)
- Функции (FUNCTION)
- Триггеры (TRIGGER)
- Пользователи (USER)

<small>Синтаксис CREATE может незначительно отличаться в разных СУБД (MySQL, PostgreSQL, SQL Server, Oracle).</small>

[↑ К содержанию](#^b3n8t5q1w)

## Обзор оператора CREATE ^d5n8w1x2y
Оператор CREATE используется для создания новых объектов в базе данных. Каждый тип объекта имеет свой специфический синтаксис, но общая структура следует определенным правилам.

### Базовый синтаксис:
```sql
CREATE OBJECT_TYPE object_name
(
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
) [additional_options];
```

### Основные типы объектов:
```sql
-- Создание таблицы
CREATE TABLE employees (...);

-- Создание индекса
CREATE INDEX idx_name ON table_name(column_name);

-- Создание представления
CREATE VIEW view_name AS SELECT ...;

-- Создание базы данных
CREATE DATABASE company_db;
```

<small>Привилегии пользователя должны включать право на создание объектов в целевой базе данных.</small>

[↑ К содержанию](#^b3n8t5q1w)

## CREATE TABLE ^e6o9y3z4a
CREATE TABLE - самый часто используемый оператор CREATE, позволяющий создавать новые таблицы в базе данных.

### Синтаксис создания таблиц ^f7p0a5b6c
Базовый синтаксис создания таблицы:

```sql
CREATE TABLE table_name
(
    column1 datatype [constraint],
    column2 datatype [constraint],
    column3 datatype [constraint],
    ...
    [table_constraints]
);
```

**Пример создания простой таблицы:**
```sql
CREATE TABLE customers
(
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    registration_date DATE DEFAULT CURRENT_DATE
);
```

**Расширенный пример с внешними ключами:**
```sql
CREATE TABLE orders
(
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE NOT NULL,
    total_amount DECIMAL(10,2),
    status VARCHAR(20) DEFAULT 'PENDING',
    
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    CHECK (total_amount >= 0)
);
```

[↑ К содержанию](#^b3n8t5q1w)

### Типы данных ^g8q1c7d8e
При создании таблиц необходимо указывать типы данных для каждого столбца.

<table>
<tr>
<td><strong>Категория</strong></td>
<td><strong>Типы данных</strong></td>
<td><strong>Описание</strong></td>
</tr>
<tr>
<td>Строковые</td>
<td>CHAR(n), VARCHAR(n), TEXT</td>
<td>Текстовые данные различной длины</td>
</tr>
<tr>
<td>Числовые</td>
<td>INT, BIGINT, DECIMAL(p,s), FLOAT</td>
<td>Целые и дробные числа</td>
</tr>
<tr>
<td>Дата/время</td>
<td>DATE, TIME, DATETIME, TIMESTAMP</td>
<td>Дата и время</td>
</tr>
<tr>
<td>Бинарные</td>
<td>BLOB, BINARY, VARBINARY</td>
<td>Двоичные данные</td>
</tr>
<tr>
<td>Логические</td>
<td>BOOLEAN, BIT</td>
<td>Логические значения</td>
</tr>
<tr>
<td>Специальные</td>
<td>JSON, XML, UUID, ARRAY</td>
<td>Специализированные типы</td>
</tr>
</table>

**Пример с различными типами данных:**
```sql
CREATE TABLE product_catalog
(
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10,2) NOT NULL,
    quantity_in_stock INT DEFAULT 0,
    is_available BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    specifications JSON,
    product_image BYTEA
);
```

[↑ К содержанию](#^b3n8t5q1w)

### Ограничения (Constraints) ^h9r2e9f0g
Ограничения обеспечивают целостность данных в таблице.

**Основные типы ограничений:**

```sql
CREATE TABLE employees
(
    -- PRIMARY KEY - уникальный идентификатор
    employee_id INT PRIMARY KEY,
    
    -- NOT NULL - обязательное поле
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    
    -- UNIQUE - уникальные значения
    email VARCHAR(100) UNIQUE,
    social_security_number VARCHAR(20) UNIQUE,
    
    -- CHECK - проверка условия
    age INT CHECK (age >= 18),
    salary DECIMAL(10,2) CHECK (salary > 0),
    
    -- DEFAULT - значение по умолчанию
    hire_date DATE DEFAULT CURRENT_DATE,
    status VARCHAR(20) DEFAULT 'ACTIVE',
    
    -- FOREIGN KEY - связь с другой таблицей
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

**Ограничения на уровне таблицы:**
```sql
CREATE TABLE order_items
(
    order_id INT,
    product_id INT,
    quantity INT NOT NULL,
    unit_price DECIMAL(10,2),
    
    -- Составной первичный ключ
    PRIMARY KEY (order_id, product_id),
    
    -- Внешние ключи
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id),
    
    -- Проверка на уровне таблицы
    CHECK (quantity > 0 AND unit_price >= 0)
);
```

[↑ К содержанию](#^b3n8t5q1w)

## CREATE INDEX ^i0s3g1h2i
Индексы ускоряют поиск данных в таблицах за счет дополнительных структур данных.

### Базовый синтаксис:
```sql
CREATE [UNIQUE] INDEX index_name
ON table_name (column1 [ASC|DESC], column2 [ASC|DESC], ...);
```

**Примеры создания индексов:**
```sql
-- Простой индекс на одном столбце
CREATE INDEX idx_customer_email ON customers(email);

-- Уникальный индекс
CREATE UNIQUE INDEX idx_product_sku ON products(sku);

-- Составной индекс на нескольких столбцах
CREATE INDEX idx_orders_date_customer ON orders(order_date, customer_id);

-- Индекс с сортировкой
CREATE INDEX idx_products_price_desc ON products(price DESC);

-- Частичный индекс (только для определенных строк)
CREATE INDEX idx_active_users ON users(email) WHERE is_active = TRUE;
```

**Специализированные типы индексов:**
```sql
-- PostgreSQL: GIN индекс для полнотекстового поиска
CREATE INDEX idx_documents_content ON documents USING GIN (to_tsvector('english', content));

-- MySQL: FULLTEXT индекс
CREATE FULLTEXT INDEX idx_articles_title_content ON articles(title, content);

-- SQL Server: Filtered index
CREATE INDEX idx_recent_orders ON orders(order_id) WHERE order_date > '2024-01-01';
```

<small>Индексы улучшают производительность чтения, но могут замедлять операции вставки и обновления.</small>

[↑ К содержанию](#^b3n8t5q1w)

## CREATE VIEW ^j1t4h3j4k
Представления (VIEW) - это виртуальные таблицы, основанные на результате SQL-запроса.

### Синтаксис создания представлений:
```sql
CREATE [OR REPLACE] VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

**Примеры представлений:**
```sql
-- Простое представление
CREATE VIEW active_customers AS
SELECT customer_id, first_name, last_name, email
FROM customers
WHERE status = 'ACTIVE';

-- Представление с объединением таблиц
CREATE VIEW order_summary AS
SELECT 
    o.order_id,
    o.order_date,
    c.first_name || ' ' || c.last_name AS customer_name,
    COUNT(oi.product_id) as item_count,
    SUM(oi.quantity * oi.unit_price) as total_amount
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY o.order_id, o.order_date, c.first_name, c.last_name;

-- Обновляемое представление
CREATE VIEW customer_contacts AS
SELECT customer_id, first_name, last_name, email, phone
FROM customers
WHERE email IS NOT NULL;
```

**Материализованные представления (PostgreSQL):**
```sql
CREATE MATERIALIZED VIEW monthly_sales AS
SELECT 
    DATE_TRUNC('month', order_date) as month,
    COUNT(*) as order_count,
    SUM(total_amount) as revenue
FROM orders
GROUP BY DATE_TRUNC('month', order_date);

-- Обновление материализованного представления
REFRESH MATERIALIZED VIEW monthly_sales;
```

[↑ К содержанию](#^b3n8t5q1w)

## CREATE DATABASE ^k2u5i5l6m
Создание новой базы данных.

### Базовый синтаксис:
```sql
CREATE DATABASE database_name
[CHARACTER SET charset_name]
[COLLATE collation_name];
```

**Примеры создания баз данных:**
```sql
-- Простое создание базы данных
CREATE DATABASE company;

-- С указанием кодировки и collation
CREATE DATABASE company_utf8
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;

-- PostgreSQL: создание с параметрами
CREATE DATABASE analytics
WITH 
    OWNER = admin
    ENCODING = 'UTF8'
    CONNECTION LIMIT = 100;

-- SQL Server: создание с файловыми группами
CREATE DATABASE sales_db
ON PRIMARY 
(
    NAME = sales_data,
    FILENAME = '/var/data/sales.mdf',
    SIZE = 100MB,
    MAXSIZE = UNLIMITED,
    FILEGROWTH = 10%
)
LOG ON 
(
    NAME = sales_log,
    FILENAME = '/var/data/sales.ldf',
    SIZE = 50MB,
    MAXSIZE = 200MB,
    FILEGROWTH = 5%
);
```

[↑ К содержанию](#^b3n8t5q1w)

## CREATE PROCEDURE и FUNCTION ^l3v6j7n8o
Создание хранимых процедур и функций для инкапсуляции бизнес-логики.

### Хранимые процедуры:
```sql
-- MySQL/PostgreSQL
CREATE PROCEDURE process_order(IN order_id INT, OUT result VARCHAR(100))
BEGIN
    DECLARE order_status VARCHAR(20);
    
    SELECT status INTO order_status 
    FROM orders 
    WHERE order_id = order_id;
    
    IF order_status = 'PENDING' THEN
        UPDATE orders SET status = 'PROCESSED' WHERE order_id = order_id;
        SET result = 'Order processed successfully';
    ELSE
        SET result = 'Order already processed';
    END IF;
END;

-- SQL Server
CREATE PROCEDURE sp_get_customer_orders
    @customer_id INT
AS
BEGIN
    SELECT o.order_id, o.order_date, o.total_amount
    FROM orders o
    WHERE o.customer_id = @customer_id
    ORDER BY o.order_date DESC;
END;
```

### Пользовательские функции:
```sql
-- Скалярная функция
CREATE FUNCTION calculate_discount(price DECIMAL(10,2), discount_rate DECIMAL(3,2))
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN price * (1 - discount_rate);
END;

-- Табличная функция (SQL Server)
CREATE FUNCTION fn_get_products_by_category(@category_id INT)
RETURNS TABLE
AS
RETURN
    SELECT product_id, product_name, price
    FROM products
    WHERE category_id = @category_id;

-- Функция с условиями (PostgreSQL)
CREATE FUNCTION get_customer_level(total_purchases DECIMAL) 
RETURNS VARCHAR(20) AS $$
BEGIN
    IF total_purchases > 1000 THEN
        RETURN 'GOLD';
    ELSIF total_purchases > 500 THEN
        RETURN 'SILVER';
    ELSE
        RETURN 'STANDARD';
    END IF;
END;
$$ LANGUAGE plpgsql;
```

[↑ К содержанию](#^b3n8t5q1w)

## Другие объекты CREATE ^m4w7k9p0q
SQL поддерживает создание множества других объектов базы данных.

### CREATE SEQUENCE (последовательности):
```sql
-- Создание последовательности для автоинкремента
CREATE SEQUENCE order_id_seq
START WITH 1000
INCREMENT BY 1
MINVALUE 1000
MAXVALUE 999999
CYCLE;

-- Использование последовательности
CREATE TABLE orders
(
    order_id INT DEFAULT nextval('order_id_seq') PRIMARY KEY,
    -- другие столбцы...
);
```

### CREATE TRIGGER (триггеры):
```sql
-- Триггер для аудита изменений
CREATE TRIGGER audit_customer_changes
    BEFORE UPDATE ON customers
    FOR EACH ROW
BEGIN
    INSERT INTO customer_audit (customer_id, changed_field, old_value, new_value, change_date)
    VALUES (OLD.customer_id, 'email', OLD.email, NEW.email, CURRENT_TIMESTAMP);
END;
```

### CREATE SCHEMA (схемы):
```sql
-- Создание схемы для организации объектов
CREATE SCHEMA hr AUTHORIZATION hr_manager;

-- Создание таблицы в определенной схеме
CREATE TABLE hr.employees (...);
```

### CREATE USER и ROLE:
```sql
-- Создание пользователя
CREATE USER analyst_user 
IDENTIFIED BY 'secure_password'
PASSWORD EXPIRE INTERVAL 90 DAY;

-- Создание роли
CREATE ROLE data_reader;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO data_reader;
```

[↑ К содержанию](#^b3n8t5q1w)

## Лучшие практики ^n5x8l0r1s
Эффективное использование оператора CREATE требует соблюдения определенных правил и рекомендаций.

### Проектирование таблиц:
```sql
-- ✅ Правильно: использовать значимые имена
CREATE TABLE customer_orders (...);

-- ❌ Неправильно: использовать абстрактные имена
CREATE TABLE tbl1 (...);

-- ✅ Правильно: согласованное именование столбцов
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    unit_price DECIMAL(10,2)
);

-- ✅ Правильно: использовать ограничения для целостности данных
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE NOT NULL,
    hire_date DATE NOT NULL CHECK (hire_date <= CURRENT_DATE)
);
```

### Безопасность и производительность:
```sql
-- Минимальные привилегии при создании объектов
CREATE USER app_user WITH PASSWORD 'secure_pass';
GRANT CONNECT ON DATABASE company TO app_user;
GRANT USAGE ON SCHEMA public TO app_user;
GRANT SELECT, INSERT ON customers TO app_user;

-- Оптимизация индексов
CREATE INDEX idx_orders_composite ON orders (customer_id, order_date DESC)
WHERE status = 'COMPLETED'; -- Частичный индекс

-- Регулярное обслуживание
CREATE TABLE sales (
    sale_id BIGSERIAL PRIMARY KEY,
    sale_data JSONB NOT NULL
) PARTITION BY RANGE (sale_date); -- Партиционирование для больших таблиц
```

### Рекомендации по типам данных:
<table>
<tr>
<td><strong>Сценарий</strong></td>
<td><strong>Рекомендуемый тип</strong></td>
<td><strong>Причина</strong></td>
</tr>
<tr>
<td>Идентификаторы</td>
<td>INT/BIGSERIAL, UUID</td>
<td>Автоинкремент, глобальная уникальность</td>
</tr>
<tr>
<td>Имена, названия</td>
<td>VARCHAR(50-255)</td>
<td>Оптимальное использование памяти</td>
</tr>
<tr>
<td>Денежные суммы</td>
<td>DECIMAL(10,2)/NUMERIC</td>
<td>Точность вычислений</td>
</tr>
<tr>
<td>Дата/время</td>
<td>TIMESTAMP WITH TIME ZONE</td>
<td>Учет часовых поясов</td>
</tr>
<tr>
<td>Большие тексты</td>
<td>TEXT</td>
<td>Неограниченная длина</td>
</tr>
<tr>
<td>Флаги, состояния</td>
<td>BOOLEAN, ENUM</td>
<td>Семантическая ясность</td>
</tr>
</table>

[↑ К содержанию](#^b3n8t5q1w)

## Заключение ^o6y9m2t3u
Оператор CREATE является краеугольным камнем управления структурой базы данных в SQL. Его правильное использование позволяет создавать эффективные, безопасные и масштабируемые базы данных.

<big>**Ключевые принципы использования CREATE:**</big>

- **Планирование**: Тщательно проектируйте структуру перед созданием объектов
- **Стандартизация**: Используйте соглашения по именованию и типам данных
- **Безопасность**: Назначайте минимально необходимые привилегии
- **Производительность**: Создавайте индексы и используйте оптимизации
- **Документация**: Комментируйте сложные объекты базы данных

<big>**Основные объекты CREATE:**</big>

- **Таблицы**: Фундаментальные структуры для хранения данных
- **Индексы**: Ускорители производительности запросов
- **Представления**: Абстракции для упрощения доступа к данным
- **Процедуры/Функции**: Инкапсуляция бизнес-логики
- **Прочие объекты**: Последовательности, триггеры, схемы

Оператор CREATE, в сочетании с другими DDL-операторами (ALTER, DROP), образует основу для управления жизненным циклом объектов базы данных и обеспечения их эффективного функционирования.

[↑ К содержанию](#^b3n8t5q1w)

**Теги:** #sql #create #ddl #базы-данных #таблицы #индексы #представления
```