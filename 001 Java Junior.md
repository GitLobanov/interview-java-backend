## Вопросы

6. Преимущества Apache Kafka

- [Преимущества](../004%20Interview%20Info/posts/_inforage/Kafka/Base/Kafka.md#преимущества)

47. Основные методы борьбы с n+1

48. Базовые структуры данных по базам

49. Принципы Rest
- [[Принципы REST

50. Примеры асинхронного обращения?

51. Какие проблемы могут возникнуть при отсутствии тела запроса ?

52. JRPS и GraphQL, что такое кратко

53. Как работает Kafka?

54. Гарантирует ли Kafka порядок сообщений?

55. Для чего нужны Liquibase и Flyway на практике?

56. Что такое Dockerfile?

57. Что такое рехеширование, когда оно используется?

58. Как работает hashtable

59. Тип хранения аннотации

60. Что такое груминг?
63.  Как вынести Retry в конфигурацию?
64. Пример кейса: чтобы собрать отчеты нужно пойти в PostgreSQL собрать поля по JOIN, по agreement полю нужно пойти в no sql db Tarantool, чтобы достать оттуда соглашение. Работа с двумя дб.
65. В чем различается архитектура монолита от микросервисов, в плане развертывания
66. Blue/Green Deployment. Если один сервис упал, остальные не упадут, верно?

- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Devops/Blue-Green Deployment]]

67. Как команда может получть новую фичу?

- От аналитиков. Бизнес аналитики составляли юзер стори, передавали аналитикам, которые трактовали это на язык разработчиков.
- Разработчики общаются больше с аналитиками, а не бизнес-аналитиками

68. Что используешь для утечек памяти и нагрузок сети?

- Графана - собирает данные, Прометеус - можно посмотреть (DevOps)
- Spring Actuator (Developer)

70. Какие плагины можешь выделить в Idea?

- Sonara

71. Приходилось ли реализовывать составные индексы?

72. Приходилось использовать Json Bi поля?

- Когда в поле нужно сохранять прям Json

72. Как указать Lock без использования SQL запросов
73. 

118. Какие типы методов обычно используют статическое связывание в Java?  

119. Что такое динамическое связывание в Java?

120. Что такое клонирование объектов в Java?  

121. Какой интерфейс в Java используется для клонирования объектов?  

122. Что такое глубокое и поверхностное клонирование?

123. Что такое дженерики в Java?  

124. Для чего используются дженерики в Java?  

125. Как объявить класс или интерфейс с использованием дженериков?

126. В чем разница между интерфейсами List и Set?  

127. Перечислите основные интерфейсы коллекций в пакете java.util.  

128. Какие различия между ArrayList и LinkedList?

129. Как работает метод put в HashMap?  

130. В чем разница между HashMap и TreeMap?  

131. Какой метод позволяет получить значение по ключу в HashMap?

132. Что такое immutable (неизменяемые) коллекции в Java?  

133. Как можно создать immutable список в Java?  

134. Перечислите преимущества использования immutable коллекций.

135. Что такое concurrent коллекции и в чем их основное предназначение?  

136. Какие проблемы решают concurrent коллекции по сравнению с обычными коллекциями Java?  

137. Перечислите несколько примеров concurrent коллекций в Java.

173. Как использовать ReentrantLock и как он отличается от использования synchronized блоков или методов?



174. Какие стратегии вы бы использовали для оптимизации работы с многопоточностью в высоконагруженной системе?
175. Какие проблемы решает механизм CAS?
176. Какие операции поддерживает интерфейс Atomic в Java и как они связаны с CAS?
177. Какие особенности HotSpot JVM вы можете назвать?
178. Как JVM управляет памятью, включая стек и кучу?
179. Какой загрузчик классов используется по умолчанию для загрузки пользовательских классов?
180. Как можно написать собственный загрузчик классов и для чего это может потребоваться?
181. В чем разница между JIT-компиляцией и AOT (Ahead-of-Time) компиляцией?
182. Какие основные типы оптимизаций использует JIT-компилятор?
183. Какие существуют реализации JIT компилятора в различных JVM (например, HotSpot, GraalVM)?
184. Как работает сборщик мусора ZGC и для каких задач он наиболее подходит?
189. Сложность HashMap
192. Паттерны каскадных сбоев. Circuit Breaker Pattern, Bulkhead
https://vc.ru/u/1969656-dmitrii/725850-12-shablonov-mikroservisov-kotorye-ya-hotel-by-znat-do-sobesedovaniya-po-sistemnomu-dizainu?ysclid=m4ccoa0zex614019296
194. Опиши структуру кафки
- [[../004 Interview Info/posts/_inforage/Kafka/Base/Kafka]]
209. SAGA и его аналоги
- CQRS
- 2PC
- RLBS
- Sidecar паттерн;
- Упреждающая журнализация (Write-Ahead Log);
- Split-Brain паттерн;
- Направленная отправка (Hinted Handoff);
- Чтение с восстановлением (Read Repair).
- [Топ-5 архитектурных паттернов для распределённых систем](https://tproger.ru/translations/top-5-arhitekturnyh-patternov-dlja-raspredeljonnyh-sistem?ysclid=m4jjljd53t764285362)
210. TransactionalOutbox в разрезе распределенных транзакций
211. Что такое ArgumentCaptor
- [Using Mockito ArgumentCaptor](https://www.baeldung.com/mockito-argumentcaptor)
212. Шардирование на практике
- [[../004 Interview Info/Inforage/Patterns/Storage/Database/Шардирование]]
213. Что можно сделать с помощью команды HEAD
- [[../004 Interview Info/posts/_inforage/Other/How works Head in git]]
2. Что такое сервис локатор
- [[../004 Interview Info/Inforage/Patterns/Storage/Other/Service Locator|Service Locator]]
1. Оптиместические и пессеместические блокировки БД
- [Оптимистические и пессимистические блокировки на примере Hibernate (JPA)](https://habr.com/ru/articles/858714/)
2. Основные паттерны микросервисов
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Decomposition/Decomposition based on business capability]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Decomposition/Decomposition based on business subdomain]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Decomposition/Strangler]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Integration/Уровень защиты от повреждений (Anti-Corruption Layer)]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Data/Database per service]], [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Data/Shared database]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Data/API Composition]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Data/CQRS]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Data/Event Sourcing]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Data/SAGA]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Integration/API Gateway]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Integration/Backends for Frontends]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Observability/Server-Side Service Discovery]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Observability/Client-Side Service Discovery]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Devops/Blue-Green Deployment]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Devops/«Экземпляр сервиса на хост» (Service Instance Per Host)]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Fault tolerance/Circuit Breaker]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Fault tolerance/«Переборка» (Bulkhead)]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Monitoring/«Агрегация логов» (Log Aggregation)]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Monitoring/«Распределенная трассировка» (Distributed Tracing)]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Other/«Коляска» («Прицеп», Sidecar)]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Other/«Внешняя конфигурация» (External Configuration)]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Other/«Посредник» («Посол», Ambassador)]]
2. Какие есть варианты наследования в хибернейте
- SINGLE_TABLE (Все подклассы отображаются в одну таблицу.)
- JOINED (ach class in the hierarchy is mapped to its table. The only column that repeatedly appears in all the tables is the identifier, which will be used for joining them when needed), 
- TABLE_PER_CLASS (maps each entity to its table, which contains all the properties of the entity, including the ones inherited)
1. Что быстрее поиска по логарифму в бд?
- Вычисление по хэшу, если нам нужно точное значение
2. оконные функции
- [[../004 Interview Info/Inforage/Базы данных/Storage/SQL/Оконные функции]]
3. Как работает Conditional в спринге?
- [Spring Conditional Annotations](https://www.baeldung.com/spring-conditional-annotations)
4. Что такое Dummy в тестирование?
- объекты передаются, но никогда не используются. Обычно они используются только для заполнения списков параметров.
- [Терминология](https://www.calabonga.net/blog/post/terminologiya-dummy-fake-stubs-spies-mocks)
1. На каком этапе отрабатывают методы бин пост процессор
2. Как работает Lookup?
3. Структура кубера
4. Что такое ингрес в кубере?
5. Какие проблемы могут быть с аннотацией асинк?
- [Асинхронное выполнение в Spring. Аннотации @EnableAsync и @Async](https://www.tune-it.ru/web/romo/blog/-/blogs/spring-async-programming)
6. Есть ли в постгресс кластеризованный индекс? 
7. Что такое зеленые потоки
Зелёные потоки в Java — это потоки выполнения, управление которыми вместо операционной системы производит среда выполнения (например, JVM)
7. Что такое луковая и гексогональная архитектура?
8. Что такое аксон сага?
[Saga и Event Sourcing с Axon. Первое знакомство](https://habr.com/ru/articles/744460/)
9. Что такое комунда в саге?
[Camunda и распределённые транзакции на примере](https://bpmn2.ru/blog/camunda-i-raspredelennie-tranzakcii)
10. **Как работает создание новых бакетов при заполнение элементов в одном бакете?**
11. Что такое скип лист мапа?
12. Чем отличается команды RUN, CMD и ENTRYPOINT
Инструкция `RUN` используется в Dockerfile для выполнения команд, которые создают и конфигурируют образ контейнера Docker.
В инструкции `CMD` указывается команда, которая будет выполняться при запуске контейнера из образа Docker по умолчанию.
Инструкция `ENTRYPOINT` задает исполняемый файл для контейнера в качестве файла по умолчанию. Она похожа на `CMD`, но между ними есть различие: если `CMD` переопределяется аргументами командной строки, переданными в команду `docker run`, то для переопределения `ENTRYPOINT` все аргументы командной строки добавляются к самой инструкции `ENTRYPOINT`.
[Выбираем между инструкциями RUN, CMD и ENTRYPOINT](https://habr.com/ru/companies/nixys/articles/830830/)
13. Основные элементы кубера?
14. [SQL INTERCEPT, EXCEPT](https://datalemur.com/sql-tutorial/sql-union-intercept-except)
- `JOINS` combine results **horizontally**.
- `UNION ALL` and `UNION` stacks two or more results **vertically**.
- `INTERSECT` identifies **common rows** between two or more results.
- `EXCEPT` finds **unique rows** in the first `SELECT` query, but not present in the second `SELECT` query.
15. Что такое сервис регистри/service discovery
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Observability/Server-Side Service Discovery]]
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Observability/Client-Side Service Discovery]]
16. Что такое пост и пред условие в принципе барбары лисков
- [[../004 Interview Info/posts/_inforage/Java/Рефакторинг/SOLID]]
17. Какая сортировка используется в джава сорт
- [Arrays, Collections: Алгоритмический минимум](https://habr.com/ru/articles/344288/)
18. Почему event sourcing и cqrs идут вместе?
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Data/Event Sourcing]]
19. Что такое паттерн log aggregation?
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Monitoring/«Агрегация логов» (Log Aggregation)]]
20. Что такое Mapped diagnostic context?
- [Improved Java Logging with Mapped Diagnostic Context (MDC)](https://www.baeldung.com/mdc-in-log4j-2-logback)
21. Как работает класс лоадер?
- [[../004 Interview Info/posts/_inforage/Java/Core/Загрузка классов. Class Loader]]
22. Что такое Avro в кафке?
- [[../004 Interview Info/posts/_inforage/Kafka/Storage/Avro]]
24. Что такое паттерн Retry?
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Fault tolerance/Retry]]
25. Что такое DirtiesContext?
_@DirtiesContext_ is a **Spring testing annotation**. It indicates the associated test or class modifies the _ApplicationContext_. It tells the testing framework to close and recreate the context for later tests. We can annotate a test method or an entire class. By setting the _MethodMode_ or _ClassMode_, **we can control when Spring marks the context for closure**.
26. Какие бывают ссылки на объекты?
- [[../004 Interview Info/posts/_inforage/Java/Core/Виды ссылок. Weak, Soft, Phantom]]
27. Какие есть гарантии доставки в кафка?
- At-most-once delivery (“как максимум однократная доставка”). Это значит, что сообщение не может быть доставлено больше одного раза. При этом сообщение может быть потеряно.
- At-least-once delivery (“как минимум однократная доставка”). Это значит, что сообщение никогда не будет потеряно. При этом сообщение может быть доставлено более одного раза.
- Exactly-once delivery (“строго однократная доставка”). Святой грааль систем сообщений. Все сообщения доставляются строго единожды.
28. Какой порядок выполнения в запросе sql?
- FROM
- ON
- JOIN
- WHERE
- GROUP BY
- HAVING
- SELECT
- DISTINCT
- ORDER BY
- TOP
1. Как работает volatile и механизм CAS?
[Volatile и Atomic в Java](https://sky.pro/wiki/java/volatile-i-atomic-v-java-obespechenie-atomarnosti-operatsiy/)
30. Как работают CopyOnWriteArrayList/ConcurrentHashMap
- [CopyOnWriteArrayList & ConcurrentHashMap in Java](https://blog.devgenius.io/copyonwritearraylist-concurrenthashmap-in-java-ac52bdcdc433)
- [Guide to CopyOnWriteArrayList](https://www.baeldung.com/java-copy-on-write-arraylist)
31. Почему String рекомендуется в качестве ключа для HashMap?
Поскольку строки неизменны, их хэшкод кэшируется в момент создания, попадая в пулл строк, и не требует повторного пересчета. Это делает строки отличным кандидатом для ключа в `Map` и они обрабатываются быстрее, чем другие объекты-ключи `HashMap`. 
32. Как работает 2pc, 3pc
- [[../004 Interview Info/Inforage/Patterns/Storage/Microservices/Data/2-phase-commit]]
33. Устройство паттерна билдер
- [[../004 Interview Info/Inforage/Patterns/Storage/GoF/03 Основное/GoF Строитель]]
34. В чем разница между RestController vs Contoller
35. [Spring MVC: создание веб-сайтов и RESTful сервисов](https://habr.com/ru/articles/500572/)
36. Что такое MVCC (Multi-Version Concurrency Control) в постгре
37. Что такое view в sql 
38. Что такое @Modifying в data JPA
39. Что такое гит аменд 
40. Что такое интерактивный гит ребейз
41. В чем отличие библиотек от спринг стартеров?
42. Виды агрегации логов
43. Жизненный цикл мавена
44. Различие градла от мавена
45. Что такое Граф QL
46. Если у нас hashCode всегда 1, когда будут увеличиваться бакеты?
47. Методы для разрешения коллизий
- Метод цепочек является наиболее простым методом разрешения коллизий. В ячейке массива мы будем хранить не элементы, а связанный список данных элементов.  
- В данном методе в ячейках хранятся сами элементы, а в случае коллизии происходит **последовательность проб**, то есть мы начинаем по некоторому алгоритму перебирать ячейки в надежде найти свободную. Это можно делать разными алгоритмами (**линейная / квадратичная последовательности проб**, **двойное хеширование**), каждый из которых обладает своими проблемами (например, возникновение **первичных** или **вторичных кластеров**).  
48. Почему рекомендуется внедрение через конструктор, а не через поля?
- конструктор требует указания всех зависимостей при создании объекта
- объект не будет создан без необходимых данных.
- поля объекта можно сделать неизменяемыми (final)
49. Что такое dead queue in kafka?
50. Короткое замыкание в стримах
51. Что такое partitioning by
52. Что такое FetchType
53. Зачем использовать TreeMap and LinkedHashMap
54. Что вместо CAS в новых версиях атомиков
55. Какие ты знаешь команды гит?
- стэш, реверт, ресет
# Паттерны

## [[../004 Interview Info/Inforage/Patterns/GoF]]

- Порождающие паттерны решают задачи создания объектов.
Абстрактная фабрика, Строитель, Фабричный метод, Прототип, Одиночка
- Структурные паттерны фокусируются на объединении классов и объектов.
Адаптер, Мост, Компоновщик, Декоратор, Фасад, Легковес, Прокси
- Поведенческие паттерны описывают взаимодействие и коммуникацию между объектами.
Цепочка ответственности, Команда, Состояние, Стратегия, Наблюдатель, Посетитель, Посредник, Заместитель 
## [[../004 Interview Info/Inforage/Patterns/GRASP]]
### Низкая связанность (Low Coupling) и Высокое зацепление (High Cohesion)

> Пример из живой природы. Органы человека выполняют определенную функцию, и не пытаются выполнять чужие задачи - High Cohesion. Однако может не сильно зависеть от других людей, чтобы просуществовать - Low Coupling. 

> Пример из программного кода. Сервис заказов должен хорошо выполнять свою задачу, имея связанную структуру и не стараться выполнять чужую работу, к примеру авторизации пользователей - High Cohesion. Однако он должен иметь низкую связанность с другими сервисами, к примеру не знать как там реализуется оплата заказа.
# Принципы

## YAGNI

>You Aren’t Gonna Need It / Вам это не понадобится. Если пишете код, то будьте уверены, что он вам понадобится. Не пишите код, если думаете, что он пригодится позже

- Отказ от реализации "на будущее" или "на всякий случай".
- Фокус на текущих требованиях, а не на возможных гипотетических сценариях.
- Минимизация технического долга, связанного с поддержкой ненужных фич.
## DRY

> Don’t Repeat Yourself / Не повторяйтесь. Дублирование кода – пустая трата времени и ресурсов.

## KISS

> Keep It Simple, Stupid / Будь проще. Не придумывайте к задаче более сложного решения, чем ей требуется. Лучше сделать что-то простое и работающее, чем усложнять без необходимости.

- Избегание сложных конструкций, когда можно использовать более простые.
- Отказ от излишней абстракции или ненужной гибкости.
- Фокус на ясности и читаемости кода.

## [[../004 Interview Info/posts/_inforage/Java/Рефакторинг/SOLID]]

- S) Single-responsibility principle /Принцип единственной ответственности - каждый объект, класс и метод должны отвечать только за что-то одно. Если ваш объект/класс/метод делает слишком много, вы получите спагетти-код.
- O) Open–closed principle / Принцип открытости-закрытости - сущности программы должны быть открыты для расширения, но закрыты для модификации. Речь о том, что нельзя переопределять методы или классы, а просто добавлять дополнительные функции по мере необходимости.
- L) Liskov substitution principle / Принцип подстановки Лисков - этот принцип гласит, что объекты старших классов должны быть заменимы объектами подклассов, и приложение при такой замене должно работать так, как ожидается.
- I) Interface segregation principle / Принцип разделения интерфейсов. Объекты не должны зависеть от интерфейсов, которые они не используют  
- D) Dependency inversion principle / Принцип инверсии зависимостей. Этот принцип невозможно переоценить. Мы должны полагаться на абстракции, а не на конкретные реализации. Компоненты ПО должны иметь низкую связность и высокую согласованность.
# Java Core
## Collection Hierarchy

![[../004 Interview Info/posts/_res/Pasted image 20240926124516.png]]

- [[../004 Interview Info/posts/_inforage/Java/Core/data structure/Set]]
- [[../004 Interview Info/posts/_inforage/Java/Core/data structure/TreeSet]]
## Hash Code & Equals Contract

- Если a.equals(b), то a.hashCode() == b.hashCode() должно быть всегда, но не наоборот
- желательно, чтобы в обоих методах участвовали одни и те же поля

> equals

[[../004 Interview Info/posts/_inforage/Java/Core/Контракт Equals]]

- if (this == o) return true;
- if (o == null || getClass() != o.getClass()) return false;
- if (id != person.id) return false;
- return name != null ? name.equals(person.name) : person.name == null;

> hashCode

[[../004 Interview Info/posts/_inforage/Java/Core/Контракт hashCode]]

- int result = id;
- result = 31 * result + (name != null ? name.hashCode() : 0);
- return result;

## [HashMap](../004%20Interview%20Info/posts/_inforage/Java/Core/data%20structure/HashMap.md)
- В качестве ключа можно хранить null
- Состоит из корзин, кол-во 16 изначально
### TreeMap 

- В качестве ключа нельзя хранить null. Помним что это дерево, которму нужен Comparable, Comparator При добавление идет сравнение с другими, чтобы определить куда положить объект.
### Добавление в Hash Map

- вычисляет hashCode
- делает побитовое умножение на кол-во корзин - 1 (int index = hash % (n - 1);)
- результат будет индексом
- добавление объекта по индексу
- Если индексы совпадают, проверяет hash, чтобы удостоверится что объекты разные
- Hash разные, формирует односвязный список добавляя туда объект
- Если коли-во объектов в связанном списке становится больше (по умолчанию 8), связанный список преобразуется в красно-черное дерево
- Колизея может случится когда hash одинаковые, идет доп. проверка по equals
- Если hashCode возвращает константу, то всегда один и тот же индекс, и таким образом мапа выродится в одно-связный список, а затем в дерево

### Чтение из Hash Map

- Вызываем метод get()
- Вычисляется хэш-код
- Находим индекс - операция взятия остатка от деления хэш-кода на размер массива
```java
int index = hash % (n - 1);
```
- поиск в связанном списке. 
	 - если один элемент проверка ключей сразу по Equals.
	 - если связанный список элементов, производит итерацию и сравнивает по Equals.
	 - если это красно-черное дерево, поиск выполняется с использованием алгоритма бинарного дерева
- как только элемент находится по Equals - он возвращает его значение

- В идеальных условиях чтение из `HashMap` имеет **время O(1)**, так как доступ к элементу в массиве происходит мгновенно, **один элемент в бакете**.
- Если в одной ячейке находятся несколько элементов (коллизии), то время может возрасти до **O(n) для связанных списков**, но использование **деревьев снижает его до O(log n)**

## Strong, Weak, Soft, Phantom Link

[[../004 Interview Info/posts/_inforage/Java/Core/Виды ссылок. Weak, Soft, Phantom]]

- Сильная ссылка - создание объекта при помощи **new**
- Слабая ссылка - создание при помощи Weak Reference -  если существуют только слабые ссылки, объект очищается
- Мягкие ссылки - объекты очищаются в случае нехватки памяти
- Фантомные ссылки - можно получать уведомления об ее удалении при помощи помещения ее в Queue Reference.
## [[../004 Interview Info/posts/_inforage/Java/Core/JVM]]
## Отношения между классами

- наследование: объекты дочернего класса наследуют свойства родительского класса
- ассоциация: объекты вступают во взаимодействие между собой
- агрегация: объекты разных классов образуют целое, оставаясь самостоятельными
- композиция: объекты одного класса входят в объекты другого, не обладая самостоятельностью
- класс-метакласс: экземпляры класса являются классы
## Garbage Collectors

>  Устройство памяти

- Heap
- Stack

> Как работает GC в общем виде

- Чистит память от неиспользуемых объектов

- Все объекты создаются в Eden
- Когда Eden заполняется приходит GC, выполняет Minor Collection (Mark-Copy)
- Собирая все молодые объекты в ToSpace
- S0 и S1, каждый раз меняется ролями FromSpace и ToSpace
- Те объекты, которые пережили достаточное кол-во сборок мусора помещаются в old generation
- Затем при повторной минорной сборке, From и To меняются местами.
- В old generation скапливаются singletons, beans, resource managers 
- Когда old generation заполняется, выполняется Major Collection (Mark-Sweep-Compact), после этого выполняет Full Collection
- Permanent Generation (MetaSpace) хранит мета информацию об классах и методах

![[../004 Interview Info/posts/_res/Pasted image 20240926131355.png]]

> Виды GC

- Serial Garbage Collector: Работает с единственным потоком, замораживает приложение при чистке

- Parallel Garbage Collector: Использует несколько потоков, замораживает другие потоки

- CMarkSweep Garbage Collector - Deprecated

- G1 Garbage Collector

Запускается на мультипроцессорных машинах, с 8 java. Делит память на 2048 регионов, в начале делает Stop the World, затем параллельно выполняет отслеживание, после второго S the W, выполняет релокацию
## Functional Interfaces

- Function T, R
- Supplier T
- Consumer T
- [[../004 Interview Info/posts/_inforage/Java/Core/function coding/BiFunction]]
- [[../004 Interview Info/posts/_inforage/Java/Core/function coding/UnaryOperator]]
- [[../004 Interview Info/posts/_inforage/Java/Core/function coding/BinaryOperator]]
- @FuncationalInterface - marked interface
## Stream API

- Source
- Intermediate ops / Промежуточные операции
- Terminal ops / Терминальные операции 
-  [[../004 Interview Info/posts/_inforage/Java/Core/function coding/Stateless-Stateful операторы]]

## Parallel Stream 

> Process

- Spliterator рекурсивно разбивает коллекцию на части, при помощи trySplit()
- Затем каждая часть разбивается на подзадачи (fork) при помощи Fork Join Pool
- Каждая задача обрабатывается параллельно
- После обработки все результаты собираются (join)
- По умолчанию используется общий Fork Join Pool, в котором количество потоков равно количеству доступных процессорных ядер (available processors).
## Multi Concurrency 

>Способы создания потоков

- наследование класса Thread
- реализация Runnable, и потом использование в локальных Threads

> Callable vs Runnable

- Callable пробрасывает exception и возвращает результат

> Executor Service

- execute(Runnable command)
- submit(Callable T task/Runnable task) - returns a `Future`
- shutdown() allowing previously submitted tasks to execute before terminating
- shutdownNow() halts the processing of waiting tasks, and returns a list of the tasks that were awaiting execution
- invokeAny(Collection<\? extends Callable T> tasks) executes the given tasks, returning the result of one that successfully completes.
- invokeAll(Collection \<? extends Callable T> tasks) executes the given tasks, returning a list of `Future` objects representing their pending results
- waitTermination(long timeout, TimeUnit unit)

>Виды создание Executor Service

- newFixedThreadPool(n)
- newCachedThreadPool()
- newSingleThreadExecutor()
- newSheduled()
- newWorkStealingPool

>Happens Before

- Последовательность в одном потоке: A Happens-Before B, если A происходит перед B в одном потоке.
- Синхронизация: Если А вызывает join() на потоке B, то все действия в B происходят перед возвращением из join в А.
- Блоки synchronized: если поток А завершает работу с блоком synchronized, и другой поток B начинает работать с тем же блоком, все действия в А происходят до действия в B.
- Volatile: запись в volatile переменную Happens-Before любого последующего чтения этой переменной
- Atomic: Запись в переменную Atomic Happens-Before любого последующего чтения этой переменной.
- Thread.start(): Если поток А вызывает start() на потоке B, все действия в А перед вызовом start() происходят перед действиями в B.
- Transitive Rule: Если A Happens-Before B и B Happens Before C, то А также Happens-Before C

> volatile 

- Запись в `volatile` переменную видна всем потокам, она не кэшируется для одного потока. Если один поток записывает значение в `volatile` переменную, другие потоки сразу увидят это значение
- `volatile` не гарантирует атомарность операций. Например, операции, такие как инкремент (`count++`), не являются атомарными, даже если `count` объявлен как `volatile`
- `volatile` не обеспечивает блокировку, поэтому не защищает от состояний гонки. 
- Хотя `volatile` гарантирует, что записи будут видны, он не гарантирует порядок выполнения операций, связанных с изменением других переменных

> Атомарность

- Несколько операций выполняются как единое целое
- Атомарные операции гарантируют, что данные остаются консистентными даже при одновременном доступе из нескольких потоков
- В контексте многопоточности это означает, что операции не могут быть прерваны другими потоками.
- более надежное: compareAndSet(V expect, V update)
- ненадежные: incrementAndGet(), addAndGet(int delta)
- Механизм CAS (Compare-And-Swap): позволяет выполнить операцию обновления только в том случае, если текущее значение соответствует ожидаемому, что помогает избежать состояний гонки без блокировок

> Synchronized vs Lock

- lock, интерфейс требующий создания объекта
- в lock можно блокировать на уровне объекта
- synchronized, автоматически снимает блокировку при выходе из блока, что упрощает обработку исключений
- lock, требует явного снятия блокировки, что может привести к ошибкам, если разработчик забудет это сделать
- lock, есть tryLock(), что позволяет установить тайм-ауты и избежать зависаний
- оба позволяют рекурсивные вызовы, при ReentrantLock, можно сохранить гибкость
- lock, позволяет блокировать только определённые участки кода, что позволяет более точно контролировать, какие части программы могут выполняться параллельно

> Thread local

- позволяет нам хранить данные доступные специфичному потоку

```java
ThreadLocal<Integer> threadLocalValue = new ThreadLocal<>();
threadLocalValue.set(1); 
Integer result = threadLocalValue.get();
threadLocal.remove();
```

## Serializable, Externalizable, Маршаллинг

- Serializable: интерфейс, который позволяет объектам быть сериализованными, то есть преобразованными в поток байтов для сохранения или передачи по сети. Можно переопределить метод `writeObject` для настройки процесса сериализации.
- Externalizable: интерфейс, который расширяет `Serializable` и требует реализации двух методов: `writeExternal()` и `readExternal()`.
- Маршаллинг: это процесс преобразования объекта в поток байтов (или другую форму) для его хранения или передачи.
# Базы данных

## Нормализация данных

> **Нормализация** — это процесс организации данных в реляционной базе данных для уменьшения избыточности и повышения целостности данных. Основной целью нормализации является минимизация дублирования данных и обеспечение корректности данных.

- Первая нормальная форма (1НФ): если все атрибуты имеют атомарные (неделимые, цельные) значения и каждый атрибут содержит только одно значение
- Вторая нормальная форма (2НФ): если оно находится в 1НФ и каждый не ключевой атрибут неприводимо зависит от Первичного Ключа(ПК).
- Третья нормальная форма (3НФ):  когда находится во 2НФ и каждый не ключевой атрибут нетранзитивно зависит от первичного ключа. Убираем транзитивные зависимости, это когда у нас поле может зависеть от внешнего ключа
- Бойс-Кодд нормальная форма (BCNF): если она в 3НФ и нету составных ключей
## ACID

> **ACID** — это набор свойств, обеспечивающих надежность и целостность транзакций в реляционных базах данных.

- Atomicity (Атомарность): транзакция рассматривается как единое целое, выполняется полностью или не выполняется вовсе.
- Consistency (Согласованность): после завершения транзакции база данных должна находиться в согласованном состоянии. Все бизнес-правила и ограничения должны быть соблюдены до и после выполнения транзакции
- Isolation (Изолированность): транзакции должны выполняться независимо друг от друга. Результаты одной транзакции не должны быть видны другим транзакциям, пока они не завершены
- Durability (Непрерывность): после завершения транзакции все изменения должны быть надежно сохранены, даже в случае сбоя системы. Данные должны оставаться в постоянном хранилище.

## Виды проблем с изоляцией

- Dirty Read (Грязное чтение): транзакция читает данные, которые были изменены другой транзакцией, но еще не зафиксированы. Если вторая транзакция откатывается, то первая транзакция получает неверные данные.
- Lost Update (Потерянное обновление): две транзакции одновременно читают одну и ту же запись, затем обе пытаются обновить эту запись. В результате обновление одной транзакции может быть потеряно, так как вторая транзакция перезаписывает ее.
- Non-Repeatable Read (Неповторяемое чтение): транзакция читает одни и те же данные несколько раз, но получает разные результаты из-за изменений, сделанных другой транзакцией между чтениями.
- Phantom Read (Фантомное чтение): транзакция выполняет запрос и получает набор строк, а затем, при повторном выполнении того же запроса, видит, что некоторые строки добавлены или удалены другой транзакцией. Это создает иллюзию "фантомных" строк.
 ![[../004 Interview Info/posts/_res/Pasted image 20240929134753.png]]

## [[../004 Interview Info/Inforage/Microservices/Storage/CAP - теорема]]

## [[../004 Interview Info/Inforage/Microservices/Storage/PACELC теорема]]

## [[0006 Programming/006 Inforage/Microservices/03 Storage/BASE]]

## Индексы

- [[Структуры данных в БД]]

> Это специальные структуры данных, которые позволяют ускорить поиск и доступ к данным в таблицах. они работают как указатели на данные и помогают базе данных **находить строки быстрее**, без необходимости просматривать всю таблицу.

### Типы

- Primary Index: автоматически создается для Primary Key. Гарантирует уникальность и используется для быстрого доступа к строкам.
- Unique Index: обеспечивает уникальность значений в одном или нескольких столбцах, но в отличие от первичного индекса, может быть создан для любого столбца.
- Non-Unique Index: не требует уникальности значений и используется для ускорения запросов к столбцам, которые не являются ключевыми.
- Clustered Index: сортирует и хранит данные таблицы физически в соответствии с ключевыми значениями. У таблицы может быть только один кластеризованный индекс.
- Non-Clustered Index: содержит указатели на строки в таблице, а не сортирует данные. В таблице может быть несколько некластеризованных индексов.
- Composite Index: индекс, созданный на нескольких столбцах для ускорения поиска, где условия запроса используют комбинацию этих столбцов.

### Структуры данных

- B-Tree: организован в виде сбалансированного дерева, где каждая операция поиска, вставки или удаления происходит за O log(n)
- Hash Index: Хеш-функция преобразует значения столбца в ключи фиксированного размера, которые указывают на соответствующие строки. Поиск O (1)
- Bitmap Index: хранит данные в виде битовых карт для каждого уникального значения столбца. Например, если столбец содержит только несколько уникальных значений, для каждого значения создается битовая карта, где каждый бит указывает на присутствие или отсутствие значения в строке

## JOIN

- 1. INNER JOIN: возвращает только те строки, которые есть в обеих таблицах и соответствуют условию.
- 2. LEFT JOIN (LEFT OUTER JOIN): возвращает все строки из левой таблицы, даже если нет совпадений в правой. Несовпадающие строки будут с NULL-значениями для правой таблицы.
- 3. RIGHT JOIN (RIGHT OUTER JOIN): возвращает все строки из правой таблицы, даже если нет совпадений в левой. Несовпадающие строки будут с NULL-значениями для левой таблицы.
-  4. FULL JOIN (FULL OUTER JOIN): возвращает все строки из обеих таблиц: совпадающие и несовпадающие. Несовпадающие строки заполняются NULL-значениями.
- 5. CROSS JOIN: возвращает **декартово произведение** всех строк из обеих таблиц, то есть каждая строка первой таблицы объединяется с каждой строкой второй таблицы.

![[../004 Interview Info/posts/_res/Pasted image 20240929151959.png]]

## Блокировки

### Типы

- Shared Lock (S): позволяет нескольким транзакциям читать данные одновременно, но блокирует изменения.
- Exclusive Lock (X): полная блокировка данных — ни чтение, ни запись другими транзакциями не допускается, только одна транзакция изменяет данные в данный момент

### Оптимистическая и пессимистическая блокировка

- Оптимистическая: предполагает, что конфликтов не будет. Сохраняет результаты модификации в рабочей памяти транзакции, поэтому конфликты могут возникнуть на этапе коммита.
- Пессимистическая: предполагает, что конфликт возможен, и блокирует данные сразу при начале транзакции

## Вопросы

- [[../004 Interview Info/posts/_qa/Theory/Infrastructure/QA. БД]]

# WEB

## HTTP идемпотентные методы

> Идемпотентные методы HTTP — это методы, которые при повторном выполнении приводят к тому же результату, что и при первом запросе, независимо от количества повторов.

- Идемпотентные: GET, PUT, DELETE, HEAD, OPTIONS
- НЕ идемпотентные: POST, PATCH, CONNECT

![[../004 Interview Info/posts/_res/Pasted image 20240929154406.png]]

## REST

- [[../004 Interview Info/posts/_inforage/Компсети/01 Вопросы/Вопросы. REST]]
### Principles

- Клиент-серверная модель (client-server model).
- Отсутствие состояния (statelessness).
- Кэширование (cacheability).
- Единообразие интерфейса (uniform interface). HATEOAS
- Многоуровневая система (layered system).
- Код по требованию (code on demand) — необязательно.
### Уровни зрелости REST API

![[../004 Interview Info/posts/_res/Pasted image 20241001125557.png]] 

**Level 0**: Most people agree that if you have your API at this level, you don't even have a REST API. 
Since XML is broadly used in this scenario, inside the XML body we specify the operation that needs to be done, as well as the relevant data for that. This means that the necessary detail is inside our request's body.

**Level 1**: Starting level for REST API. We here introduce the notion of Resource URI, meaning individual URIs for each resource.
At this level, we have a mindset of "divide and conquer", meaning that we split our huge only endpoints into smaller fractions, where each fraction represents an endpoint (which itself represents a resource).

**Level 2**: Most of today's APIs are at this level. We introduce the standard HTTP methods to operate on multiple URIs. Our HTTP verb specifies what we will be doing in that specific resource. Each verb was originally designed for one purpose and you use it for such purpose.

**Level 3**: In this level, we come across the acronym HATEOAS, which stands for Hypertext As The Engine Of Application State.

![[../004 Interview Info/posts/_res/Pasted image 20241001130623.png]]
# Spring

## Вопросы

- [[../004 Interview Info/posts/_qa/Theory/Spring/QA. Spring Core]]
- [[../004 Interview Info/posts/_qa/Theory/Spring/QA. Spring MVC]]
- [[../004 Interview Info/posts/_qa/Theory/Spring/QA. Spring Boot]]
- [[../004 Interview Info/posts/_qa/Theory/Spring/QA. Spring Security]]

## Основа

- [[Spring Framework]]
# Kafka

## Вопросы

- [[../004 Interview Info/posts/_qa/Theory/Infrastructure/QA. Kafka]]
## Основы

- [[../004 Interview Info/posts/_inforage/Kafka/Base/Kafka]]

# Microservices

## Вопросы

- [[../004 Interview Info/posts/_qa/Theory/Architecture/QA. Microservices]]
## Основа

- [[0006 Programming/005 Learn/Microservices]]
# Ресурсы

- [Deep Dive Into Java Executor Framework](https://dzone.com/articles/deep-dive-into-java-executorservice)
- [Золотое правило git rebase](https://habr.com/ru/companies/otus/articles/352640/)
- [GRASP паттерны проектирования](https://habr.com/ru/articles/92570/)
- [Шпаргалка по шаблонам проектирования](https://habr.com/ru/articles/210288/) 
- [Подготовка к Spring Professional Certification. Контейнер, IoC, бины](https://habr.com/ru/articles/470305/)
- [Как внедрить Prototype в Singleton в Spring / ProxyMode](https://habr.com/ru/articles/761330/)
- 


