Массивы и циклы в Java

Массивы (Arrays)

Объявление массивов

```java
// Различные способы объявления массивов
int[] numbers1;          // Рекомендуемый стиль
int numbers2[];          // Альтернативный стиль

// Инициализация массивов
int[] numbers = new int[5];              // Массив из 5 нулей
String[] names = new String[3];          // Массив из 3 null
boolean[] flags = {true, false, true};   // Литеральная инициализация
```

Многомерные массивы

```java
// Двумерные массивы
int[][] matrix = new int[3][3];
int[][] jagged = new int[3][];           // Зубчатый массив
jagged[0] = new int[2];
jagged[1] = new int[4];
jagged[2] = new int[3];

// Инициализация
int[][] grid = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

Работа с массивами

```java
public class ArrayOperations {
    
    // Доступ к элементам
    public static void accessElements() {
        int[] arr = {10, 20, 30, 40, 50};
        
        System.out.println(arr[0]);     // 10
        System.out.println(arr.length); // 5
        
        // Изменение элемента
        arr[2] = 35;
        System.out.println(arr[2]);     // 35
    }
    
    // Копирование массивов
    public static void copyArrays() {
        int[] original = {1, 2, 3, 4, 5};
        
        // Метод arraycopy
        int[] copy1 = new int[5];
        System.arraycopy(original, 0, copy1, 0, original.length);
        
        // Метод Arrays.copyOf
        int[] copy2 = Arrays.copyOf(original, original.length);
        
        // Копирование с изменением размера
        int[] resized = Arrays.copyOf(original, 10);
    }
    
    // Сортировка и поиск
    public static void sortAndSearch() {
        int[] numbers = {5, 2, 8, 1, 9};
        
        Arrays.sort(numbers);                    // Сортировка
        int index = Arrays.binarySearch(numbers, 8); // Бинарный поиск
        
        System.out.println(Arrays.toString(numbers)); // [1, 2, 5, 8, 9]
        System.out.println("Index of 8: " + index);   // 3
    }
}
```

Класс Arrays - полезные методы

```java
import java.util.Arrays;

public class ArraysClassMethods {
    
    public static void demonstrateMethods() {
        int[] arr1 = {1, 2, 3};
        int[] arr2 = {1, 2, 3};
        
        // Сравнение массивов
        boolean equal = Arrays.equals(arr1, arr2); // true
        
        // Заполнение массива
        int[] filled = new int[5];
        Arrays.fill(filled, 42); // [42, 42, 42, 42, 42]
        
        // Преобразование в строку
        String arrayString = Arrays.toString(arr1); // "[1, 2, 3]"
        
        // Глубокое преобразование для многомерных массивов
        int[][] deepArray = {{1, 2}, {3, 4}};
        String deepString = Arrays.deepToString(deepArray); // "[[1, 2], [3, 4]]"
    }
}
```

Циклы (Loops)

Цикл for

```java
public class ForLoopExamples {
    
    // Базовый цикл for
    public static void basicForLoop() {
        for (int i = 0; i < 5; i++) {
            System.out.println("i = " + i);
        }
        // Вывод: 0, 1, 2, 3, 4
    }
    
    // Обратный отсчет
    public static void countdown() {
        for (int i = 10; i > 0; i--) {
            System.out.println(i);
        }
        System.out.println("Поехали!");
    }
    
    // Изменение шага
    public static void stepByTwo() {
        for (int i = 0; i <= 10; i += 2) {
            System.out.println(i); // 0, 2, 4, 6, 8, 10
        }
    }
}
```

Улучшенный цикл for (for-each)

```java
public class ForEachExamples {
    
    // Цикл for-each для массивов
    public static void iterateArray() {
        int[] numbers = {1, 2, 3, 4, 5};
        
        for (int num : numbers) {
            System.out.println(num);
        }
    }
    
    // For-each с многомерными массивами
    public static void iterate2DArray() {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        
        for (int[] row : matrix) {
            for (int cell : row) {
                System.out.print(cell + " ");
            }
            System.out.println();
        }
    }
}
```

Цикл while

```java
public class WhileLoopExamples {
    
    // Базовый цикл while
    public static void basicWhile() {
        int count = 0;
        while (count < 5) {
            System.out.println("Count: " + count);
            count++;
        }
    }
    
    // Чтение до условия
    public static void readUntilCondition() {
        Scanner scanner = new Scanner(System.in);
        String input;
        
        System.out.println("Введите 'quit' для выхода:");
        while (!(input = scanner.nextLine()).equals("quit")) {
            System.out.println("Вы ввели: " + input);
        }
        scanner.close();
    }
    
    // Бесконечный цикл с break
    public static void infiniteLoopWithBreak() {
        int i = 0;
        while (true) {
            System.out.println(i);
            i++;
            if (i >= 5) {
                break;
            }
        }
    }
}
```

Цикл do-while

```java
public class DoWhileExamples {
    
    // Цикл do-while (гарантированно выполнится хотя бы один раз)
    public static void basicDoWhile() {
        int i = 0;
        do {
            System.out.println("i = " + i);
            i++;
        } while (i < 5);
    }
    
    // Пример с пользовательским вводом
    public static void menuExample() {
        Scanner scanner = new Scanner(System.in);
        int choice;
        
        do {
            System.out.println("1. Опция 1");
            System.out.println("2. Опция 2");
            System.out.println("3. Выход");
            System.out.print("Выберите: ");
            choice = scanner.nextInt();
            
            switch (choice) {
                case 1 -> System.out.println("Выбрана опция 1");
                case 2 -> System.out.println("Выбрана опция 2");
                case 3 -> System.out.println("Выход...");
                default -> System.out.println("Неверный выбор");
            }
        } while (choice != 3);
        
        scanner.close();
    }
}
```

Операторы управления циклами

```java
public class LoopControl {
    
    // Оператор break
    public static void breakExample() {
        for (int i = 0; i < 10; i++) {
            if (i == 5) {
                break; // Выход из цикла при i = 5
            }
            System.out.println(i); // 0, 1, 2, 3, 4
        }
    }
    
    // Оператор continue
    public static void continueExample() {
        for (int i = 0; i < 10; i++) {
            if (i % 2 == 0) {
                continue; // Пропуск четных чисел
            }
            System.out.println(i); // 1, 3, 5, 7, 9
        }
    }
    
    // Метки для break и continue
    public static void labeledBreakContinue() {
        outerLoop:
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (i == 1 && j == 1) {
                    break outerLoop; // Выход из обоих циклов
                }
                System.out.println(i + ", " + j);
            }
        }
    }
}
```

Практические примеры

Обработка массивов с циклами

```java
public class ArrayLoopCombinations {
    
    // Поиск максимального элемента
    public static int findMax(int[] array) {
        if (array.length == 0) {
            throw new IllegalArgumentException("Массив не может быть пустым");
        }
        
        int max = array[0];
        for (int i = 1; i < array.length; i++) {
            if (array[i] > max) {
                max = array[i];
            }
        }
        return max;
    }
    
    // Поиск среднего значения
    public static double findAverage(int[] array) {
        if (array.length == 0) return 0;
        
        int sum = 0;
        for (int num : array) {
            sum += num;
        }
        return (double) sum / array.length;
    }
    
    // Реверс массива
    public static void reverseArray(int[] array) {
        int left = 0;
        int right = array.length - 1;
        
        while (left < right) {
            // Обмен элементов
            int temp = array[left];
            array[left] = array[right];
            array[right] = temp;
            
            left++;
            right--;
        }
    }
    
    // Поиск элемента в массиве
    public static int linearSearch(int[] array, int target) {
        for (int i = 0; i < array.length; i++) {
            if (array[i] == target) {
                return i;
            }
        }
        return -1; // Элемент не найден
    }
    
    // Копирование с фильтрацией
    public static int[] filterEvenNumbers(int[] numbers) {
        int count = 0;
        
        // Сначала подсчитаем четные числа
        for (int num : numbers) {
            if (num % 2 == 0) {
                count++;
            }
        }
        
        // Создаем новый массив нужного размера
        int[] result = new int[count];
        int index = 0;
        
        // Заполняем результат
        for (int num : numbers) {
            if (num % 2 == 0) {
                result[index++] = num;
            }
        }
        
        return result;
    }
}
```

Работа с многомерными массивами

```java
public class MultiDimensionalArrays {
    
    // Сумма всех элементов матрицы
    public static int sumMatrix(int[][] matrix) {
        int sum = 0;
        for (int[] row : matrix) {
            for (int cell : row) {
                sum += cell;
            }
        }
        return sum;
    }
    
    // Транспонирование матрицы
    public static int[][] transpose(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[][] result = new int[cols][rows];
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                result[j][i] = matrix[i][j];
            }
        }
        return result;
    }
    
    // Поиск в двумерном массиве
    public static int[] findElement(int[][] matrix, int target) {
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                if (matrix[i][j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{-1, -1}; // Не найдено
    }
}
```

Советы и лучшие практики

Производительность

```java
public class PerformanceTips {
    
    // Кэширование длины массива для больших циклов
    public static void cachedLength() {
        int[] largeArray = new int[1000000];
        
        // Хорошо - длина кэшируется
        for (int i = 0, n = largeArray.length; i < n; i++) {
            largeArray[i] = i;
        }
    }
    
    // Минимизация вложенных циклов
    public static void optimizeNestedLoops() {
        int[][] matrix = new int[1000][1000];
        
        // Предпочтительный порядок итераций (по строкам)
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                matrix[i][j] = i + j;
            }
        }
    }
}
```

Обработка ошибок

```java
public class ErrorHandling {
    
    // Безопасный доступ к массиву
    public static void safeArrayAccess(int[] array, int index) {
        if (array == null) {
            throw new IllegalArgumentException("Массив не может быть null");
        }
        if (index < 0 || index >= array.length) {
            throw new ArrayIndexOutOfBoundsException(
                "Индекс " + index + " выходит за границы массива"
            );
        }
        System.out.println(array[index]);
    }
    
    // Проверка перед итерацией
    public static void safeIteration(int[] array) {
        if (array != null) {
            for (int element : array) {
                System.out.println(element);
            }
        }
    }
}
```

Ключевые моменты для запоминания

1. Массивы:
   · Фиксированный размер после создания
   · Индексация с 0
   · Поддержка многомерных и зубчатых массивов
2. Циклы:
   · for - когда известно количество итераций
   · while - когда условие может измениться непредсказуемо
   · do-while - минимум одна итерация гарантирована
   · for-each - упрощенная итерация по коллекциям
3. Управление:
   · break - выход из цикла
   · continue - переход к следующей итерации
   · Метки - управление вложенными циклами
4. Лучшие практики:
   · Всегда проверяйте границы массива
   · Используйте for-each когда возможно
   · Кэшируйте длину массива в больших циклах
   · Избегайте глубоко вложенных циклов

https://codingbat.com/doc/java-array-loops.html