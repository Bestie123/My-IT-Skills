
# Основные принципы ООП в Java

## Содержание ^sodergn
- [Введение](#^vvedenie-8k9p)
- [Принципы ООП](#^principy-3n8t)
  - [Инкапсуляция](#^inkapsulyaciya-5q1w)
  - [Наследование](#^nasledovanie-4m7v)
  - [Полиморфизм](#^polimorfizm-2k9p)
  - [Абстракция](#^abstrakciya-1b2c)
- [Практические примеры](#^primery-d3f4)
- [Заключение](#^zakluchenie-7g8h)

## Введение ^vvedenie-8k9p
Объектно-ориентированное программирование (ООП) — это парадигма программирования, основанная на концепции объектов, которые содержат данные и методы для работы с этими данными. Java — полностью объектно-ориентированный язык, построенный на четырех основных принципах ООП.

[↑ К содержанию](#^sodergn)

## Принципы ООП ^principy-3n8t
В Java выделяют четыре основных принципа объектно-ориентированного программирования, которые обеспечивают модульность, переиспользуемость и поддерживаемость кода.

### Инкапсуляция ^inkapsulyaciya-5q1w
Инкапсуляция — это механизм сокрытия внутренней реализации объекта и предоставления контролируемого доступа к данным через публичные методы.

**Ключевые особенности:**
- Сокрытие данных с помощью модификаторов доступа (`private`, `protected`, `public`)
- Использование геттеров и сеттеров для контролируемого доступа
- Защита внутреннего состояния объекта

```java
public class BankAccount {
    private String accountNumber;
    private double balance;
    
    // Конструктор
    public BankAccount(String accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }
    
    // Геттер для баланса
    public double getBalance() {
        return balance;
    }
    
    // Сеттер с проверкой
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    // Метод для снятия средств
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
}
```

<small>В этом примере поле `balance` защищено модификатором `private`, что предотвращает прямой доступ извне класса.</small>

[↑ К содержанию](#^sodergn)

### Наследование ^nasledovanie-4m7v
Наследование позволяет создавать новые классы на основе существующих, перенимая их свойства и поведение.

**Основные понятия:**
- **Родительский класс (суперкласс)** — базовый класс
- **Дочерний класс (подкласс)** — класс, который наследует от родительского
- Ключевое слово `extends` для наследования
- Конструктор родительского класса вызывается через `super()`

```java
// Родительский класс
class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void eat() {
        System.out.println(name + " ест");
    }
    
    public void sleep() {
        System.out.println(name + " спит");
    }
}

// Дочерний класс
class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age); // Вызов конструктора родительского класса
        this.breed = breed;
    }
    
    public void bark() {
        System.out.println(name + " лает: Гав-гав!");
    }
    
    @Override
    public void eat() {
        System.out.println(name + " ест корм для собак");
    }
}
```

<small>Ключевое слово `@Override` указывает, что метод переопределен в дочернем классе.</small>

[↑ К содержанию](#^sodergn)

### Полиморфизм ^polimorfizm-2k9p
Полиморфизм позволяет объектам разных классов обрабатываться как объекты общего родительского класса, но при этом сохранять свое уникальное поведение.

**Типы полиморфизма в Java:**
- **Переопределение методов** (Runtime polymorphism)
- **Перегрузка методов** (Compile-time polymorphism)

```java
// Пример переопределения методов
class Shape {
    public void draw() {
        System.out.println("Рисую фигуру");
    }
    
    public double calculateArea() {
        return 0.0;
    }
}

class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Рисую круг радиусом " + radius);
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("Рисую прямоугольник " + width + "x" + height);
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
}

// Пример использования полиморфизма
public class PolymorphismExample {
    public static void main(String[] args) {
        Shape[] shapes = new Shape[2];
        shapes[0] = new Circle(5.0);
        shapes[1] = new Rectangle(4.0, 6.0);
        
        for (Shape shape : shapes) {
            shape.draw(); // Полиморфный вызов
            System.out.println("Площадь: " + shape.calculateArea());
        }
    }
}
```

<small>Полиморфизм позволяет работать с разными типами объектов через общий интерфейс.</small>

[↑ К содержанию](#^sodergn)

### Абстракция ^abstrakciya-1b2c
Абстракция позволяет скрыть сложную реализацию и показать только необходимые функциональные возможности.

**Способы реализации абстракции в Java:**
- **Абстрактные классы** (`abstract class`)
- **Интерфейсы** (`interface`)

```java
// Абстрактный класс
abstract class Vehicle {
    protected String model;
    protected int speed;
    
    public Vehicle(String model, int speed) {
        this.model = model;
        this.speed = speed;
    }
    
    // Абстрактный метод (без реализации)
    public abstract void move();
    
    // Конкретный метод
    public void displayInfo() {
        System.out.println("Модель: " + model + ", Скорость: " + speed + " км/ч");
    }
}

// Интерфейс
interface Electric {
    void charge();
    int getBatteryLevel();
}

// Класс, реализующий абстракцию
class ElectricCar extends Vehicle implements Electric {
    private int batteryLevel;
    
    public ElectricCar(String model, int speed, int batteryLevel) {
        super(model, speed);
        this.batteryLevel = batteryLevel;
    }
    
    @Override
    public void move() {
        System.out.println(model + " движется бесшумно на электричестве");
    }
    
    @Override
    public void charge() {
        batteryLevel = 100;
        System.out.println(model + " заряжен до 100%");
    }
    
    @Override
    public int getBatteryLevel() {
        return batteryLevel;
    }
}
```

<small>Абстрактные классы могут содержать как абстрактные, так и конкретные методы, в то время как интерфейсы до Java 8 могли содержать только абстрактные методы.</small>

[↑ К содержанию](#^sodergn)

## Практические примеры ^primery-d3f4
Рассмотрим комплексный пример, демонстрирующий все четыре принципа ООП в действии.

```java
// Абстрактный класс (Абстракция)
abstract class Employee {
    private String name; // Инкапсуляция
    private int id;
    protected double baseSalary;
    
    public Employee(String name, int id, double baseSalary) {
        this.name = name;
        this.id = id;
        this.baseSalary = baseSalary;
    }
    
    // Геттеры и сеттеры (Инкапсуляция)
    public String getName() { return name; }
    public int getId() { return id; }
    
    // Абстрактный метод
    public abstract double calculateSalary();
    
    public void displayInfo() {
        System.out.println("ID: " + id + ", Имя: " + name + ", Зарплата: " + calculateSalary());
    }
}

// Наследование
class Developer extends Employee {
    private String programmingLanguage;
    private int overtimeHours;
    
    public Developer(String name, int id, double baseSalary, String language, int overtime) {
        super(name, id, baseSalary); // Наследование
        this.programmingLanguage = language;
        this.overtimeHours = overtime;
    }
    
    // Полиморфизм через переопределение
    @Override
    public double calculateSalary() {
        return baseSalary + (overtimeHours * 1000); // Доплата за переработку
    }
    
    public void code() {
        System.out.println(getName() + " пишет код на " + programmingLanguage);
    }
}

class Manager extends Employee {
    private double bonus;
    
    public Manager(String name, int id, double baseSalary, double bonus) {
        super(name, id, baseSalary);
        this.bonus = bonus;
    }
    
    // Полиморфизм через переопределение
    @Override
    public double calculateSalary() {
        return baseSalary + bonus;
    }
    
    public void manageTeam() {
        System.out.println(getName() + " управляет командой разработчиков");
    }
}

// Использование всех принципов
public class OOPDemo {
    public static void main(String[] args) {
        Employee[] employees = new Employee[2];
        employees[0] = new Developer("Анна", 1, 50000, "Java", 10);
        employees[1] = new Manager("Иван", 2, 70000, 20000);
        
        // Полиморфизм в действии
        for (Employee emp : employees) {
            emp.displayInfo();
            
            // Проверка типа для вызова специфических методов
            if (emp instanceof Developer) {
                ((Developer) emp).code();
            } else if (emp instanceof Manager) {
                ((Manager) emp).manageTeam();
            }
            System.out.println("---");
        }
    }
}
```

<small>Этот пример демонстрирует, как все четыре принципа ООП работают вместе для создания гибкой и расширяемой архитектуры.</small>

[↑ К содержанию](#^sodergn)

## Заключение ^zakluchenie-7g8h
Четыре принципа ООП в Java — инкапсуляция, наследование, полиморфизм и абстракция — образуют фундамент объектно-ориентированного программирования. Понимание и правильное применение этих принципов позволяет создавать:

- **Модульный код** — легко изменять и расширять
- **Переиспользуемый код** — уменьшение дублирования
- **Поддерживаемый код** — легкое понимание и модификация
- **Надежный код** — защита данных и контролируемое поведение

Освоение этих принципов является ключевым для становления профессиональным Java-разработчиком и создания качественного программного обеспечения.

[↑ К содержанию](#^sodergn)

**Теги:** #java #ооп #программирование #инкапсуляция #наследование #полиморфизм #абстракция
```