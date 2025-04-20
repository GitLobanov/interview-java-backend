#### Какие требования к Entity

[Требования Entity](../../../_inforage/Hibernate/03%20Storage/Требования%20Entity.md)
#### Какие есть состояния Entity

[Состояние entity](../../../_inforage/Hibernate/03%20Storage/Состояние%20entity.md)

#### Почему нельзя создать final класс Entity?

- от него нельзя наследоваться: без возможности расширения для добавления новых атрибутов или поведения, придется смешать все в одном классе, то нарушает принцип единой ответственности.
- hibernate, может понадобиться реализовывать inheritance
#### Какие основные преимущества использования JPA по сравнению с JDBC?

- **JPA** предоставляет **абстракцию** над базой данных, скрывая детали реализации SQL-запросов, соединений и транзакций. С помощью JPA можно работать с объектами Java (сущностями) вместо того, чтобы напрямую управлять строками SQL-запросов.
- **JDBC** требует явного написания SQL-запросов для выполнения операций с базой данных. Это увеличивает количество кода и делает его сложным для обслуживания.

#### Какая связь JPA с Hibernate?

- Jpa - спецификации, Hibernate - реализация, которая строиться на спецификаци
#### Что появилось раньше JPA или Hibernate

- Hibernate, потом JPA, после чего Hibernate зарефакторился под спецификацию

#### Какие виды кэша есть в Hibernate и как их использовать

- Первый уровень (Session Cache)
	- Это кэш, связанный с текущей сессией. Он используется для хранения сущностей, загруженных в рамках одной сессии.
- Второй уровень (Second-level Cache)
	- Это кэш, который работает на уровне приложения, виден всем сессиям.
	- hibernate.cache.use_second_level_cache=true
- Кэш запросов (Query Cache)
	- Этот кэш используется для хранения результатов запросов, чтобы избежать повторных выполнения тех же запросов в БД.
	- `hibernate.cache.use_query_cache=true`.
- https://www.baeldung.com/hibernate-second-level-cache

#### Стратегии кэширования кэша 2 уровня

- **_READ_ONLY_**: Used only for entities that never change (an exception is thrown if an attempt to update such an entity is made). It’s very simple and performative. It’s suitable for static reference data that doesn’t change.
- **_NONSTRICT_READ_WRITE_**: Cache is updated after the transaction that changed the affected data has been committed. Thus, strong consistency isn’t guaranteed, and there’s a small time window in which stale data may be obtained from the cache. This kind of strategy is suitable for use cases that can tolerate eventual consistency.
- **_READ_WRITE_**: This strategy guarantees strong consistency, which it achieves by using ‘soft’ locks. When a cached entity is updated, a soft lock is stored in the cache for that entity as well, which is released after the transaction is committed. All concurrent transactions that access soft-locked entries will fetch the corresponding data directly from the database.
- **_TRANSACTIONAL_**: Cache changes are done in distributed XA transactions. A change in a cached entity is either committed or rolled back in both the database and cache in the same XA transaction.

#### По какому значению кэшируются сущности по первому уровня кэша?

- Хибернейт кэширует по id, и ложит в мапу
#### Какие варианты настройки есть для второго уровня кэша?

- Выбор concurrent стратегии
- Выбор провайдера: EhCache, JCache

#### Какие вы знаете стратегии генерации ID, предоставляемые JPA?

```java
---- 1
@GeneratedValue(strategy = GenerationType.IDENTITY)

---- 2
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "user_seq")
@SequenceGenerator(name = "user_seq", sequenceName = "user_sequence", allocationSize = 1)

---- 3
@GeneratedValue(strategy = GenerationType.TABLE, generator = "user_table")
@TableGenerator(name = "user_table", table = "id_generator", pkColumnName = "gen_name", valueColumnName = "gen_val", allocationSize = 1)

---- 4
@GeneratedValue(strategy = GenerationType.AUTO)

---- 5 since Hiber 5.4+
@GeneratedValue(strategy = GenerationType.UUID)
```

#### Стратегия генерации по умолчанию?

- @GeneratedValue(strategy = GenerationType.AUTO)

#### Как можно интегрировать пользовательские SQL запросы в JPA?

#### Какие есть стратегии наследования?

- [Hibernate наследование](../../../_inforage/Hibernate/03%20Storage/Hibernate%20наследование.md)

#### Расскажите что такое FetchType?

**`FetchType`** — это параметр, который управляет стратегией загрузки данных в JPA
#### Какой дефолтный fetchType в Hibernate?

OneToMany: LAZY
ManyToOne: EAGER
ManyToMany: LAZY
OneToOne: EAGER

#### Какие основные причины возникновения LazyInitializationException?


#### Какие есть способы предотвращения LazyInitializationException без изменения стратегии загрузки?

####  Как решается LazyInitializationException

[Lazy-load в Hibernate](../../../_inforage/Hibernate/03%20Storage/Lazy-load%20в%20Hibernate.md)

#### Как возникает и решается N+1

- Сначала загружается **основная сущность** (например, родительская сущность).
- Для каждой родительской сущности выполняется отдельный запрос для загрузки **связанных сущностей** (например, дочерних).
- Например, если у вас есть сущности **`Order`** и **`OrderItem`**, то для каждого **`Order`** может быть выполнен отдельный запрос для загрузки **`OrderItem`**, что приведет к большому числу запросов, если заказов много.

[Проблема N+1. Решения](../../../_inforage/Hibernate/03%20Storage/Проблема%20N+1.%20Решения.md)

#### Назовите хотя бы одну альтернативу Hibernate.

- Jooc
- EclipseLink

#### Почему в JPA репозиторий, нету метода обновить с чем это связано?

- В JPA нет отдельного метода `update()`, потому что управление сущностями происходит через механизм состояний (managed entity).
- Потому что метод `save()` уже все это делает. Когда ты вызываешь `save()`, JPA проверяет, существует ли объект в базе данных. Если объект новый — он будет вставлен как новая запись, если уже существует — он обновляется. Поэтому нет необходимости в отдельном методе `update()`, так как этот процесс автоматизирован через `save()`.

#### Что если у нас открыта транзакция, нужно ли использовать метод save?

- Если мы вешаем транзакцию над методом, достаем сущность, изменяем то после коммита транзакции данные сохраняться  в бд и без использования метода save 

#### Что такое EntityManager?

**`EntityManager`** — это основной интерфейс для взаимодействия с базой данных в JPA (Java Persistence API). Он выполняет аналогичную роль, что и **`Session`** в Hibernate, но в контексте стандарта JPA.

- **`persist(Object entity)`**: Сохраняет новую сущность в базу данных.
- **`find(Class<T> entityClass, Object primaryKey)`**: Находит сущность по первичному ключу.
- **`remove(Object entity)`**: Удаляет сущность из базы данных.
- **`merge(Object entity)`**: Обновляет (сливает) состояние сущности с базой данных.
- **`createQuery(String qlString)`**: Создает запрос на основе JPQL или SQL.
- **`getTransaction()`**: Возвращает транзакцию для явного управления.

#### Чем отличаются NamedEntityGraph от EntityGraph?

- **`NamedEntityGraph`** позволяет заранее определить граф сущностей и его атрибуты, который затем можно использовать в разных частях приложения. Этот граф может быть заранее именован, а затем ссылаться на него через аннотацию **`@EntityGraph`** в методах репозитория.

```java
@Entity
@NamedEntityGraph(name = "Item.characteristics",
    attributeNodes = @NamedAttributeNode("characteristics")
)
public class Item {
...

public interface ItemRepository extends JpaRepository<Item, Long> {
    @EntityGraph(value = "Item.characteristics")
    Item findByName(String name);
}
```

- Здесь используется **`EntityGraph`** с атрибутом **`attributePaths`**, чтобы указать, что нужно загрузить **`characteristics`** при выполнении запроса.

```java
@Entity
public class Item {
...
@OneToMany(mappedBy = "item")
    private Set<Characteristic> characteristics;
...
public interface ItemRepository extends JpaRepository<Item, Long> {
    @Query("SELECT i FROM Item i WHERE i.name = :name")
    @EntityGraph(attributePaths = {"characteristics"})
    Item findByNameWithCharacteristics(@Param("name") String name);
}

```

#### JPA CascadeType.REMOVE vs orphanRemoval

- As stated earlier, marking a reference field with _CascadeType.REMOVE_ is a way to **delete a** **child entity or entities** **whenever the deletion of its parent happens**.
- **`orphanRemoval`** — это свойство, которое указывает, что дочерняя сущность должна быть удалена, если она больше не связана с родительской сущностью. То есть, если объект был отсоединен от родителя, он автоматически будет удален в базе.
- https://www.baeldung.com/jpa-cascade-remove-vs-orphanremoval

```java
@Entity
public class OrderRequest {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @OneToOne(cascade = { CascadeType.REMOVE, CascadeType.PERSIST })
    private ShipmentInfo shipmentInfo;

    @OneToMany(orphanRemoval = true, cascade = CascadeType.PERSIST, mappedBy = "orderRequest")
    private List<LineItem> lineItems;

    public void removeLineItem(LineItem lineItem) {
        lineItems.remove(lineItem);
    }
}
```

#### Как реализовать динамический запрос с фильтрацией и с опцианальными параметрами?

- JPA Specification
- Creteria API
- Query DSL

#### Почему нельзя делать класс и поля final в сущности?

- **Класс `final`**: Если сам класс сущности является `final`, Hibernate не сможет создать для него прокси, а значит, не сможет корректно работать с ленивой загрузкой и другими функциональностями, требующими проксирования.
#### Если у нас поле установлено как Lazy, как хибер понимает что нужно заполнить коллекцию дочерних?

- Прокси объект сам делает новый запрос к бд за элементами, когда включена сессия

#### Что хиберейнт использует для создания прокси Dynamic or CGLIB?

- Смотрит есть ли интерфейс, тогда Dynamic
- Нету, наследуется от класс
- После 5.2 версии хибера, использует Byte Buddy — для создания прокси через байт-код, что позволяет лучше управлять созданием проксей и улучшать производительность.

#### Что делает оператор SELECT FOR UPDATE и для чего он используется?

- если запросить явно `FOR SHARE` - то читать мы сможем в много потоков с этой `FOR SHARE` блокировкой не блокируя друг друга. Но вот если кто-то захочет обновить эту строку - то он встанет в очередь ожидания пока не завершатся все транзакции удерживающие читающую блокировку и вместе с тем задержит все последующие `FOR SHARE` транзакции.
- если запросить `FOR UPDATE` - то мы можем быть уверены, что ни одна другая транзакция не сможет обновить эту строку до конца нашей транзакции.
#### Как реализовать оптимистичную блокировку в Hibernate?

1. `LockModeType.OPTIMISTIC` - блокировка по умолчанию для всех сущностей, помеченных аннотацией `@Version`.
2. `LockModeType.OPTIMISTIC_FORCE_INCREMENT` - блокировка полностью аналогичная прошлой, но проверяет версии дочерних элементов (например, связных с помощью отношений один ко многим).

#### Как реализовать пессимистичную блокировку в Hibernate?

1. `LockModeType.PESSIMISTIC_READ` - блокирует любое изменение записи до тех пор пока транзакция не завершит свою работу, но при этом разрешает другим сущностям её читать. Позволяет не допустить “грязное чтение”. Это называется общей блокировкой, так как блокируется запись одной транзакцией, но доступ для чтения разрешен для всех.
2. `LockModeType.PESSIMISITIC_WRITE` - блокирует любое чтение и изменение записи до тех пор пока транзакция не завершит свою работу. Это называется эксклюзивная блокировка, так как предоставляет запись только для одной транзакции.
3. `LockModeType.PESSIMISTIC_FORCE_INCREMENT` - совмещение пессимистической `write` блокировки и оптимистической. Позволяет предотвратить ситуации, когда сначала первая транзакция считала сущность, затем мы пессимистично захватили эту же запись во второй транзакции, завершились быстрее чем первая транзакция, то первая транзакция перетрет наши данные.

Также в рамках пессимистической блокировки есть понятие `PessimisticLockScope`. Поэтому чисто символически я его упомяну. У него есть два значения:

1. `PessimisticLockScope.NORMAL` - блокирует исключительно саму запись (если есть наследование, то блокируются все субтаблицы).
2. `PessimisticLockScope.SHARED` - блокирует саму запись и все её связи.


###### Какие ORM знаешь и с какими работал?


###### Можно ли поставить аннотацию @Data над Entity?


###### Какие проблемы бывают в Hibernate?

- n+1
- LazyInitException

###### ConnectionPool


###### Какие есть дополнительные параметры у @OneToMany @ManyToOne и что они делают?


###### Зачем нужен fetchType? Какие по умолчанию для каждого типа?

###### Приходилось ли использовать JPA CRITERIA API? В чем отличие от SQL запроса?

###### Есть 2 метода 1- на select данных в бд, 1 - update данных в таблице, они разъедены во времени. Может быть несколько пользователей, которые работают с этими методами асинхронно. Что можно предложить, чтобы сохранить консистентность данных?



######  Почему способ хранения Single Table не лучший для хранения данных

###### отличия btree bндекса от бинарного и для чего они нужны

###### Что такое type в EntityGraph?

###### Сколько потоков Hiraki?

`spring.datasource.hikari.maximum-pool-size=10` 
`spring.datasource.hikari.minimum-idle=10` 
###### Настройки hikari

```java
spring.datasource.hikari.connection-timeout=50000 
spring.datasource.hikari.idle-timeout=300000 
spring.datasource.hikari.max-lifetime=900000 
spring.datasource.hikari.maximum-pool-size=10 
spring.datasource.hikari.minimum-idle=10 
spring.datasource.hikari.pool-name=ConnPool 

spring.datasource.hikari.connection-test-query=select 1 from dual spring.datasource.hikari.data-source-properties.cachePrepStmts=true spring.datasource.hikari.data-source-properties.prepStmtCacheSize=250 spring.datasource.hikari.data-source-properties.prepStmtCacheSqlLimit=2048 spring.datasource.hikari.data-source-properties.useServerPrepStmts=true spring.datasource.hikari.data-source-properties.useLocalSessionState=true spring.datasource.hikari.data-source-properties.rewriteBatchedStatements=true spring.datasource.hikari.data-source-properties.cacheResultSetMetadata=true spring.datasource.hikari.data-source-properties.cacheServerConfiguration=true spring.datasource.hikari.data-source-properties.elideSetAutoCommits=true spring.datasource.hikari.data-source-properties.maintainTimeStats=false
```