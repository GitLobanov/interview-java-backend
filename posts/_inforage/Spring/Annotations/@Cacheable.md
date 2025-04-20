- `@Cacheable`: Запускает наполнение кэша.
- `@CacheEvict`: Запускает выгрузку кэша.
- `@CachePut`: Обновляет кэш, не вмешиваясь в выполнение метода.
- `@Caching`: Перегруппировывает несколько операций кэширования для применения к методу.
- `@CacheConfig`: Разделяет некоторые общие настройки, связанные с кэшем, на уровне класса.
## Configuring the Cache Provider

**Spring boot needs an underlying cache provider that can store and manage the cached objects and support lookups**. Spring boot autoconfigures one of these providers with default options if it is present in the classpath and we have enabled cache by `@EnableCaching`.

- JCache (JSR-107) (EhCache 3, Hazelcast, Infinispan, and others)
- EhCache ([example](https://howtodoinjava.com/spring-boot/spring-boot-ehcache-example/))
- Hazelcast
- Infinispan
- Couchbase
- Redis
- Caffeine ([example](https://howtodoinjava.com/spring-boot/spring-boot-caffeine-cache/))
- Simple cache

Аннотацию `@Cacheable` можно использовать для выделения методов, которые можно кэшировать – то есть методов, для которых результат сохраняется в кэше, чтобы при последующих вызовах (с теми же аргументами) возвращалось значение из кэша без необходимости снова вызывать метод. В своей простейшей форме объявление аннотации требует указания имени кэша, связанного с аннотируемым методом, как показано в следующем примере:

```java
@Cacheable("books") 
public Book findBook(ISBN isbn) {...}
```

## Генерация ключей по умолчанию

Поскольку кэши по своей сути являются хранилищами ключевых значений, каждый вызов кэшируемого метода должен быть преобразован в подходящий ключ для доступа к кэшу. Абстракция кэширования использует простой `KeyGenerator`, основанный на следующем алгоритме:

- Если параметры не заданы, возвращается `SimpleKey.EMPTY`.
- Если задан только один параметр, возвращается этот экземпляр.
- Если задано более одного параметра, возвращается `SimpleKey`, содержащий все параметры.

Этот подход хорошо работает для большинства случаев использования, если параметры имеют естественные ключи и реализуют допустимые методы `hashCode()` и `equals()`. Если это не так, то необходимо менять стратегию.

Чтобы указать другой генератор ключей по умолчанию, необходимо реализовать интерфейс `org.springframework.cache.interceptor.KeyGenerator`.


Для таких случаев аннотация `@Cacheable` позволяет указать способ генерации ключа через его атрибут `key`. Вы можете использовать язык выражений SpE для выбора интересующих вас аргументов (или их вложенных свойств), выполнения операций или даже вызова произвольных методов без необходимости писать какой-либо код или реализовывать какой-либо интерфейс. Этот подход более предпочтителен, чем генератор по умолчанию, так как методы имеют тенденцию сильно отличаться по сигнатурам по мере роста кодовой базы. Хотя стратегия по умолчанию может сработать для некоторых методов, она редко срабатывает для всех методов.

В следующих примерах используются различные объявления на языке SpEL:

`@Cacheable(cacheNames="books", key="#isbn") public Book findBook(ISBN isbn, boolean checkWarehouse, boolean includeUsed) @Cacheable(cacheNames="books", key="#isbn.rawNumber") public Book findBook(ISBN isbn, boolean checkWarehouse, boolean includeUsed) @Cacheable(cacheNames="books", key="T(someType).hash(#isbn)") public Book findBook(ISBN isbn, boolean checkWarehouse, boolean includeUsed)`

В предыдущих фрагментах показано, как можно с легкостью выбрать определенный аргумент, одно из его свойств или даже произвольный (статический) метод.

Если алгоритм, отвечающий за генерацию ключа, слишком специфичен или его необходимо использовать совместно, можно определить кастомный `KeyGenerator` для операции. Для этого задается имя используемой реализации бина `KeyGenerator`, как показано в следующем примере:

```java
@Cacheable(cacheNames="books", keyGenerator="myKeyGenerator") 
public Book findBook(ISBN isbn, boolean checkWarehouse, boolean includeUsed)
```

## Синхронизированное кэширование

В многопоточном окружении некоторые операции могут быть вызваны одновременно для одного и того же аргумента (обычно при запуске). По умолчанию абстракция кэша ничего не блокирует, а одно и то же значение может вычисляться несколько раз, что сводит на нет цель кэширования.

Для таких случаев можно использовать атрибут `sync`, чтобы дать поставщику базового кэша команду блокировать запись кэша на время вычисления значения. В результате только один поток будет занят вычислением значения, а остальные заблокированы до тех пор, пока запись не обновится в кэше. В следующем примере показано, как использовать атрибут `argNames`:

```java
@Cacheable(cacheNames="foos", sync=true) 
public Foo executeExpensiveOperation(String id) {...}
```

Это опциональная функция, поэтому ваша любимая кэш-библиотека может ее не поддерживать. Все реализации `CacheManager`, передаваемые основным фреймворком, поддерживают её. Более подробную информацию см. в документации поставщика кэша.### 

## Условное кэширование

Иногда метод может не подходить для постоянного кэширования (например, он может зависеть от заданных аргументов). Аннотации кэша поддерживают такие случаи использования благодаря параметру `condition`, который принимает выражение `SpEL`, вычисленное как `true` или `false`. Если `true`, то метод кэшируется. Если нет, то метод ведет себя так, как будто он не кэширован (то есть метод вызывается каждый раз, независимо от того, какие значения находятся в кэше или какие аргументы используются). Например, следующий метод кэшируется только в том случае, если `name` аргумента имеет длину меньше 32:

```java
@Cacheable(cacheNames="book", condition="#name.length() < 32") 
public Book findBook(String name)
```

В дополнение к параметру `condition` можно использовать параметр `unless`, чтобы запретить добавление значения в кэш. В отличие от `condition`, выражения с `unless` вычисляются после вызова метода. Развивая предыдущий пример, возможно, нам понадобится кэшировать только книги в мягкой обложке, как это сделано в следующем примере:

```java
@Cacheable(cacheNames="book", condition="#name.length() < 32", unless="#result.hardback") 
public Book findBook(String name)
```

It is used on the method level to let spring know that the response of the method is cacheable. Spring intercepts the request/response of this method and stores the response in the cache by the name specified in the annotation attribute e.g. _@Cacheable(“employees”)_.

```java
@Cacheable("employees")
public Optional<Employee> getEmployeeById(Long id) {
  return repository.findById(id);
}
```

Since caches are essentially key-value stores, each invocation of a cached method needs to be translated into a suitable key for cache access. By default, Spring uses the method parameters to form the cache keys as follows:

- If no params are given, return `SimpleKey.EMPTY`.
- If only one param is given, return that instance.
- If more than one param is given, return a `SimpleKey` that contains all parameters.

It is very necessary to understand that keys must implement [valid _hashCode()_ and _equals()_ methods contract](https://howtodoinjava.com/java/basics/java-hashcode-equals-methods/) for correct lookups.

```java
@Cacheable(value = "employees", key = "#id")
public Optional<Employee> getEmployeeById(Long id) {...}

@Cacheable(value = "employees", key = "#department.id")
public List<Employee> getEmployeesByDepartmentId(Department department) {...}
```

We can also do the caching only when a certain condition is satisfied. In the following example, we are caching when the employee id is greater than 0;

```java
@Cacheable(value = "employees", key = "#id", condition="#id > 0")
public Optional<Employee> getEmployeeById(Long id) {...}
```

## Аннотация `@CachePut`

Если кэш необходимо обновить, не вмешиваясь в выполнение метода, можно использовать аннотацию `@CachePut`. То есть метод будет вызываться всегда, а его результат помещаться в кэш (в соответствии с параметрами аннотации `@CachePut`). Она поддерживает те же параметры, что и аннотация `@Cacheable`, но используется для наполнения кэша, а не для оптимизации потока методов. В следующем примере используется аннотация `@CachePut`:

`@CachePut(cacheNames="book", key="#isbn") public Book updateBook(ISBN isbn, BookDescriptor descriptor)`

Использовать аннотации `@CachePut` и `@Cacheable` в одном и том же методе, как правило, настоятельно не рекомендуется, поскольку они имеют отличающуюся логику работы. В то время как в последнем случае вызов метода пропускается при использовании кэша, в первом происходит форсированный вызов метода для выполнения обновления кэша. Это приводит к непредвиденной логике работы, и, за исключением особых тупиковых ситуаций (например, аннотаций с условиями, исключающими их друг из друга), таких объявлений следует избегать. Обратите внимание, что такие условия не должны использовать результирующий объект (то есть переменную `#result`), так как они валидируются заранее для подтверждения исключения.

The _@CachePut_ annotation is very similar to the _@Cacheable_ annotation except the annotated method is always executed irrespective of whether the cache key is present in the cache or not. It supports the same options as `@Cacheable`.

```java
@CachePut(cacheNames = "employees", key = "#employee.id")
public Employee updateEmployee(Employee employee) {...}
```

## Аннотация `@CacheEvict`

Абстракция кэша позволяет не только создавать хранилище кэша, но и вытеснять из него данные. Этот процесс полезен для удаления устаревших или неиспользуемых данных из кэша. В отличие от аннотации `@Cacheable`, аннотация `@CacheEvict` разграничивает методы, которые выполняют вытеснение из кэша (то есть методы, которые действуют как триггеры для удаления данных из кэша). Как и её родственница, аннотация `@CacheEvict` требует задания одного или нескольких кэшей, на которые распространяется действие, позволяет задать кастомный кэш и разрешение ключа или условие, а также имеет дополнительный параметр (`allEntries`), который указывает, нужно ли выполнять вытеснение данных из всего кэша, а не только вытеснение записи (на основе ключа). В следующем примере вытесняются все записи из кэша `books`:

`@CacheEvict(cacheNames="books", allEntries=true) **(1)** public void loadBooks(InputStream batch)`

1. Использование атрибута `allEntries` для вытеснения всех записей из кэша.

Эта опция может пригодиться, если необходимо очистить всю область кэша. Вместо того чтобы вытеснять каждую запись (что заняло бы много времени, поскольку это неэффективно), все записи удаляются за одну операцию, как показано в предыдущем примере. Обратите внимание, что фреймворк игнорирует любой ключ, заданный в этом сценарии, поскольку он не применим (вытесняются данные из всего кэша, а не только одна запись).

Также можно указать, должно ли вытеснение происходить после (по умолчанию) или перед вызовом метода, используя атрибут `beforeInvocation`. Первый вариант обеспечивает ту же семантику, что и остальные аннотации: После успешного завершения метода выполняется действие (в данном случае вытеснение) над кэшем. Если метод не выполняется (поскольку он может быть кэширован) или генерируется исключение, вытеснение не происходит. Последний вариант (`beforeInvocation=true`) приводит к тому, что вытеснение всегда происходить до вызова метода. Это полезно в тех случаях, когда вытеснение не обязательно должно быть связано с результатом выполнения метода.

Обратите внимание, что методы `void` можно использовать с аннотацией `@CacheEvict` – поскольку методы действуют как триггер, возвращаемые значения игнорируются (так как они не взаимодействуют с кэшем). Это не относится к аннотации `@Cacheable`, которая привносит данные в кэш или обновляет данные в кэше и, таким образом, требует получения результата.

This annotation is helpful in evicting (removing) the cache previously loaded. When _@CacheEvict_ annotated methods will be executed, it will clear the cache matched with a cache name, cache key or a condition to be specified.

```java
@CacheEvict(cacheNames="employees", key="#id") 
public void deleteEmployee(Long id) {...}
```

The _@CacheEvict_ annotation offers an extra parameter _‘allEntries’_ for evicting the whole specified cache, rather than a key in the cache.

```java
@CacheEvict(cacheNames="employees", allEntries=true) 
public void deleteAllEmployees() {...}
```

## Аннотация `@Caching` и группировка настроек.

Иногда требуется задать несколько аннотаций одного типа (например, `@CacheEvict` или `@CachePut`) – например, потому что условие или ключевое выражение в разных кэшах разное. Аннотация `@Caching` позволяет использовать несколько вложенных аннотаций `@Cacheable`, `@CachePut` и `@CacheEvict` для одного и того же метода. 

```java
    @Caching(
            cacheable = {
                    @Cacheable("users"),
                    @Cacheable("contacts")
            },
            put = {
                    @CachePut("tables"),
                    @CachePut("chairs"),
                    @CachePut(value = "meals", key = "#user.email")
            },
            evict = {
                    @CacheEvict(value = "services", key = "#user.name")
            }
    )
    void cacheExample(User user) {
    }
```

  
Это единственный способ группировать аннотации.

The _@Caching_ annotation is needed to group multiple annotations when we need to use multiple cache annotations in a single method. In the following example, we are using the _@CacheEvict_ annotation, twice.

```java
@Caching(evict = {
    @CacheEvict(cacheNames = "departments", allEntries = true), 
    @CacheEvict(cacheNames = "employees", key = "...")})
public boolean importEmployees(List<Employee> data) { ... }
```
## Аннотация `@CacheConfig`

До сих пор мы видели, что операции кэширования предполагают множество параметров настройки и что можно задавать эти параметры для каждой операции. Однако некоторые параметры настройки могут быть громоздкими, если применять их ко всем операциям класса. Например, задание имени кэша, который должен использоваться для каждой операции кэширования класса, можно заменить одним определением на уровне класса. Именно здесь в игру вступает аннотация `@CacheConfig`. В следующих примерах `@CacheConfig` используется для установки имени кэша:

`@CacheConfig("books") **(1)** public class BookRepositoryImpl implements BookRepository {     @Cacheable     public Book findBook(ISBN isbn) {...} }`

1. Использование аннотации `@CacheConfig` для установки имени кэша.

`@CacheConfig` – это аннотация на уровне класса, которая позволяет совместно использовать имена кэша, кастомный `KeyGenerator`, кастомный CacheManager и кастомный CacheResolver . Размещение этой аннотации на классе не активирует никаких операций кэширования.

Настройка на уровне операции всегда переопределяет настройку, установленную для аннотации `@CacheConfig`. Следовательно, это дает нам три уровня настройки для каждой операции кэширования:

- `KeyGenerator`, конфигурируемый глобально и доступный для `CacheManager`
- На уровне класса, используя аннотацию `@CacheConfig`.
- На уровне операций.
    
This annotation allows us to specify some of the cache configurations at the class level, so we do not have to repeat them multiple times over each method.

```java
@Service
@CacheConfig(cacheNames={"employees"})
public class EmployeeService {

  @Autowired
  EmployeeRepository repository;

  @Cacheable(key = "#id")
  public Optional<Employee> getEmployeeById(Long id) { ... }

  @CachePut(key = "#employee.id")
  public Employee updateEmployee(Employee employee) { ... }
}
```
## Гибкая настройка. CacheManager

- _**SimpleCacheManager**_ — самый простой кэш-менеджер, удобный для изучения и тестирования.
- _**ConcurrentMapCacheManager**_ — лениво инициализирует возвращаемые экземпляры для каждого запроса. Также рекомендуется для тестирования и изучения работы с кэшем, а также, для каких-то простых действий, вроде наших. Для серьёзной работы с кэшем рекомендуются имплементации ниже.
- _**JCacheCacheManager**_, _**EhCacheCacheManager**_, _**CaffeineCacheManager**_ — серьёзные кэш-менеджеры «от партнёров», гибко настраиваемые и выполняющие задачи очень широкого спектра действия.

![](../../../_res/Pasted%20image%2020250313165041.png)


## Resources

- [Декларативное кэширование на основе аннотаций](https://javarush.com/quests/lectures/questspring.level06.lecture22)
- [Spring Cache: от подключения кэширования за 1 минуту до гибкой настройки кэш-менеджера](https://habr.com/ru/articles/465667/)
- [Spring Boot Caching with Example](https://howtodoinjava.com/spring-boot/spring-boot-cache-example/)
- 