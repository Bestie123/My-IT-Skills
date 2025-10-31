Java Arrays - Исчерпывающая документация

Содержание

· Введение
· Создание массивов
· Доступ к элементам
· Многомерные массивы
· Класс Arrays
· Полезные методы
· Примеры использования

---

Введение

Массивы в Java — это структура данных, которая хранит фиксированное количество элементов одного типа.

Основные характеристики:

· Фиксированный размер после создания
· Индексация с 0
· Быстрый доступ по индексу
· Могут хранить примитивы или объекты

Создание массивов

Синтаксис объявления

```java
// Способы объявления
тип[] имяМассива;
тип имяМассива[];

// Примеры
int[] numbers;
String names[];
```

Инициализация

```java
// 1. Объявление + создание
int[] numbers = new int[5];

// 2. Объявление + явная инициализация
int[] numbers = {1, 2, 3, 4, 5};

// 3. Поэтапно
int[] numbers;
numbers = new int[5];

// 4. С отложенной инициализацией
int[] numbers = new int[]{1, 2, 3, 4, 5};
```

Доступ к элементам

Основные операции

```java
int[] arr = new int[3];

// Запись
arr[0] = 10;
arr[1] = 20;
arr[2] = 30;

// Чтение
System.out.println(arr[0]); // 10
System.out.println(arr[1]); // 20

// Длина массива
int length = arr.length; // 3
```

Итерация по массиву

```java
// For loop
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}

// Enhanced for loop
for (int element : arr) {
    System.out.println(element);
}

// Stream API
Arrays.stream(arr).forEach(System.out::println);
```

Многомерные массивы

Двумерные массивы

```java
// Создание
int[][] matrix = new int[3][3];

// Инициализация
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Доступ к элементам
matrix[0][0] = 1;  // первая строка, первый столбец
matrix[1][2] = 6;  // вторая строка, третий столбец

// Итерация
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

Рваные массивы (Jagged Arrays)

```java
// Создание массива с разной длиной подмассивов
int[][] jagged = new int[3][];
jagged[0] = new int[2];
jagged[1] = new int[4];
jagged[2] = new int[3];
```

Класс Arrays

Класс java.util.Arrays предоставляет утилитные методы для работы с массивами.

Импорт

```java
import java.util.Arrays;
```

Полезные методы

Сортировка

```java
int[] numbers = {5, 2, 8, 1, 9};

// Сортировка всего массива
Arrays.sort(numbers); // [1, 2, 5, 8, 9]

// Сортировка части массива
Arrays.sort(numbers, 1, 4); // сортировка с 1 по 3 индекс
```

Бинарный поиск

```java
int[] numbers = {1, 2, 3, 4, 5};
int index = Arrays.binarySearch(numbers, 3); // 2
int notFound = Arrays.binarySearch(numbers, 6); // отрицательное число
```

Важно: Массив должен быть отсортирован перед использованием бинарного поиска.

Сравнение массивов

```java
int[] arr1 = {1, 2, 3};
int[] arr2 = {1, 2, 3};
int[] arr3 = {1, 2, 4};

boolean equal1 = Arrays.equals(arr1, arr2); // true
boolean equal2 = Arrays.equals(arr1, arr3); // false
```

Заполнение массива

```java
int[] arr = new int[5];

// Заполнение одним значением
Arrays.fill(arr, 10); // [10, 10, 10, 10, 10]

// Заполнение части массива
Arrays.fill(arr, 1, 4, 5); // [10, 5, 5, 5, 10]
```

Копирование массивов

```java
int[] original = {1, 2, 3, 4, 5};

// copyOf - копирует с начала
int[] copy1 = Arrays.copyOf(original, 3); // [1, 2, 3]
int[] copy2 = Arrays.copyOf(original, 7); // [1, 2, 3, 4, 5, 0, 0]

// copyOfRange - копирует диапазон
int[] copy3 = Arrays.copyOfRange(original, 1, 4); // [2, 3, 4]

// System.arraycopy
int[] copy4 = new int[5];
System.arraycopy(original, 0, copy4, 0, original.length);
```

Преобразование в строку

```java
int[] arr = {1, 2, 3};
String str = Arrays.toString(arr); // "[1, 2, 3]"

// Для многомерных массивов
int[][] matrix = {{1, 2}, {3, 4}};
String deepStr = Arrays.deepToString(matrix); // "[[1, 2], [3, 4]]"
```

Потоки (Streams)

```java
int[] numbers = {1, 2, 3, 4, 5};

// Создание потока из массива
Arrays.stream(numbers)
      .filter(n -> n % 2 == 0)
      .forEach(System.out::println);
```

Примеры использования

Базовые операции

```java
public class ArrayExamples {
    public static void main(String[] args) {
        // Создание и заполнение
        int[] numbers = new int[10];
        for (int i = 0; i < numbers.length; i++) {
            numbers[i] = i * 2;
        }
        
        // Поиск максимального элемента
        int max = numbers[0];
        for (int num : numbers) {
            if (num > max) {
                max = num;
            }
        }
        System.out.println("Максимум: " + max);
        
        // Реверс массива
        for (int i = 0; i < numbers.length / 2; i++) {
            int temp = numbers[i];
            numbers[i] = numbers[numbers.length - 1 - i];
            numbers[numbers.length - 1 - i] = temp;
        }
    }
}
```

Работа с объектами

```java
class Person {
    String name;
    int age;
    
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

// Массив объектов
Person[] people = new Person[3];
people[0] = new Person("Alice", 25);
people[1] = new Person("Bob", 30);
people[2] = new Person("Charlie", 35);

// Сортировка по возрасту
Arrays.sort(people, (p1, p2) -> Integer.compare(p1.age, p2.age));
```

Утильные методы

```java
public class ArrayUtils {
    // Поиск элемента в массиве
    public static <T> int findIndex(T[] array, T target) {
        for (int i = 0; i < array.length; i++) {
            if (array[i].equals(target)) {
                return i;
            }
        }
        return -1;
    }
    
    // Объединение массивов
    public static int[] concatenate(int[] a, int[] b) {
        int[] result = new int[a.length + b.length];
        System.arraycopy(a, 0, result, 0, a.length);
        System.arraycopy(b, 0, result, a.length, b.length);
        return result;
    }
}
```

Особенности и лучшие практики

1. Проверка границ: Всегда проверяйте индексы перед доступом
2. Null безопасность: Проверяйте массив на null перед использованием
3. Копирование vs ссылки: Помните, что присваивание массивов создает ссылки, а не копии
4. Инициализация: Примитивы инициализируются нулевыми значениями, объекты - null

```java
// Безопасный доступ
public static void safeArrayAccess(int[] arr, int index) {
    if (arr != null && index >= 0 && index < arr.length) {
        System.out.println(arr[index]);
    } else {
        System.out.println("Некорректный индекс или массив");
    }
}
```

---

Связанные темы

· [[Java Collections]]
· [[Java Streams API]]
· [[Java Generics]]

Теги

#java #arrays #programming #datastructures