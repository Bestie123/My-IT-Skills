Java Loops - Исчерпывающая документация

Содержание

· Введение
· Цикл for
· Улучшенный for-each
· Цикл while
· Цикл do-while
· Операторы управления
· Вложенные циклы
· Бесконечные циклы
· Практические примеры
· Лучшие практики

Введение

Циклы в Java используются для многократного выполнения блока кода. Основные типы циклов:

· for - когда известно количество итераций
· while - когда условие проверяется перед итерацией
· do-while - когда условие проверяется после итерации
· for-each - для итерации по коллекциям и массивам

Цикл for

Синтаксис

```java
for (инициализация; условие; обновление) {
    // тело цикла
}
```

Примеры

```java
// Базовый пример
for (int i = 0; i < 5; i++) {
    System.out.println("Итерация: " + i);
}

// Несколько переменных
for (int i = 0, j = 10; i < j; i++, j--) {
    System.out.println(i + " : " + j);
}

// Обратный отсчет
for (int i = 10; i > 0; i--) {
    System.out.println(i);
}

// Пропуск инициализации (если переменная объявлена ранее)
int k = 0;
for (; k < 5; k++) {
    System.out.println(k);
}
```

Улучшенный for-each

Синтаксис

```java
for (тип переменная : коллекция) {
    // тело цикла
}
```

Примеры

```java
// Для массивов
int[] numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    System.out.println(num);
}

// Для коллекций
List<String> names = Arrays.asList("Анна", "Борис", "Виктор");
for (String name : names) {
    System.out.println(name);
}

// Для строк (символы)
String text = "Hello";
for (char c : text.toCharArray()) {
    System.out.println(c);
}
```

Цикл while

Синтаксис

```java
while (условие) {
    // тело цикла
}
```

Примеры

```java
// Базовый пример
int count = 0;
while (count < 5) {
    System.out.println("Счет: " + count);
    count++;
}

// Чтение данных до определенного условия
Scanner scanner = new Scanner(System.in);
String input;
while (!(input = scanner.nextLine()).equals("exit")) {
    System.out.println("Вы ввели: " + input);
}

// Обработка коллекций
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
while (!numbers.isEmpty()) {
    System.out.println(numbers.remove(0));
}
```

Цикл do-while

Синтаксис

```java
do {
    // тело цикла
} while (условие);
```

Примеры

```java
// Гарантированное выполнение хотя бы один раз
int attempts = 0;
do {
    System.out.println("Попытка: " + (attempts + 1));
    attempts++;
} while (attempts < 3);

// Меню пользователя
Scanner scanner = new Scanner(System.in);
int choice;
do {
    System.out.println("1. Опция 1");
    System.out.println("2. Опция 2");
    System.out.println("0. Выход");
    System.out.print("Выберите: ");
    choice = scanner.nextInt();
} while (choice != 0);
```

Операторы управления

break

```java
// Прерывание цикла
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break; // выход при i = 5
    }
    System.out.println(i);
}

// break с меткой
outerLoop:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i * j == 4) {
            break outerLoop; // выход из обоих циклов
        }
    }
}
```

continue

```java
// Пропуск итерации
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue; // пропуск четных чисел
    }
    System.out.println(i);
}

// continue с меткой
outerLoop:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            continue outerLoop; // переход к следующей итерации внешнего цикла
        }
        System.out.println(i + "," + j);
    }
}
```

return

```java
public int findNumber(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i; // выход из метода и цикла
        }
    }
    return -1;
}
```

Вложенные циклы

Примеры

```java
// Таблица умножения
for (int i = 1; i <= 10; i++) {
    for (int j = 1; j <= 10; j++) {
        System.out.printf("%4d", i * j);
    }
    System.out.println();
}

// Обработка двумерного массива
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

Бесконечные циклы

Примеры

```java
// Бесконечный for
for (;;) {
    // код
    if (условиеВыхода) break;
}

// Бесконечный while
while (true) {
    // код
    if (условиеВыхода) break;
}

// Серверное приложение
while (true) {
    // Ожидание подключений
    // Обработка запросов
    if (shouldShutdown) break;
}
```

Практические примеры

Обработка коллекций

```java
// Фильтрация списка
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<Integer> evenNumbers = new ArrayList<>();

for (int num : numbers) {
    if (num % 2 == 0) {
        evenNumbers.add(num);
    }
}
```

Поиск элемента

```java
// Поиск в массиве
String[] names = {"Анна", "Борис", "Виктор", "Галина"};
String target = "Виктор";
boolean found = false;

for (String name : names) {
    if (name.equals(target)) {
        found = true;
        break;
    }
}
```

Накопление суммы

```java
// Сумма элементов
int[] values = {10, 20, 30, 40, 50};
int sum = 0;

for (int value : values) {
    sum += value;
}
```

Лучшие практики

1. Выбор правильного цикла

```java
// Известно количество итераций - используйте for
for (int i = 0; i < array.length; i++) { }

// Итерация по коллекции - используйте for-each
for (String item : collection) { }

// Условие выхода неизвестно - используйте while
while (condition) { }

// Гарантированное выполнение - используйте do-while
do { } while (condition);
```

2. Оптимизация производительности

```java
// Кэширование длины массива
for (int i = 0, n = array.length; i < n; i++) { }

// Минимизация операций в условии
int length = list.size();
for (int i = 0; i < length; i++) { }
```

3. Читаемость кода

```java
// Хорошо - понятные имена переменных
for (int studentIndex = 0; studentIndex < studentCount; studentIndex++) { }

// Плохо - непонятные имена
for (int i = 0; i < n; i++) { }
```

4. Избегание распространенных ошибок

```java
// Бесконечный цикл
while (true) {
    if (exitCondition) break; // всегда предусматривайте выход
}

// Изменение счетчика цикла
for (int i = 0; i < 10; i++) {
    if (condition) {
        i--; // опасно! может привести к бесконечному циклу
    }
}
```

5. Обработка исключений

```java
for (int i = 0; i < 10; i++) {
    try {
        // код, который может вызвать исключение
    } catch (Exception e) {
        // обработка исключения
        continue; // переход к следующей итерации
    }
}
```

Связанные темы

· [[Java Arrays]]
· [[Java Collections]]
· [[Java Control Statements]]
· [[Java Exception Handling]]

---

Документация создана для базы знаний Obsidian. Обновлено: ${new Date().toLocaleDateString()}

https://codingbat.com/doc/java-for-while-loops.html