## Структурные паттерны

Структурные паттерны проектирования определяют способы построения отношений между классами и объектами для формирования больших структур. Эти паттерны часто используются для объединения объектов, чтобы создать новые функциональные возможности, не изменяя внутреннюю структуру классов. Рассмотрим основные структурные паттерны с примерами их реализации на Java.

---

### 1. **Adapter (Адаптер)**

**Описание:** Адаптер преобразует интерфейс одного класса в интерфейс, ожидаемый клиентом. Это позволяет объектам с несовместимыми интерфейсами работать вместе.

**Пример:** У нас есть класс, который воспроизводит аудио в формате MP3, а мы хотим добавить поддержку других форматов, например, WAV.

```java
// Интерфейс, ожидаемый клиентом
interface MediaPlayer {
    void play(String audioType, String fileName);
}

// Реализация MediaPlayer, которая воспроизводит MP3 файлы
class Mp3Player implements MediaPlayer {
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing mp3 file. Name: " + fileName);
        } else {
            System.out.println("Invalid media type: " + audioType);
        }
    }
}

// Адаптер, который позволяет проигрывать WAV-файлы
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedMediaPlayer;

    public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("wav")) {
            advancedMediaPlayer = new WavPlayer();
        }
    }

    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("wav")) {
            advancedMediaPlayer.playWav(fileName);
        }
    }
}

// Дополнительный интерфейс для WAV плеера
interface AdvancedMediaPlayer {
    void playWav(String fileName);
}

// Реализация WAV плеера
class WavPlayer implements AdvancedMediaPlayer {
    public void playWav(String fileName) {
        System.out.println("Playing wav file. Name: " + fileName);
    }
}

// Клиентский класс
class AudioPlayer implements MediaPlayer {
    private MediaAdapter mediaAdapter;

    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing mp3 file. Name: " + fileName);
        } else if (audioType.equalsIgnoreCase("wav")) {
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        } else {
            System.out.println("Invalid media type: " + audioType);
        }
    }
}
```

**Использование:**
```java
AudioPlayer player = new AudioPlayer();
player.play("mp3", "song.mp3");
player.play("wav", "song.wav");
```

---

### 2. **Bridge (Мост)**

**Описание:** Мост разделяет абстракцию и реализацию, позволяя изменять их независимо друг от друга.

**Пример:** Пусть у нас есть две формы (Круг и Квадрат) и несколько цветов (Красный и Зеленый). Мы хотим иметь возможность создавать комбинации форм и цветов.

```java
// Интерфейс цвета
interface Color {
    void applyColor();
}

class RedColor implements Color {
    public void applyColor() {
        System.out.println("Applying red color");
    }
}

class GreenColor implements Color {
    public void applyColor() {
        System.out.println("Applying green color");
    }
}

// Абстракция формы
abstract class Shape {
    protected Color color;

    public Shape(Color color) {
        this.color = color;
    }

    abstract void draw();
}

class Circle extends Shape {
    public Circle(Color color) {
        super(color);
    }

    public void draw() {
        System.out.print("Circle drawn. ");
        color.applyColor();
    }
}

class Square extends Shape {
    public Square(Color color) {
        super(color);
    }

    public void draw() {
        System.out.print("Square drawn. ");
        color.applyColor();
    }
}
```

**Использование:**
```java
Shape redCircle = new Circle(new RedColor());
Shape greenSquare = new Square(new GreenColor());

redCircle.draw();    // Вывод: Circle drawn. Applying red color
greenSquare.draw();  // Вывод: Square drawn. Applying green color
```

---

### 3. **Composite (Компоновщик)**

**Описание:** Компоновщик объединяет объекты в древовидные структуры для представления иерархий часть-целое. Это позволяет клиентам обрабатывать отдельные объекты и группы объектов одинаково.

**Пример:** Представим организацию, где есть менеджеры и сотрудники. Каждый менеджер может управлять другими сотрудниками или менеджерами.

```java
import java.util.ArrayList;
import java.util.List;

// Базовый класс Employee
class Employee {
    private String name;
    private String position;
    private List<Employee> subordinates;

    public Employee(String name, String position) {
        this.name = name;
        this.position = position;
        subordinates = new ArrayList<>();
    }

    public void add(Employee e) {
        subordinates.add(e);
    }

    public void remove(Employee e) {
        subordinates.remove(e);
    }

    public List<Employee> getSubordinates() {
        return subordinates;
    }

    public String toString() {
        return "Employee: [Name: " + name + ", Position: " + position + "]";
    }
}
```

**Использование:**
```java
Employee ceo = new Employee("John", "CEO");

Employee headSales = new Employee("Robert", "Head of Sales");
Employee headMarketing = new Employee("Michel", "Head of Marketing");

Employee salesExecutive1 = new Employee("Laura", "Sales Executive");
Employee salesExecutive2 = new Employee("Bob", "Sales Executive");

ceo.add(headSales);
ceo.add(headMarketing);

headSales.add(salesExecutive1);
headSales.add(salesExecutive2);

// Печать всей иерархии
System.out.println(ceo);
for (Employee headEmployee : ceo.getSubordinates()) {
    System.out.println(" " + headEmployee);
    for (Employee employee : headEmployee.getSubordinates()) {
        System.out.println("   " + employee);
    }
}
```

---

### 4. **Decorator (Декоратор)**

**Описание:** Декоратор динамически добавляет объекту новые обязанности (поведение). Полезен, когда невозможно наследоваться, и нужно расширять объект.

**Пример:** Создадим базовый интерфейс для кофе и добавим дополнительные "декораторы", такие как добавка молока и сахара.

```java
// Базовый интерфейс кофе
interface Coffee {
    String getDescription();
    double getCost();
}

class SimpleCoffee implements Coffee {
    public String getDescription() {
        return "Simple coffee";
    }

    public double getCost() {
        return 5.0;
    }
}

// Декоратор добавления молока
class MilkDecorator implements Coffee {
    private Coffee coffee;

    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    public String getDescription() {
        return coffee.getDescription() + ", milk";
    }

    public double getCost() {
        return coffee.getCost() + 1.5;
    }
}

// Декоратор добавления сахара
class SugarDecorator implements Coffee {
    private Coffee coffee;

    public SugarDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    public String getDescription() {
        return coffee.getDescription() + ", sugar";
    }

    public double getCost() {
        return coffee.getCost() + 0.5;
    }
}
```

**Использование:**
```java
Coffee coffee = new SimpleCoffee();
System.out.println(coffee.getDescription() + " $" + coffee.getCost());

coffee = new MilkDecorator(coffee);
System.out.println(coffee.getDescription() + " $" + coffee.getCost());

coffee = new SugarDecorator(coffee);
System.out.println(coffee.getDescription() + " $" + coffee.getCost());
```

---

### 5. **Facade (Фасад)**

**Описание:** Фасад предоставляет упрощенный интерфейс к сложной системе классов, помогая скрыть сложность и уменьшить зависимости.

**Пример:** Упрощение работы с подсистемой банка через фасад.

```java
class BankAccountFacade {
    private int accountNumber;
    private int securityCode;

    public BankAccountFacade(int accountNumber, int securityCode) {
        this.accountNumber = accountNumber;
        this.securityCode = securityCode;
    }

    public void withdrawCash(double amount) {
        // Проверка аккаунта, проверка кода и другие действия
        System.out.println("Withdrawing " + amount + " from account.");
    }

    public void depositCash(double amount) {
        // Проверка аккаунта и другие действия
        System.out.println("Depositing " + amount + " into account.");
    }
}
```

**Использование:**
```java
BankAccountFacade bankAccount = new BankAccountFacade(123456, 1234);
bankAccount.withdrawCash(100.0);
bankAccount.depositCash(50.0);
```

---

Эти паттерны позволяют оптимально строить большие системы и эффективно управлять их компонентами.