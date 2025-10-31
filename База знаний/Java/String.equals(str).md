String.equals(Object obj)

Сигнатура метода

```java
public boolean equals(Object anObject)
```

Назначение

Сравнивает текущую строку с указанным объектом на содержательное равенство. Проверяет, имеет ли переданный объект ту же последовательность символов, что и текущая строка.

Принцип работы

1. Проверка ссылки: если объект тот же самый (та же ссылка), возвращает true
2. Проверка типа: если объект не является строкой, возвращает false
3. Посимвольное сравнение: сравнивает каждый символ двух строк

Реализация

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

Особенности поведения

· Чувствительность к регистру: "Java".equals("java") → false
· Null-безопасность: "text".equals(null) → false
· Сравнение с не-строками: "text".equals(123) → false

Примеры использования

```java
String str1 = "Hello";
String str2 = "Hello";
String str3 = "HELLO";
String str4 = new String("Hello");

System.out.println(str1.equals(str2));     // true
System.out.println(str1.equals(str3));     // false
System.out.println(str1.equals(str4));     // true
System.out.println(str1.equals(null));     // false
System.out.println(str1.equals("Hello"));  // true
```

Отличие от оператора ==

```java
String a = "hello";
String b = "hello";
String c = new String("hello");

a.equals(b);    // true - одинаковое содержимое
a == b;         // true - кэширование в пуле строк
a.equals(c);    // true - одинаковое содержимое  
a == c;         // false - разные объекты в памяти
```

Рекомендации по использованию

1. Всегда используйте equals() для сравнения строк по содержимому
2. Проверяйте на null первым аргументом чтобы избежать NPE:
   ```java
   if (myString != null && myString.equals("expected")) {...}
   // Или лучше:
   if ("expected".equals(myString)) {...}
   ```
3. Для case-insensitive сравнения используйте equalsIgnoreCase()

Связанные методы

· [[String.equalsIgnoreCase]] - сравнение без учета регистра
· [[String.compareTo]] - лексикографическое сравнение
· [[String.contentEquals]] - сравнение с CharSequence
· [[Objects.equals]] - null-безопасное сравнение

Производительность

· В лучшем случае: O(1) - при совпадении ссылок
· В худшем случае: O(n) - посимвольное сравнение
· Оптимизирован для быстрого сравнения (проверка длины, побайтовое сравнение)

---

Теги: #java/string #java/methods #string-comparison
Связанные: [[String Pool]], [[String Comparison]], [[Object.equals]]