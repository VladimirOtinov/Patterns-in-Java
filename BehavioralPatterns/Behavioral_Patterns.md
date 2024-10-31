## Поведенческие паттерны 
Поведенческие паттерны проектирования в объектно-ориентированном программировании помогают описывать эффективные способы взаимодействия между объектами, делая их связи более гибкими и слабыми. Рассмотрим каждый из основных поведенческих паттернов на Java с примерами их применения и реализации.

### 1. Chain of Responsibility (Цепочка обязанностей)

**Идея**: Паттерн «Цепочка обязанностей» позволяет передавать запросы вдоль цепочки обработчиков. Каждый обработчик решает, обработать запрос или передать его дальше по цепочке.

**Пример применения**: Система обработки пользовательских запросов, где разные уровни обработки (например, модераторы, администраторы) проверяют и, возможно, решают запрос.

**Реализация**:
```java
abstract class Handler {
    protected Handler nextHandler;
    public void setNextHandler(Handler nextHandler) {
        this.nextHandler = nextHandler;
    }
    public abstract void handleRequest(String request);
}

class AdminHandler extends Handler {
    public void handleRequest(String request) {
        if (request.equals("admin")) {
            System.out.println("Request handled by Admin.");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

class ModeratorHandler extends Handler {
    public void handleRequest(String request) {
        if (request.equals("moderator")) {
            System.out.println("Request handled by Moderator.");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

// Использование
public class Main {
    public static void main(String[] args) {
        Handler moderator = new ModeratorHandler();
        Handler admin = new AdminHandler();
        moderator.setNextHandler(admin);
        
        moderator.handleRequest("admin"); // Output: Request handled by Admin.
    }
}
```

### 2. Command (Команда)

**Идея**: Паттерн «Команда» инкапсулирует запрос как объект, позволяя вам передавать его как параметр, логировать, отменять и повторять действия.

**Пример применения**: Управление операциями (например, отмена/повтор действий) в редакторе текстов или графики.

**Реализация**:
```java
interface Command {
    void execute();
}

class Light {
    public void turnOn() {
        System.out.println("Light is ON");
    }
    public void turnOff() {
        System.out.println("Light is OFF");
    }
}

class TurnOnCommand implements Command {
    private Light light;
    public TurnOnCommand(Light light) {
        this.light = light;
    }
    public void execute() {
        light.turnOn();
    }
}

class RemoteControl {
    private Command command;
    public void setCommand(Command command) {
        this.command = command;
    }
    public void pressButton() {
        command.execute();
    }
}

// Использование
public class Main {
    public static void main(String[] args) {
        Light light = new Light();
        Command turnOn = new TurnOnCommand(light);
        
        RemoteControl remote = new RemoteControl();
        remote.setCommand(turnOn);
        remote.pressButton(); // Output: Light is ON
    }
}
```

### 3. Interpreter (Интерпретатор)

**Идея**: Паттерн «Интерпретатор» определяет грамматику и интерпретирует выражения на основе этой грамматики.

**Пример применения**: Разработка простого языка или диалекта для создания SQL-подобных запросов.

**Реализация**:
```java
interface Expression {
    boolean interpret(String context);
}

class TerminalExpression implements Expression {
    private String data;
    public TerminalExpression(String data) {
        this.data = data;
    }
    public boolean interpret(String context) {
        return context.contains(data);
    }
}

class OrExpression implements Expression {
    private Expression expr1;
    private Expression expr2;
    public OrExpression(Expression expr1, Expression expr2) {
        this.expr1 = expr1;
        this.expr2 = expr2;
    }
    public boolean interpret(String context) {
        return expr1.interpret(context) || expr2.interpret(context);
    }
}

// Использование
public class Main {
    public static void main(String[] args) {
        Expression isJava = new TerminalExpression("Java");
        Expression isPattern = new TerminalExpression("Pattern");
        Expression isJavaPattern = new OrExpression(isJava, isPattern);

        System.out.println(isJavaPattern.interpret("This is a Java program")); // true
    }
}
```

### 4. Iterator (Итератор)

**Идея**: Паттерн «Итератор» предоставляет последовательный доступ к элементам коллекции, скрывая внутреннее представление.

**Пример применения**: Реализация класса, предоставляющего возможность обойти элементы коллекции (например, ArrayList) без знания её внутренней структуры.

**Реализация**:
```java
class NameRepository implements Iterable<String> {
    private String[] names = {"Alice", "Bob", "Charlie"};

    public Iterator<String> iterator() {
        return new NameIterator();
    }

    private class NameIterator implements Iterator<String> {
        int index;
        public boolean hasNext() {
            return index < names.length;
        }
        public String next() {
            if (this.hasNext()) {
                return names[index++];
            }
            return null;
        }
    }
}

// Использование
public class Main {
    public static void main(String[] args) {
        NameRepository names = new NameRepository();
        for (String name : names) {
            System.out.println(name);
        }
    }
}
```

### 5. Observer (Наблюдатель)

**Идея**: Паттерн «Наблюдатель» создает механизм подписки, который уведомляет подписчиков об изменениях состояния.

**Пример применения**: Подписка на обновления (например, новости или курсы акций).

**Реализация**:
```java
interface Observer {
    void update(String message);
}

class Subscriber implements Observer {
    private String name;
    public Subscriber(String name) {
        this.name = name;
    }
    public void update(String message) {
        System.out.println(name + " received message: " + message);
    }
}

class Publisher {
    private List<Observer> observers = new ArrayList<>();
    public void addObserver(Observer observer) {
        observers.add(observer);
    }
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}

// Использование
public class Main {
    public static void main(String[] args) {
        Publisher publisher = new Publisher();
        Observer user1 = new Subscriber("User1");
        Observer user2 = new Subscriber("User2");

        publisher.addObserver(user1);
        publisher.addObserver(user2);

        publisher.notifyObservers("New update available!");
    }
}
```
---
### Эти примеры показывают основные поведенческие паттерны и их роль в программировании на Java.

---

# Менее популярные паттерны
Существуют поведенческие паттерны, которые менее популярны и применяются реже, но они могут быть полезны в специфических случаях. Эти паттерны включают: **Visitor (Посетитель)**, **Mediator (Посредник)**, **State (Состояние)** и **Memento (Снимок)**. Рассмотрим их по очереди с краткими примерами.

---

### 1. Visitor (Посетитель)

**Идея**: Паттерн «Посетитель» позволяет добавлять новые операции к объектам, не изменяя их структуру. Это полезно, когда нужно выполнять новые действия над объектами, не изменяя их классы.

**Пример применения**: Создание калькулятора для обработки различных типов геометрических фигур, где каждая фигура выполняет операцию через объект-посетитель.

**Реализация**:
```java
interface Shape {
    void accept(Visitor visitor);
}

class Circle implements Shape {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

class Rectangle implements Shape {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

interface Visitor {
    void visit(Circle circle);
    void visit(Rectangle rectangle);
}

class AreaCalculator implements Visitor {
    public void visit(Circle circle) {
        System.out.println("Calculating area for Circle");
    }
    public void visit(Rectangle rectangle) {
        System.out.println("Calculating area for Rectangle");
    }
}

// Использование
public class Main {
    public static void main(String[] args) {
        Shape circle = new Circle();
        Shape rectangle = new Rectangle();
        Visitor areaCalculator = new AreaCalculator();

        circle.accept(areaCalculator);
        rectangle.accept(areaCalculator);
    }
}
```

---

### 2. Mediator (Посредник)

**Идея**: Паттерн «Посредник» управляет взаимодействиями между объектами, уменьшая зависимости между ними. Посредник может выполнять роль централизованного управляющего, что упрощает коммуникацию.

**Пример применения**: Создание чат-комнаты, где пользователи отправляют сообщения через посредника, который управляет всеми взаимодействиями.

**Реализация**:
```java
interface Mediator {
    void sendMessage(String message, User user);
}

class ChatRoomMediator implements Mediator {
    private List<User> users = new ArrayList<>();

    public void addUser(User user) {
        users.add(user);
    }

    public void sendMessage(String message, User user) {
        for (User u : users) {
            if (u != user) {
                u.receive(message);
            }
        }
    }
}

class User {
    private String name;
    private Mediator mediator;

    public User(String name, Mediator mediator) {
        this.name = name;
        this.mediator = mediator;
    }

    public void send(String message) {
        System.out.println(name + " sends: " + message);
        mediator.sendMessage(message, this);
    }

    public void receive(String message) {
        System.out.println(name + " received: " + message);
    }
}

// Использование
public class Main {
    public static void main(String[] args) {
        Mediator mediator = new ChatRoomMediator();

        User user1 = new User("Alice", mediator);
        User user2 = new User("Bob", mediator);
        ((ChatRoomMediator) mediator).addUser(user1);
        ((ChatRoomMediator) mediator).addUser(user2);

        user1.send("Hello, Bob!");
    }
}
```

---

### 3. State (Состояние)

**Идея**: Паттерн «Состояние» позволяет объекту изменять свое поведение при изменении внутреннего состояния. Этот паттерн полезен, когда объект должен менять поведение в зависимости от своего текущего состояния.

**Пример применения**: Управление состоянием заказа (например, «В обработке», «Отправлен», «Доставлен»).

**Реализация**:
```java
interface OrderState {
    void next(Order order);
    void prev(Order order);
    void printStatus();
}

class OrderedState implements OrderState {
    public void next(Order order) {
        order.setState(new ShippedState());
    }
    public void prev(Order order) {
        System.out.println("Order is in its initial state.");
    }
    public void printStatus() {
        System.out.println("Order placed.");
    }
}

class ShippedState implements OrderState {
    public void next(Order order) {
        order.setState(new DeliveredState());
    }
    public void prev(Order order) {
        order.setState(new OrderedState());
    }
    public void printStatus() {
        System.out.println("Order shipped.");
    }
}

class DeliveredState implements OrderState {
    public void next(Order order) {
        System.out.println("Order already delivered.");
    }
    public void prev(Order order) {
        order.setState(new ShippedState());
    }
    public void printStatus() {
        System.out.println("Order delivered.");
    }
}

class Order {
    private OrderState state = new OrderedState();

    public void setState(OrderState state) {
        this.state = state;
    }

    public void previousState() {
        state.prev(this);
    }

    public void nextState() {
        state.next(this);
    }

    public void printStatus() {
        state.printStatus();
    }
}

// Использование
public class Main {
    public static void main(String[] args) {
        Order order = new Order();
        order.printStatus();      // Order placed.
        order.nextState();
        order.printStatus();      // Order shipped.
        order.nextState();
        order.printStatus();      // Order delivered.
    }
}
```

---

### 4. Memento (Снимок)

**Идея**: Паттерн «Снимок» позволяет сохранять состояние объекта и восстанавливать его позже, не нарушая инкапсуляцию. Подходит для создания функциональности «Отмены» и «Повтора» в приложении.

**Пример применения**: Создание функционала отмены для текстового редактора.

**Реализация**:
```java
class TextEditor {
    private String content;

    public void setContent(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }

    public Memento save() {
        return new Memento(content);
    }

    public void restore(Memento memento) {
        content = memento.getContent();
    }

    static class Memento {
        private final String content;
        public Memento(String content) {
            this.content = content;
        }
        public String getContent() {
            return content;
        }
    }
}

// Использование
public class Main {
    public static void main(String[] args) {
        TextEditor editor = new TextEditor();
        editor.setContent("First version");
        Memento memento = editor.save();
        
        editor.setContent("Second version");
        System.out.println("Current content: " + editor.getContent()); // Output: Second version

        editor.restore(memento);
        System.out.println("Restored content: " + editor.getContent()); // Output: First version
    }
}
```

---

Эти паттерны не так часто применяются, так как они решают более специфичные задачи. Однако их использование может значительно улучшить гибкость и расширяемость системы в определенных случаях, например, когда необходимо контролировать изменение состояния объектов или вводить сложные операции над различными классами.