#### Какие требования к Entity [X]

[Требования Entity](../../../_inforage/Hibernate/03%20Storage/Требования%20Entity.md)
#### Какие есть состояния Entity [X]

[Состояние entity](../../../_inforage/Hibernate/03%20Storage/Состояние%20entity.md)

#### Какие типы отношений вы знаете ? [X]

1. @OneToOne (один к одному)
    - Пример: User ↔ Passport (у одного пользователя один паспорт и наоборот).
2. @OneToMany (один ко многим)
    - Пример: Department ↔ Employee (в одном отделе много сотрудников).
3. @ManyToOne (многие к одному)
    - Обратная сторона @OneToMany. Пример: Employee ↔ Department (многие сотрудники работают в одном отделе).
4. @ManyToMany (многие ко многим)
    - Пример: Student ↔ Course (студент может записаться на много курсов, и на одном курсе много студентов).

#### В чем разница между однонаправленными и двунаправленными отношениями ? [X]

| Характеристика | Однонаправленное (Unidirectional)                    | Двунаправленное (Bidirectional)                            |
| -------------- | ---------------------------------------------------- | ---------------------------------------------------------- |
| Навигация      | Доступ только с одной стороны.                       | Доступ с обеих сторон.                                     |
| Аннотации      | Только на одной сущности.                            | На обеих сущностях (+ `mappedBy`).                         |
| Пример         | `User` → `Address` (но `Address` не знает о `User`). | `User` ↔ `Address` (обе сущности ссылаются друг на друга). |
| Плюсы          | Проще в реализации.                                  | Удобнее для запросов (можно идти от любой стороны).        |
| Минусы         | Ограниченная гибкость.                               | Требует синхронизации (например, при `cascade`).           |
Пример двунаправленного `@OneToMany`:

```java
@Entity
public class Department {
    @Id private Long id;

    @OneToMany(mappedBy = "department")  // "department" — поле в Employee
    private List<Employee> employees;
}

@Entity
public class Employee {
    @Id private Long id;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;  // Владелец отношения (содержит внешний ключ)
}
```

#### Как лучше всего сделать отношение @OneToMany ? [X]

Рекомендации для `@OneToMany`:

1. Используйте `List` вместо `Set` (если порядок важен).
2. Добавьте `@JoinColumn` (если не хотите промежуточную таблицу):

```java
@OneToMany
@JoinColumn(name = "department_id")  // Создаст столбец в таблице Employee
private List<Employee> employees;
```
    
3. Или используйте `mappedBy` в двунаправленной связи.

```java
@ManyToOne
@JoinColumn(name = "department_id")
private Department department; 
```

4. Избегайте `EAGER` (лучше `LAZY` + `JOIN FETCH` в запросах).
5. Для производительности можно:
    - Использовать `@BatchSize` для ленивой загрузки.
    - Добавить `@OrderBy` для сортировки.

#### В чем разница между eager и lazy ? [X]

| Характеристика | `FetchType.EAGER`                                    | `FetchType.LAZY`                                                                 |
| -------------- | ---------------------------------------------------- | -------------------------------------------------------------------------------- |
| Загрузка       | Данные загружаются сразу с сущностью.                | Данные загружаются только при обращении.                                         |
| Использование  | По умолчанию для `@ManyToOne`, `@OneToOne`.          | По умолчанию для `@OneToMany`, `@ManyToMany`.                                    |
| Плюсы          | Удобно, если данные всегда нужны.                    | Экономит память и ускоряет загрузку.                                             |
| Минусы         | Может вызывать N+1 проблему.                         | Может привести к `LazyInitializationException` (если обращаться вне транзакции). |
| Рекомендация   | Почти всегда лучше `LAZY` + `JOIN FETCH` в запросах. | Используйте `LAZY` и явные запросы с `JOIN FETCH`.                               |

#### Какой дефолтный fetchType в Hibernate ? [X]

- Для @OneToMany и @ManyToMany: LAZY.
- Для @ManyToOne и @OneToOne: EAGER.

#### Что лучше set или list указывать в ассоциации Hibernate [X]

Выбор между Set и List для указания в ассоциации зависит от конкретных требований и ситуации.  **Для ассоциаций ManyToMany обычно предпочтительнее Set**. Он не содержит дубликатов и генерирует более эффективные SQL-запросы. **List** **может быть более эффективным** **для ассоциаций OneToMany**, так как не требует дополнительных проверок на дублирование данных, если список может содержать их. Также стоит учитывать, что тип данных влияет на то, как Hibernate управляет ассоциацией в базе данных. Например, для ассоциаций ManyToMany не рекомендуется использовать List, так как при этом Hibernate неэффективно удаляет связанные объекты.

#### Есть ли разница в том использовать в качестве коллекции Set или List для отношений ? [X]

| Критерий                     | `List`                                                       | `Set`                                                      |
| ---------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------- |
| Дубликаты                    | Разрешены.                                                   | Запрещены (уникальность по `equals/hashCode`).             |
| Порядок                      | Сохраняет порядок вставки (если не использовать `@OrderBy`). | Не гарантирует порядок (если не `LinkedHashSet`).          |
| Производительность           | Медленнее при проверке дубликатов (`contains()` — O(n)).     | Быстрее (`contains()` — O(1) для `HashSet`).               |
| Использование в `@OneToMany` | Требует `@JoinColumn` или `@OrderColumn`.                    | Часто используется с `mappedBy` (лучше для `@ManyToMany`). |
| Ленивая загрузка             | Может загружать элементы по индексу частично.                | Всегда загружает весь `Set` сразу.                         |

#### Что такое механизм dirty checking ? [X]

Dirty Checking — это автоматическое отслеживание изменений в сущностях Hibernate и генерация SQL-запросов для обновления только изменившихся полей.

1. При загрузке сущности (например, через `session.get()` или `entityManager.find()`) Hibernate сохраняет её "снимок" (snapshot) в кеше первого уровня (Persistence Context).
2. Перед коммитом транзакции Hibernate сравнивает текущее состояние сущности с этим снимком.
3. Если есть изменения — автоматически генерируется SQL `UPDATE` только для изменённых полей.

```java
User user = session.get(User.class, 1L);  // Загружаем и сохраняем снимок
user.setName("New Name");               // Меняем поле → Dirty Checking обнаружит это
transaction.commit();                   // Автоматически выполнит UPDATE users SET name = ? WHERE id = 1
```

#### Как запретить изменения dirty checking ? [X]

```java
// 1 updatable = false
@Entity
public class User {
    @Column(updatable = false)  // Это поле нельзя изменить через Dirty Checking
    private String registrationDate;
}

// 2 evict()
User user = session.get(User.class, 1L);
user.setName("New Name");
session.evict(user);  // Удаляем из контекста → изменения не сохранятся
transaction.commit(); // Никакого UPDATE не будет
```

#### Что такое connection pool и зачем нужен ? [X]

Connection Pool — это пул заранее созданных подключений к БД, которые переиспользуются.

- Установка нового подключения к БД — дорогая операция (TCP-рукопожатие, аутентификация).
- Пул избегает создания/закрытия соединений для каждого запроса.

Как работает:
1. При старте приложения создается N подключений.
2. Когда приложение запрашивает соединение — берется из пула.
3. После использования — возвращается в пул (а не закрывается).

#### Как лучше всего сделать equals и hashCode в Hibernate ? [X]

1. Не использовать `id` в `equals/hashCode`, если он `@GeneratedValue` (до сохранения `id == null`).
2. Использовать неизменямое бизнес-поле (например, `email` для `User`).
3. Синхронизация с БД: Методы должны давать одинаковый результат до/после сохранения.

#### Какие вы знаете стратегии генерации ID, предоставляемые JPA ? [X]

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

#### Как наиболее эффективно реализовать генерацию Id?

#### Почему нельзя создать final класс Entity ? [X]

Если у вас есть `final` сущность (entity) в JPA, вы не сможете использовать такие функции, как наследование или проксирование, которые часто требуются для различных оптимизаций JPA, таких как ленивую загрузку.

- JPA зависит от возможности создавать прокси (особенно для ленивой загрузки) и подклассы. Поставщик JPA (например, Hibernate) часто создает прокси-подкласс сущности для выполнения определенных операций, таких как ленивое подключение ассоциаций.
- Спецификация JPA предполагает, что сущности могут быть подклассифицированы или динамически изменены поставщиком постоянства (persistence provider). Если класс помечен как `final`, это ограничивает гибкость. Кроме того, поставщики постоянства часто используют рефлексию для проверки и манипуляции с сущностями. Если класс `final`, некоторые операции, основанные на рефлексии, могут быть ограничены или не работать как ожидалось, особенно в случаях, связанных с наследованием или определенными механизмами конфигурации, которые должны быть расширяемыми.
- Поставщики JPA, такие как Hibernate, используют улучшение байткода (особенно во время выполнения) для оптимизации и изменения классов сущностей. Если класс сущности помечен как `final`, это предотвращает подобное улучшение, так как ограничивает возможность изменения поведения класса или добавления функциональности динамически.

#### Что такое JPQL/HQL ? [X]

Hibernate Query Language (HQL) и Java Persistence Query Language (JPQL) - оба являются объектно-ориентированными языками запросов, схожими по природе с SQL. JPQL - это подмножество HQL. JPQL-запрос всегда является допустимым HQL- запросом, однако обратное неверно.

- JPQL — это язык запросов, который является частью спецификации JPA. Он используется для создания запросов к объектам Java.
- HQL — это аналогичный язык запросов в Hibernate, но с некоторыми особенностями, ориентированными на работу с Hibernate.

```sql
SELECT b FROM Book b WHERE b.title = :title
```

#### Что такое Criteria API ? [X]

Criteria API — [это другой способ работы с запросами в JPA и Hibernate](https://telegra.ph/36CHto-takoe-Criteria-API-i-dlya-chego-on-ispolzuetsya-01-29), который основан на Java-объектах и типизированных запросах, а не на строках запросов. Criteria API позволяет создавать запросы программным путем, без использования строковых выражений, что помогает избежать ошибок при компиляции запросов и улучшает рефакторинг кода.

```java
CriteriaBuilder cb = entityManager.getCriteriaBuilder();
CriteriaQuery<Book> cq = cb.createQuery(Book.class);
Root<Book> book = cq.from(Book.class);
cq.select(book).where(cb.equal(book.get("title"), title));
TypedQuery<Book> query = entityManager.createQuery(cq);
```

#### JPQL/HQL vs Criteria API ? [X]

- JPQL/HQL — строковые запросы, которые читаются и пишутся, как SQL.
- Criteria API — позволяет строить запросы программно, что помогает избежать ошибок синтаксиса и улучшает автодополнение в IDE.
#### Что такое JPA? Что такое Hibernate ? [X]

- JPA (Java Persistence API) — это спецификация Java для работы с реляционными базами данных, которая позволяет работать с базами данных с помощью объектно-ориентированного подхода. JPA описывает стандартные интерфейсы и аннотации для создания и управления персистентными объектами. JPA не является реализацией, а лишь спецификацией, и требует реализации, такой как Hibernate.
- Hibernate — это фреймворк, который реализует спецификацию JPA и предоставляет дополнительные возможности для работы с базой данных, такие как кеширование, расширенные функции запросов и оптимизация производительности. Hibernate — это одна из самых популярных реализаций JPA.

#### Что такое JDBC ? [X]

JDBC (Java Database Connectivity) — это низкоуровневая технология для работы с базами данных в Java. Через JDBC программисты пишут SQL-запросы вручную и управляют соединениями, транзакциями и результатами запросов.


#### Что такое Hibernate ? [X]

Hibernate — это высокоуровневая библиотека, которая управляет персистентностью объектов Java и их маппингом в реляционные базы данных. Hibernate автоматически генерирует SQL-запросы, упрощая работу с базой данных и позволяя фокусироваться на бизнес-логике.

#### JDBC vs Hibernate ? [X]

Преимущества JDBC:
- Полный контроль над SQL-запросами и производительностью.
- Подходит для простых случаев без необходимости в сложной логике работы с объектами.
    
Недостатки JDBC:
- Требуется много кода для работы с результатами запросов и объектами.
- Нет автоматической маппинга между объектами Java и записями в базе данных.

Преимущества Hibernate:
- Автоматический маппинг объектов в записи базы данных.
- Поддержка различных стратегий кеширования, ленивой загрузки и других оптимизаций.
- Упрощение работы с транзакциями и запросами.

Недостатки Hibernate:
- Менее гибок в плане производительности, если требуется полный контроль над SQL-запросами.

#### Как перехватить изменения в Entity ? [X]

Для перехвата изменений в сущности в JPA/Hibernate можно использовать Entity Listeners и @PrePersist, @PostPersist, @PreUpdate, @PostUpdate аннотации.

Entity Listeners — это классы, которые содержат методы, которые перехватывают события жизненного цикла сущности (например, сохранение, обновление, удаление).


```java
@Entity
@EntityListeners(MyEntityListener.class)
public class MyEntity {
    // поля и методы
}

public class MyEntityListener {
    @PrePersist
    public void prePersist(MyEntity entity) {
        // действия до сохранения сущности
    }

    @PostPersist
    public void postPersist(MyEntity entity) {
        // действия после сохранения сущности
    }
}

```

Аннотации жизненного цикла: `@PrePersist`, `@PostPersist`, `@PreUpdate`, `@PostUpdate`, `@PreRemove`, `@PostRemove` могут быть использованы для выполнения операций до или после конкретных изменений сущности (например, перед сохранением или после обновления).

```java
@Entity
public class Book {
    @Id
    private Long id;
    
    @PrePersist
    public void onPrePersist() {
        System.out.println("Entity is about to be persisted.");
    }
}
```

#### Что такое Hibernate envers ? [X]

Hibernate Envers — это компонент Hibernate, который позволяет отслеживать изменения сущностей с течением времени и сохранять историю изменений. Это помогает организовать аудит изменений в приложениях, где важно знать, кто и когда изменил данные.

Основные возможности Hibernate Envers:
- Автоматическое отслеживание изменений в сущностях (сохранение истории изменений).
- Поддержка создания версий сущностей.
- Возможность получения предыдущих состояний сущностей.

```java
@Entity
@Audited  // Включает аудит изменений для этой сущности
public class Book {
    @Id
    private Long id;
    private String title;
    private String author;
}
```

После того как вы добавите аннотацию `@Audited` и сделаете несколько изменений в сущности (например, сохраните, обновите или удалите объект), Hibernate Envers будет автоматически записывать изменения в таблицу аудита.

| REVISION | REVISION_TYPE | ID  | TITLE     | AUTHOR     |
| -------- | ------------- | --- | --------- | ---------- |
| 1        | ADD           | 1   | "Book A"  | "Author 1" |
| 2        | MOD           | 1   | "Book A+" | "Author 1" |
| 3        | ADD           | 2   | "Book B"  | "Author 2" |
- REVISION: это идентификатор ревизии, уникальный для каждой операции изменения.
- REVISION_TYPE: тип операции — `ADD` (добавление), `MOD` (изменение), `DEL` (удаление).
- ID, TITLE, AUTHOR: это значения полей сущности на момент изменения.

[Entity auditing with Hibernate Envers](https://golb.hplar.ch/2019/08/envers.html)
#### Какие основные преимущества использования JPA по сравнению с JDBC ? [X]

- JPA предоставляет абстракцию над базой данных, скрывая детали реализации SQL-запросов, соединений и транзакций. С помощью JPA можно работать с объектами Java (сущностями) вместо того, чтобы напрямую управлять строками SQL-запросов.
- JDBC требует явного написания SQL-запросов для выполнения операций с базой данных. Это увеличивает количество кода и делает его сложным для обслуживания.

#### Какая связь JPA с Hibernate?

- JPA - спецификация, Hibernate - реализация, которая строиться на спецификаци
#### Что появилось раньше JPA или Hibernate

- Hibernate, потом JPA, после чего Hibernate зарефакторился под спецификацию

#### Какие виды кэша есть в Hibernate и как их использовать [X]

- Первый уровень (Session Cache)
	- Это кэш, связанный с текущей сессией. Он используется для хранения сущностей, загруженных в рамках одной сессии.
- Второй уровень (Second-level Cache)
	- Это кэш, который работает на уровне фабрики сессий, виден всем сессиям.
	- hibernate.cache.use_second_level_cache=true
	- нужен сторонний провайдер (ex. ehcache, Infinispan)
- Кэш запросов (Query Cache)
	- Этот кэш используется для хранения результатов запросов, чтобы избежать повторных выполнений тех же запросов в БД.
	- `hibernate.cache.use_query_cache=true`.
	
- https://www.baeldung.com/hibernate-second-level-cache
- [Как работать с кэшем 2 уровня?](https://telegra.ph/34-Kak-rabotat-s-kehshem-2-urovnya-01-29)

#### Стратегии кэширования кэша 2 уровня [X]

- _READ_ONLY_: Used only for entities that never change (an exception is thrown if an attempt to update such an entity is made). It’s very simple and performative. It’s suitable for static reference data that doesn’t change.
- _NONSTRICT_READ_WRITE_: Cache is updated after the transaction that changed the affected data has been committed. Thus, strong consistency isn’t guaranteed, and there’s a small time window in which stale data may be obtained from the cache. This kind of strategy is suitable for use cases that can tolerate eventual consistency.
- _READ_WRITE_: This strategy guarantees strong consistency, which it achieves by using ‘soft’ locks. When a cached entity is updated, a soft lock is stored in the cache for that entity as well, which is released after the transaction is committed. All concurrent transactions that access soft-locked entries will fetch the corresponding data directly from the database.
- _TRANSACTIONAL_: Cache changes are done in distributed XA transactions. A change in a cached entity is either committed or rolled back in both the database and cache in the same XA transaction.

```java
@Entity
@Cacheable
@Cache(usage = CacheConcurrencyStrategy.READ_WRITE)  // Стратегия кэширования
public class Product {
    @Id
    private Long id;
    // ...
}
```

```java
@Entity
public class Order {
    @Id
    private Long id;

    @OneToMany
    @Cache(usage = CacheConcurrencyStrategy.READ_WRITE) // для коллекции
    private List<OrderItem> items;
}
```

#### Для чего нужна аннотация @Access? [X]

Hibernate использует два способа доступа к данным сущности:
1. Field access – аннотации (`@Id`, `@Column` и др.) ставятся над полями, Hibernate работает с ними напрямую.
2. Property access – аннотации ставятся над геттерами, Hibernate использует геттеры/сеттеры для доступа к полям.

По умолчанию тип доступа определяется тем, где стоит `@Id`:
- Над полем → `AccessType.FIELD`.
- Над геттером → `AccessType.PROPERTY`.

Зачем нужна `@Access`?
- Чтобы явно указать тип доступа (`FIELD` или `PROPERTY`) для сущности или отдельных полей.
- Позволяет смешивать оба типа в одной иерархии классов (например, у родителя `FIELD`, а у наследника `PROPERTY`).

#### Для чего нужна аннотация @Cacheable ? [X]

@Cacheable  -  аннотация  JPA,  используется  для  указания  того,  должна  ли сущность храниться в кэше второго уровня, в случае, если в файле persistence.xml (или в  свойстве  javax.persistence.sharedCache.mode  конфигурационного  файла)  для элемента shared-cache-mode установлено одно из значений:

- ENABLE_SELECTIVE: только сущности с аннотацией @Cacheable (равносильно значению  по  умолчанию  @Cacheable(value=true))  будут  сохраняться  в  кэше второго уровня.
- DISABLE_SELECTIVE: все сущности будут сохраняться в кэше второго уровня, за  исключением  сущностей,  помеченных  аннотацией  @Cacheable(value=false) как некэшируемые.
- ALL: сущности всегда кэшируются, даже если они помечены как некэшируемые.
- NONE: ни одна сущность не кэшируется, даже если помечена как кэшируемая. При данной опции имеет смысл вообще отключить кэш второго уровня.
- UNSPECIFIED: применяются значения по умолчанию для кэша второго уровня, определенные Hibernate. Это эквивалентно тому, что вообще не используется shared-cache-mode,  так  как  Hibernate  не  включает  кэш  второго  уровня,  если используется режим UNSPECIFIED.

Аннотация  @Cacheable  размещается  над  классом  сущности.  Её  действие распространяется на эту сущность и её наследников, если они не определили другое поведение.

```yml
spring:
  jpa:
    properties:
      hibernate:
        cache:
          use_second_level_cache: true
          region.factory_class: org.hibernate.cache.jcache.JCacheRegionFactory
      javax.persistence.sharedCache.mode: ENABLE_SELECTIVE
```

#### По какому значению кэшируются сущности по первому уровня кэша?

- Хибернейт кэширует по id, и ложит в мапу
#### Какие варианты настройки есть для второго уровня кэша?

- Выбор concurrent стратегии
- Выбор провайдера: EhCache, JCache

#### Как с помощью Hibernate делать валидацию сущностей?

#### Как сделать составной ключ?

#### Что такое проекции?

#### Что такое репозиторий? Как сделать кастомный запрос в БД?

#### Расскажите про Spring Data JPA? Какие преимущества у данной технологии?

#### Как можно интегрировать пользовательские SQL запросы в JPA?

#### Какие есть стратегии наследования?

- Single Table
- Table per Classs
- Joined
- [Hibernate наследование](../../../_inforage/Hibernate/03%20Storage/Hibernate%20наследование.md)

#### Расскажите что такое FetchType?

**`FetchType`** — это параметр, который управляет стратегией загрузки данных в JPA

#### Какие основные причины возникновения LazyInitializationException?


#### Какие есть способы предотвращения LazyInitializationException без изменения стратегии загрузки?

####  Как решается LazyInitializationException

[Lazy-load в Hibernate](../../../_inforage/Hibernate/03%20Storage/Lazy-load%20в%20Hibernate.md)

#### Как возникает и решается N+1 [X]

- Сначала загружается основная сущность (например, родительская сущность).
- Для каждой родительской сущности выполняется отдельный запрос для загрузки связанных сущностей (например, дочерних).
- Например, если у вас есть сущности `Order` и `OrderItem`, то для каждого `Order` может быть выполнен отдельный запрос для загрузки `OrderItem`, что приведет к большому числу запросов, если заказов много.

[Проблема N+1. Решения](../../../_inforage/Hibernate/03%20Storage/Проблема%20N+1.%20Решения.md)
[Расскажите про проблему N+1 Select и путях ее решения.](https://telegra.ph/37-Rasskazhite-pro-problemu-N1-Select-i-putyah-ee-resheniya-01-29)
#### Назовите хотя бы одну альтернативу Hibernate.

- Jooc
- EclipseLink



#### Что если у нас открыта транзакция, нужно ли использовать метод save?

- Если мы вешаем транзакцию над методом, достаем сущность, изменяем то после коммита транзакции данные сохраняться  в бд и без использования метода save 

#### Что такое EntityManager ? [X]

`EntityManager` — это основной интерфейс для взаимодействия с базой данных в JPA (Java Persistence API). Он выполняет аналогичную роль, что и `Session` в Hibernate, но в контексте стандарта JPA.

- `persist(Object entity)`: Сохраняет новую сущность в базу данных.
- `find(Class<T> entityClass, Object primaryKey)`: Находит сущность по первичному ключу.
- `remove(Object entity)`: Удаляет сущность из базы данных.
- `merge(Object entity)`: Обновляет (сливает) состояние сущности с базой данных.
- `createQuery(String qlString)`: Создает запрос на основе JPQL или SQL.
- `getTransaction()`: Возвращает транзакцию для явного управления.

#### Чем отличаются NamedEntityGraph от EntityGraph ? [X]

- `NamedEntityGraph` позволяет заранее определить граф сущностей и его атрибуты, который затем можно использовать в разных частях приложения. Этот граф может быть заранее именован, а затем ссылаться на него через аннотацию `@EntityGraph` в методах репозитория.

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

- Здесь используется `EntityGraph` с атрибутом `attributePaths`, чтобы указать, что нужно загрузить `characteristics` при выполнении запроса.

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

#### JPA CascadeType.REMOVE vs orphanRemoval [X]

- As stated earlier, marking a reference field with _CascadeType.REMOVE_ is a way to delete a child entity or entities whenever the deletion of its parent happens.
- `orphanRemoval` — это свойство, которое указывает, что дочерняя сущность должна быть удалена, если она больше не связана с родительской сущностью. То есть, если объект был отсоединен от родителя, он автоматически будет удален в базе.
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

```java
Session session = sessionFactory.openSession();
try {
    session.beginTransaction();
    
    // Блокировка PESSIMISTIC_READ (SELECT ... FOR SHARE в PostgreSQL)
    User user = session.get(User.class, 1L, LockMode.PESSIMISTIC_READ);
    
    // Работа с данными...
    session.getTransaction().commit();
} finally {
    session.close();
}

// Вариант 2: Использование JpaRepository + @Lock
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    @Lock(LockModeType.PESSIMISTIC_READ)
    @Query("SELECT u FROM User u WHERE u.id = :id")
    User findByIdWithPessimisticRead(@Param("id") Long id);

    @Lock(LockModeType.PESSIMISTIC_WRITE)
    Optional<User> findById(Long id); // Автоматически применит блокировку
}
```

###### Какие есть дополнительные параметры у @OneToMany @ManyToOne и что они делают?


###### Зачем нужен fetchType? Какие по умолчанию для каждого типа?

###### Есть 2 метода 1- на select данных в бд, 1 - update данных в таблице, они разъедены во времени. Может быть несколько пользователей, которые работают с этими методами асинхронно. Что можно предложить, чтобы сохранить консистентность данных?



######  Почему способ хранения Single Table не лучший для хранения данных

###### отличия btree bндекса от бинарного и для чего они нужны

#### Что такое type в EntityGraph ?

Параметр **`type`** в `@EntityGraph` управляет тем, как Hibernate загружает поля, не указанные в `attributePaths`:

- `LOAD` = оставляет их как есть (по умолчанию в сущности).
- `FETCH` = загружает лениво, даже если они `EAGER`.

#### Сколько потоков Hiraki?

`spring.datasource.hikari.maximum-pool-size=10` 
`spring.datasource.hikari.minimum-idle=10` 
#### Настройки hikari

```java
spring.datasource.hikari.connection-timeout=50000 
spring.datasource.hikari.idle-timeout=300000 
spring.datasource.hikari.max-lifetime=900000 
spring.datasource.hikari.maximum-pool-size=10 
spring.datasource.hikari.minimum-idle=10 
spring.datasource.hikari.pool-name=ConnPool 

spring.datasource.hikari.connection-test-query=select 1 from dual spring.datasource.hikari.data-source-properties.cachePrepStmts=true spring.datasource.hikari.data-source-properties.prepStmtCacheSize=250 spring.datasource.hikari.data-source-properties.prepStmtCacheSqlLimit=2048 spring.datasource.hikari.data-source-properties.useServerPrepStmts=true spring.datasource.hikari.data-source-properties.useLocalSessionState=true spring.datasource.hikari.data-source-properties.rewriteBatchedStatements=true spring.datasource.hikari.data-source-properties.cacheResultSetMetadata=true spring.datasource.hikari.data-source-properties.cacheServerConfiguration=true spring.datasource.hikari.data-source-properties.elideSetAutoCommits=true spring.datasource.hikari.data-source-properties.maintainTimeStats=false
```


#### Embeddable vs embedded

- По умолчанию Hibernate добавляет префикс `имяПоля_` (т.е. `address_`).
- Можно изменить это поведение через `@AttributeOverride`

```java
@Entity
public class User {
    @Id
    private Long id;
    
    @Embedded  // Указывает, что это встроенный объект
    private Address address;  // Address — @Embeddable
}

@Embeddable  // Объявляет класс встраиваемым
@AttributeOverrides({
    @AttributeOverride(name = "city", column = @Column(name = "home_city")),
    @AttributeOverride(name = "street", column = @Column(name = "home_street"))
})
public class Address {
    private String city;
    private String street;
}

in sql

CREATE TABLE User (
    id BIGINT PRIMARY KEY,
    home_city VARCHAR(255),  // Переименовано
    home_street VARCHAR(255) // Переименовано
);
```



#### Как можно джоинить таблицы в Hibernate?

- Используя `@JoinColumn` для указания внешнего ключа.
- Используя `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`.
- Через HQL или Criteria API.
- EntityGraph

#### Коллекция: `List` vs `Set` в JPA (OneToMany)

- **`Set`:**
    - Гарантирует уникальность (через `equals()`/`hashCode()`).
    - Проблемы: Если `hashCode()` зависит от полей сущности — поведение Hibernate нарушится.
- **`List`:**
    - Допускает дубликаты.
    - Под капотом: Hibernate использует `PersistentBag`.
    - Коллизии: При удалении элемента из середины — обновляет все индексы (неэффективно для больших коллекций).
- Батчи: Для `List` с `@OrderColumn` батчи могут работать некорректно. Рекомендуется `Set` или `@OrderBy`.

#### Как реализовать оптимистичную блокировку в Hibernate?

- Добавить поле с аннотацией `@Version`. Hibernate автоматически проверяет это поле при обновлении, чтобы убедиться, что данные не были изменены другой транзакцией.

#### Как реализовать пессимистичную блокировку в Hibernate?

- Использовать `LockModeType.PESSIMISTIC_READ` или `LockModeType.PESSIMISTIC_WRITE` в запросах.
- Использовать `SELECT FOR UPDATE` через HQL или Criteria API.

### Resources

- [Аннотации Java для работы с базой данных](https://javastudy.ru/spring-data-jpa/annotation-persistence/)
- [Entity auditing with Hibernate Envers](https://golb.hplar.ch/2019/08/envers.html)
- 