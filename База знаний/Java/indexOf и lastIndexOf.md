indexOf и lastIndexOf в Java

Обзор

indexOf и lastIndexOf - методы для поиска символов и подстрок в строках. Оба метода возвращают индекс найденного элемента или -1, если элемент не найден.

indexOf - поиск первого вхождения

Сигнатуры методов

```java
// Поиск символа
int indexOf(int ch)
int indexOf(int ch, int fromIndex)

// Поиск подстроки
int indexOf(String str)
int indexOf(String str, int fromIndex)
```

Параметры

· ch - символ для поиска (передается как int)
· fromIndex - индекс, с которого начинать поиск
· str - подстрока для поиска

Возвращаемое значение

· Индекс первого вхождения или -1

Примеры использования

```java
String text = "Hello, World! Hello, Java!";

// Поиск символа
int firstComma = text.indexOf(','); // 5
int firstL = text.indexOf('l'); // 2

// Поиск с указанием начальной позиции
int secondComma = text.indexOf(',', 6); // 18

// Поиск подстроки
int firstHello = text.indexOf("Hello"); // 0
int secondHello = text.indexOf("Hello", 1); // 14

// Проверка наличия
boolean hasWorld = text.indexOf("World") != -1; // true
```

lastIndexOf - поиск последнего вхождения

Сигнатуры методов

```java
// Поиск символа
int lastIndexOf(int ch)
int lastIndexOf(int ch, int fromIndex)

// Поиск подстроки
int lastIndexOf(String str)
int lastIndexOf(String str, int fromIndex)
```

Параметры

· ch - символ для поиска
· fromIndex - индекс, до которого вести поиск (поиск идет backwards)
· str - подстрока для поиска

Возвращаемое значение

· Индекс последнего вхождения или -1

Примеры использования

```java
String text = "Hello, World! Hello, Java!";

// Поиск символа
int lastComma = text.lastIndexOf(','); // 18
int lastL = text.lastIndexOf('l'); // 16

// Поиск с указанием позиции
int commaBeforePos = text.lastIndexOf(',', 10); // 5

// Поиск подстроки
int lastHello = text.lastIndexOf("Hello"); // 14
int lastHelloBeforePos = text.lastIndexOf("Hello", 10); // 0
```

Особенности поведения

Работа с fromIndex

```java
String str = "abcabc";

// indexOf - поиск вперед от указанной позиции
str.indexOf('b', 2); // 4 (начинает с индекса 2)

// lastIndexOf - поиск назад от указанной позиции
str.lastIndexOf('b', 3); // 1 (ищет до индекса 3)
```

Сравнение indexOf и lastIndexOf

```java
String example = "programming";

System.out.println(example.indexOf('g'));  // 3
System.out.println(example.lastIndexOf('g')); // 10

System.out.println(example.indexOf("pro"));  // 0
System.out.println(example.lastIndexOf("pro")); // 0
```

Практические применения

Извлечение частей строки

```java
String email = "user@example.com";

// Извлечение имени пользователя
int atIndex = email.indexOf('@');
String username = email.substring(0, atIndex);

// Извлечение домена
String domain = email.substring(atIndex + 1);
```

Поиск всех вхождений

```java
public static List<Integer> findAllOccurrences(String text, String search) {
    List<Integer> positions = new ArrayList<>();
    int index = text.indexOf(search);
    
    while (index != -1) {
        positions.add(index);
        index = text.indexOf(search, index + 1);
    }
    
    return positions;
}

// Использование
String text = "Java is fun. Java is powerful.";
List<Integer> positions = findAllOccurrences(text, "Java");
// [0, 13]
```

Валидация и проверки

```java
public static boolean isValidFileName(String fileName) {
    return fileName.indexOf('/') == -1 && 
           fileName.indexOf('\\') == -1 &&
           fileName.indexOf(':') == -1;
}

public static boolean containsMultiple(String text, String word) {
    int first = text.indexOf(word);
    return first != -1 && text.indexOf(word, first + 1) != -1;
}
```

Особенности обработки

Пустые строки

```java
String str = "hello";

// Поиск пустой строки всегда возвращает 0
str.indexOf(""); // 0
str.lastIndexOf(""); // 5 (длина строки)
```

Символы за пределами Basic Multilingual Plane

```java
String emoji = "Hello 😀 World";

// Для символов BMP (Basic Multilingual Plane)
emoji.indexOf('H'); // 0

// Для surrogate pairs (смайлики и т.д.)
// Правильный способ - использовать codePoint методы
int smileyIndex = -1;
for (int i = 0; i < emoji.length(); i++) {
    if (Character.isHighSurrogate(emoji.charAt(i))) {
        int codePoint = emoji.codePointAt(i);
        if (codePoint == 0x1F600) { // 😀
            smileyIndex = i;
            break;
        }
    }
}
```

Производительность

· Временная сложность: В худшем случае O(n*m), где n - длина строки, m - длина искомой подстроки
· Оптимизация: Для частых поисков в одной строке рассмотрите использование String.indexOf с указанием начальной позиции вместо регулярных выражений

Связанные методы

· contains() - проверка наличия подстроки
· startsWith() / endsWith() - проверка начала/конца строки
· matches() - проверка по регулярному выражению

Ссылки

· [[String методы в Java]]
· [[Работа со строками в Java]]
· [[Поиск и замена в строках]]

https://codingbat.com/doc/java-string-indexof-parsing.html