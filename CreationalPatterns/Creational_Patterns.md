## Порождающие паттерны 
в Java предназначены для гибкого создания объектов, упрощения их конфигурации и управления, что особенно полезно при проектировании систем с множеством зависимостей. Ниже приведены основные порождающие паттерны с их реализацией и примерами использования.

---

### 1. Singleton (Одиночка)

**Описание:** Гарантирует, что у класса будет только один экземпляр и предоставляет к нему глобальную точку доступа.

**Пример использования:** 
   - Для логирования в приложении, когда нужен единый объект для записи логов.
   - Управление подключением к базе данных.

**Реализация Singleton:**

```java
public class Singleton {
    // Единственный экземпляр класса
    private static Singleton instance;

    // Приватный конструктор запрещает создание экземпляров снаружи
    private Singleton() {}

    // Метод для получения экземпляра
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    // Пример метода, доступного через Singleton
    public void log(String message) {
        System.out.println("Log: " + message);
    }
}
```

**Использование:**

```java
public class Main {
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();
        singleton.log("Сообщение от Singleton");
    }
}
```

---

### 2. Factory Method (Фабричный метод)

**Описание:** Создаёт объекты с помощью метода-фабрики, позволяя подклассам определять тип создаваемых объектов.

**Пример использования:**
   - Создание объектов с разной реализацией интерфейса, когда заранее неизвестно, какой именно тип потребуется.
   - Создание различных видов документов в текстовом редакторе.

**Реализация Factory Method:**

```java
// Общий интерфейс продукта
interface Product {
    void use();
}

// Конкретные реализации продуктов
class ConcreteProductA implements Product {
    @Override
    public void use() {
        System.out.println("Using Product A");
    }
}

class ConcreteProductB implements Product {
    @Override
    public void use() {
        System.out.println("Using Product B");
    }
}

// Фабрика, которая создаёт продукты
abstract class Creator {
    public abstract Product factoryMethod();
}

class ConcreteCreatorA extends Creator {
    @Override
    public Product factoryMethod() {
        return new ConcreteProductA();
    }
}

class ConcreteCreatorB extends Creator {
    @Override
    public Product factoryMethod() {
        return new ConcreteProductB();
    }
}
```

**Использование:**

```java
public class Main {
    public static void main(String[] args) {
        Creator creatorA = new ConcreteCreatorA();
        Product productA = creatorA.factoryMethod();
        productA.use();

        Creator creatorB = new ConcreteCreatorB();
        Product productB = creatorB.factoryMethod();
        productB.use();
    }
}
```

---

### 3. Abstract Factory (Абстрактная фабрика)

**Описание:** Позволяет создавать семейства связанных объектов без привязки к их конкретным классам.

**Пример использования:** 
   - Для создания графических интерфейсов, где стиль кнопок и полей ввода зависит от операционной системы.
   - Создание компонентов для различных типов баз данных.

**Реализация Abstract Factory:**

```java
// Интерфейсы для продуктов
interface Button {
    void click();
}

interface TextField {
    void input(String text);
}

// Реализация продуктов для Windows
class WindowsButton implements Button {
    @Override
    public void click() {
        System.out.println("Windows Button clicked!");
    }
}

class WindowsTextField implements TextField {
    @Override
    public void input(String text) {
        System.out.println("Input into Windows TextField: " + text);
    }
}

// Реализация продуктов для MacOS
class MacButton implements Button {
    @Override
    public void click() {
        System.out.println("Mac Button clicked!");
    }
}

class MacTextField implements TextField {
    @Override
    public void input(String text) {
        System.out.println("Input into Mac TextField: " + text);
    }
}

// Абстрактная фабрика для создания продуктов
interface GUIFactory {
    Button createButton();
    TextField createTextField();
}

// Реализация фабрик для разных ОС
class WindowsFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WindowsButton();
    }

    @Override
    public TextField createTextField() {
        return new WindowsTextField();
    }
}

class MacFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new MacButton();
    }

    @Override
    public TextField createTextField() {
        return new MacTextField();
    }
}
```

**Использование:**

```java
public class Main {
    public static void main(String[] args) {
        GUIFactory factory = new WindowsFactory(); // Либо new MacFactory();
        Button button = factory.createButton();
        TextField textField = factory.createTextField();
        
        button.click();
        textField.input("Hello World");
    }
}
```

---

### 4. Builder (Строитель)

**Описание:** Разделяет процесс создания сложного объекта от его представления, позволяя создавать разные представления объекта.

**Пример использования:** 
   - Создание сложных объектов, таких как машины, которые могут иметь различные конфигурации.
   - Конструирование HTML- или XML-документов.

**Реализация Builder:**

```java
class Car {
    private String engine;
    private int wheels;
    private String color;

    public Car(String engine, int wheels, String color) {
        this.engine = engine;
        this.wheels = wheels;
        this.color = color;
    }

    @Override
    public String toString() {
        return "Car [engine=" + engine + ", wheels=" + wheels + ", color=" + color + "]";
    }
}

class CarBuilder {
    private String engine;
    private int wheels;
    private String color;

    public CarBuilder setEngine(String engine) {
        this.engine = engine;
        return this;
    }

    public CarBuilder setWheels(int wheels) {
        this.wheels = wheels;
        return this;
    }

    public CarBuilder setColor(String color) {
        this.color = color;
        return this;
    }

    public Car build() {
        return new Car(engine, wheels, color);
    }
}
```

**Использование:**

```java
public class Main {
    public static void main(String[] args) {
        Car car = new CarBuilder()
                .setEngine("V8")
                .setWheels(4)
                .setColor("Red")
                .build();
        
        System.out.println(car);
    }
}
```

---

### 5. Prototype (Прототип)

**Описание:** Создаёт новые объекты путем клонирования существующих. Полезен, когда создание объекта напрямую может быть дорогим или сложным.

**Пример использования:** 
   - Клонирование объектов с настройками по умолчанию, которые потом можно слегка изменять.
   - Создание различных версий документа на основе существующего шаблона.

**Реализация Prototype:**

```java
class Prototype implements Cloneable {
    private String field;

    public Prototype(String field) {
        this.field = field;
    }

    public void setField(String field) {
        this.field = field;
    }

    public String getField() {
        return field;
    }

    @Override
    protected Prototype clone() throws CloneNotSupportedException {
        return (Prototype) super.clone();
    }
}
```

**Использование:**

```java
public class Main {
    public static void main(String[] args) {
        try {
            Prototype original = new Prototype("Original");
            Prototype clone = original.clone();
            clone.setField("Clone");
            
            System.out.println("Original field: " + original.getField());
            System.out.println("Clone field: " + clone.getField());
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
}
```
##
### Эти порождающие паттерны позволяют гибко управлять созданием объектов и создавать сложные структуры с разной степенью детализации и расширяемости.