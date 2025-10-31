String.toUpperCase() в Java

Обзор

toUpperCase() — метод класса String, который преобразует все символы строки в верхний регистр.

Сигнатуры метода

```java
// Базовая версия (использует локаль по умолчанию)
public String toUpperCase()

// Версия с указанием локали
public String toUpperCase(Locale locale)
```

Параметры

· locale (опционально) - определяет правила преобразования для конкретного языка и региона
  · Тип: java.util.Locale
  · Если не указан, используется локаль по умолчанию JVM

Возвращаемое значение

· Возвращает новую строку, где все символы преобразованы в верхний регистр
· Исходная строка не изменяется (строки в Java неизменяемы)

Особенности преобразования

Латиница

```java
"hello".toUpperCase();        // "HELLO"
"Java Programming".toUpperCase(); // "JAVA PROGRAMMING"
```

Кириллица

```java
"привет".toUpperCase();       // "ПРИВЕТ"
"ёлка".toUpperCase();         // "ЁЛКА" (буква Ё сохраняется)
```

Специальные случаи

```java
"123abc!@#".toUpperCase();    // "123ABC!@#" - только буквы преобразуются
"".toUpperCase();             // "" - пустая строка остается пустой
```

Важность указания локали

Без локали (потенциальные проблемы)

```java
"istanbul".toUpperCase(); // Результат зависит от локали системы
```

С локалью (предсказуемый результат)

```java
import java.util.Locale;

// Турецкий язык - особые правила преобразования
"istanbul".toUpperCase(Locale.forLanguageTag("tr")); // "İSTANBUL"

// Английский язык
"istanbul".toUpperCase(Locale.ENGLISH); // "ISTANBUL"
```

Рекомендуемые практики

1. Использование явной локали

```java
// Для строк, не зависящих от локали (идентификаторы, ключи)
string.toUpperCase(Locale.ROOT);

// Для пользовательского интерфейса
string.toUpperCase(Locale.getDefault());

// Для конкретного языка
string.toUpperCase(Locale.ENGLISH);
```

2. Сравнение строк без учета регистра

```java
String str1 = "Hello";
String str2 = "HELLO";

// Правильный способ
boolean equal = str1.equalsIgnoreCase(str2);
// Или
boolean equal = str1.toUpperCase(Locale.ROOT).equals(str2.toUpperCase(Locale.ROOT));
```

Примеры использования

Базовые примеры

```java
public class UpperCaseExamples {
    public static void main(String[] args) {
        // Простое преобразование
        String original = "Hello World!";
        String upper = original.toUpperCase();
        System.out.println(upper); // "HELLO WORLD!"
        
        // Проверка ввода
        String userInput = "yes";
        if (userInput.toUpperCase().equals("YES")) {
            System.out.println("Пользователь согласен");
        }
        
        // Нормализация данных
        String[] names = {"alice", "BOB", "Charlie"};
        for (String name : names) {
            System.out.println(name.toUpperCase());
        }
    }
}
```

Работа с разными локалями

```java
import java.util.Locale;

public class LocaleExamples {
    public static void main(String[] args) {
        String text = "iıİi";
        
        // Турецкая локаль
        System.out.println(text.toUpperCase(new Locale("tr", "TR")));
        // Результат: "IİİI"
        
        // Английская локаль  
        System.out.println(text.toUpperCase(Locale.ENGLISH));
        // Результат: "IıII"
    }
}
```

Исключения

· NullPointerException - если вызвать метод на null-ссылке

```java
String nullString = null;
nullString.toUpperCase(); // NullPointerException
```

Производительность

· Создает новую строку, что может быть затратно при частом использовании в циклах
· Для многократных преобразований考虑使用 StringBuilder

```java
// Неэффективно
String result = "";
for (String word : words) {
    result += word.toUpperCase();
}

// Эффективно
StringBuilder sb = new StringBuilder();
for (String word : words) {
    sb.append(word.toUpperCase());
}
String result = sb.toString();
```

Связанные методы

· String.toLowerCase() - преобразование в нижний регистр
· String.equalsIgnoreCase() - сравнение без учета регистра
· Character.toUpperCase() - преобразование отдельного символа

Примечания

1. Метод учитывает специфичные для локали правила преобразования
2. Все не-буквенные символы остаются неизменными
3. Для некоторых символов преобразование может быть неоднозначным
4. Рекомендуется всегда использовать версию с локалью для предсказуемости

---

Теги: #java #string #methods #localization