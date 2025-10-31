Документация Math.abs в Java для Obsidian
верни исчерпывающую документацию по  Math.abs в java для добавления в личную базу знаний  в  obsidian 
Мы создадим документацию для метода Math.abs в Java в формате, подходящем для Obsidian.
В Obsidian мы можем использовать Markdown с возможностью добавления метаданных (frontmatter) для организации базы знаний.

Структура документации может включать:

Заголовок

Короткое описание

Сигнатуры метода (для разных типов)

Подробное описание

Примеры кода

Примечания (если есть)

Связанные методы или темы

Поскольку Math.abs перегружен для разных числовых типов, мы опишем каждую перегрузку.

Давайте создадим такой документ.

Math.abs в Java
Общее описание
Math.abs() — статический метод класса Math, который возвращает абсолютное значение (модуль) переданного аргумента. Абсолютное значение числа представляет его величину без учета знака.

Сигнатуры метода
java
public static int abs(int a)
public static long abs(long a)
public static float abs(float a)
public static double abs(double a)
Параметры
a — числовой аргумент (int, long, float или double)

Возвращаемое значение
Возвращает абсолютное значение аргумента:

Для положительных чисел и нуля — само число

Для отрицательных чисел — число с противоположным знаком

Для Integer.MIN_VALUE возвращает Integer.MIN_VALUE (отрицательное значение)

Для Long.MIN_VALUE возвращает Long.MIN_VALUE (отрицательное значение)

Для NaN возвращает NaN

Для бесконечности возвращает POSITIVE_INFINITY

Особые случаи
Входное значение	Результат
Положительное число	То же число
Отрицательное число	Число с противоположным знаком
0	0
Integer.MIN_VALUE	Integer.MIN_VALUE (остается отрицательным)
Long.MIN_VALUE	Long.MIN_VALUE (остается отрицательным)
NaN	NaN
±Infinity	POSITIVE_INFINITY
Примеры использования
java
// Целые числа
Math.abs(10);     // → 10
Math.abs(-10);    // → 10
Math.abs(0);      // → 0

// Длинные целые
Math.abs(100L);   // → 100L
Math.abs(-100L);  // → 100L

// Числа с плавающей точкой
Math.abs(3.14);   // → 3.14
Math.abs(-3.14);  // → 3.14
Math.abs(0.0);    // → 0.0

// Особые случаи
Math.abs(Integer.MIN_VALUE);  // → -2147483648
Math.abs(Float.NaN);          // → NaN
Math.abs(Double.NEGATIVE_INFINITY); // → Infinity
Важные замечания
Критический случай с Integer.MIN_VALUE
java
// Опасный пример - может привести к неожиданным результатам
int minValue = Integer.MIN_VALUE;
int absValue = Math.abs(minValue); // → -2147483648

// Безопасная проверка
public static int safeAbs(int value) {
    if (value == Integer.MIN_VALUE) {
        throw new ArithmeticException("Integer overflow in absolute value");
    }
    return Math.abs(value);
}
Производительность
Метод обычно компилируется в нативную инструкцию процессора, поэтому очень эффективен.

Альтернативы и родственные методы
Math.max(a, -a) — альтернативный способ для некоторых сценариев

Math.signum() — возвращает знак числа

StrictMath.abs() — гарантирует одинаковое поведение на всех платформах

Практическое применение
java
// Расчет расстояния между точками
public static int distance(int x1, int x2) {
    return Math.abs(x1 - x2);
}

// Проверка допустимого диапазона
public static boolean isInTolerance(int expected, int actual, int tolerance) {
    return Math.abs(expected - actual) <= tolerance;
}

// Безопасное преобразование с проверкой переполнения
public static int safeAbsolute(int value) {
    if (value == Integer.MIN_VALUE) {
        return Integer.MAX_VALUE; // или обработка ошибки
    }
    return Math.abs(value);
}
Связанные теги
#java #math #абсолютное-значение #числовые-операции #методы-Math


https://chat.deepseek.com/a/chat/s/b8d72c60-4060-4680-812f-66dacfaab68c