## Задачи для закрепления знаний
Вот несколько задач на каждый из порождающих паттернов, которые помогут глубже разобраться в их применении:

---

### 1. Singleton

**Задача:** Создайте систему для управления логированием в приложении. Ваша задача — создать класс `Logger`, который:
   - Будет существовать в одном экземпляре (Singleton).
   - Должен записывать сообщения в файл (например, `log.txt`), добавляя к каждому сообщению временную метку.
   - Обеспечьте возможность указания уровня логирования (например, INFO, DEBUG, ERROR) и сохраните его для каждого сообщения.

*Подсказка:* Реализуйте метод `log(String level, String message)` в `Logger`, который добавит сообщение в файл `log.txt` с форматом `[LEVEL] [TIME] Message`.

---

### 2. Factory Method

**Задача:** Реализуйте систему для отправки уведомлений в зависимости от типа канала связи (например, Email, SMS, Push-уведомление). 
   - Создайте интерфейс `Notification`, который будет содержать метод `send(String message)`.
   - Реализуйте конкретные классы уведомлений `EmailNotification`, `SmsNotification`, и `PushNotification`.
   - Реализуйте фабрику `NotificationFactory`, которая будет возвращать конкретный объект уведомления в зависимости от входного параметра (например, строка "email" создаст `EmailNotification`, "sms" создаст `SmsNotification` и так далее).

**Дополнительно:** Добавьте логику для проверки формата сообщения в зависимости от типа уведомления. Например, в `SmsNotification` сделайте проверку, что длина сообщения не превышает 160 символов.

---

### 3. Abstract Factory

**Задача:** Создайте кроссплатформенную систему интерфейсов для работы с окнами и кнопками, которая будет иметь разные реализации для Windows и MacOS *(либо Linux)*.
   - Создайте интерфейсы `Window` и `Button` с методами `render()`.
   - Создайте две реализации `WindowsWindow`, `WindowsButton` и `MacWindow (LinuxWindow)`, `MacButton (LinuxButton)`, которые отображают сообщения, специфичные для платформы.
   - Реализуйте абстрактную фабрику `GUIFactory` с методами `createWindow()` и `createButton()`, а также конкретные фабрики `WindowsFactory` и `MacFactory (LinuxFactory)`.
   - Напишите клиентский код, который будет создавать окна и кнопки, используя только `GUIFactory`, и выводить информацию о созданных элементах интерфейса для платформы, выбранной в рантайме.

**Дополнительно:** Добавьте к каждому компоненту проверку на размеры и стили, чтобы на разных платформах кнопки и окна отличались.

---

### 4. Builder

**Задача:** Создайте систему конструирования заказов для кафе. Необходимо разработать класс `Order`, который будет включать различные компоненты (например, напиток, основное блюдо, десерт).
   - Определите класс `OrderBuilder` с методами `setDrink(String drink)`, `setMainCourse(String mainCourse)`, и `setDessert(String dessert)`, а также метод `build()` для создания заказа.
   - Добавьте к каждому методу возможность указания размера напитка и количества сахара, тип гарнира и т. д.
   - В `Order` должен быть метод `getDescription()`, который вернёт полное описание заказа.

**Дополнительно:** Добавьте опции для добавления дополнительных ингредиентов (например, сироп в напиток, топпинг на десерт) с помощью метода `addExtra(String extra)`.

---

### 5. Prototype

**Задача:** Реализуйте прототип для создания документов в текстовом редакторе. 
   - Создайте класс `Document`, содержащий поля для текста, типа шрифта, его размера и цвета.
   - Создайте интерфейс `Cloneable` и метод `clone()` в классе `Document`, который будет возвращать копию документа.
   - Реализуйте метод, который создаёт дубликаты документов и модифицирует их (например, меняет только цвет текста).
   - Используйте клон для создания новой версии документа, в котором меняется только оформление (например, создайте копию документа, где изменен шрифт).

**Дополнительно:** Реализуйте возможность редактирования текста и сохранения его версии в отдельный `Document`. Убедитесь, что текст и оформление при этом сохраняются.

---

Эти задачи помогут попрактиковаться в использовании каждого паттерна, ведь они воспроизводят реальные сценарии, в которых паттерны упрощают работу. Попробуйте самостоятельно решить задачи, а если возникнут вопросы — обращайтесь!