При работе с ORM существует 2 стратегии инициализации данных: eager (жадная) и lazy (ленивая).

- Eager Loading — это стратегия, в которой инициализация данных происходит сразу.
- Lazy Loading — это стратегия, в которой мы откладываем инициализацию данных.

## @onetomany, @ManyToOne, @ManyToMany

Стратегия инициализации указывается при установке связи между классами-entity при помощи аргумента `fetch` в аннотациях OneToMany, ManyToOne и ManyToMany.

Например:

```java
@ManyToOne(optional = false, fetch = FetchType.LAZY)
private Course course;
```

Если явно не задать значение `fetch`, то будут использоваться следующие значения по умолчанию:

- Для `OneToMany` - LAZY
- Для `ManyToOne` - EAGER
- Для `ManyToMany` - LAZY

Lazy-loading работает за счёт создания прокси-объектов, которые загружают данные из БД при обращении к объекту. Для загрузки данных из БД Proxy-объекты используют сессию Hibernate, соответственно они могут работать только при наличии такой сессии. Если же такой сессии нет, то будет брошено исключение `LazyInitializationException`.

## LazyInitializationException

Данное исключение бросает Hibernate, когда ему необходимо инициализировать связанную сущность (например, коллекцию дочерних объектов) для которой указана "ленивая" загрузка, но при этом отсутствует контекст сессии Hibernate (Hibernate session context).

Сессия Hibernate создаётся, например, благодаря аннотации `@Transactional`. Или если мы в явном виде начнём транзакцию:

```java
entityManager = entityManagerFactory().createEntityManager();
transaction = entityManager.getTransaction().begin();
```

Существует несколько способов избавиться от `LazyInitializationException`, но не все они хороши.

### Антипаттерн Open Session In View

Суть данного антипаттерна в том, что сессия Hibernate открывается и закрывается не в сервисном слое, а в слое "выше" - на уровне представления (view layer), к примеру в контроллере. Контроллер вызывает сервисный слой, который выполняет различные действия с данными и возвращает управление контроллеру. А поскольку сессия всё ещё открыта, слой представления может инициализировать объекты, для которых указана "ленивая" загрузка. Однако если сервисный слой выполнит `commit`, то транзакция завершится. Из-за этого Hibernate будет выполнять все SQL-запросы, инициированные на слое представления, в режиме `auto-commit`, то есть каждый такой SQL-запрос будет выполняться в отдельной транзакции, что приведёт к повышению нагрузки на БД, т.к. в конце каждой транзакции БД должна сделать запись в журнале транзакций на диске, что является ресурсозатратной операцией.

Помимо повышения нагрузки на БД у Open Session In View есть и другой недостаток. Поскольку операции выполняются не в одной транзакции, а в двух и более, можно получить неконсистентное состояние данных.

Обратите внимание: Spring Boot по умолчанию использует Open Session in View, о чём свидетельствует сообщение в логах при старте:

```java
2022-04-13 23:01:34.863  WARN 109326 --- [main] JpaBaseConfiguration$JpaWebConfiguration : 
    spring.jpa.open-in-view is enabled by default. Therefore, database queries may be performed during view rendering. 
    Explicitly configure spring.jpa.open-in-view to disable this warning
```

Отключить Open Session In View можно установив `false` параметру `spring.jpa.open-in-view` в `application.properties`.

### Параметр hibernate.enable_lazy_load_no_trans

Ещё один способ избавиться от исключения `LazyInitializationException`, которым не стоит пользоваться - это установление параметру `spring.jpa.properties.hibernate.enable_lazy_load_no_trans` значения `true` в файле `application.properties`.

Этот параметр говорит Hibernate открывать временную сессию, когда нет активной сессии, в рамках которой можно инициализировать объект с ленивым типом загрузки. Это увеличивает количество используемых подключений к базе данных, количество транзакций и общую нагрузку на БД.

### Join fetch

Один из корректных вариантов инициализации данных с ленивым типом загрузки - использовать JOIN FETCH в аннотации `@Query`:

```java
@Repository
public interface CourseRepository extends JpaRepository<Course, Long> {
    @Query(value = "" +
            "SELECT c FROM Course c " +
            "  JOIN FETCH c.lessons")
    List<Course> findAll();
}
```

В этом случае Hibernate сгенерирует следующий SQL-запрос:

```java
select distinct course0_.id         as id1_0_0_,
                lessons1_.id        as id1_1_1_,
                course0_.author     as author2_0_0_,
                course0_.title      as title3_0_0_,
                lessons1_.course_id as course_i4_1_1_,
                lessons1_.text      as text2_1_1_,
                lessons1_.title     as title3_1_1_,
                lessons1_.course_id as course_i4_1_0__,
                lessons1_.id        as id1_1_0__
from courses course0_
         inner join lessons lessons1_ on course0_.id = lessons1_.course_id
```

### @EntityGraph

Ещё один корректный способ инициализации данных с ленивым типом - использовать аннотацию `@EntityGraph` над методом в репозитории:

```java
@EntityGraph(attributePaths = {"lessons"})
@Query("SELECT c FROM Course c")
List<Course> findAllUsingEntityGraph();
```

В аргументе `attributePaths` аннотации `@EntityGraph` мы указали тот объект, который необходимо инициализировать сразу, несмотря на установленный у него ленивый тип загрузки.

При необходимости в классе-сущности можно описать один или несколько EntityGraph'ов:

```java
@Entity
@Table(name = "courses")
@NamedEntityGraph(name = "joinLessons", attributeNodes = {@NamedAttributeNode("lessons")})
@NamedEntityGraph(name = "noJoins")
public class Course {
    ...
}
```

Теперь в репозитории можно обращаться к ним по имени:

```java
@EntityGraph(value = "joinLessons")
@Query("SELECT c FROM Course c")
List<Course> findAllWithLessons();

@EntityGraph(value = "noJoins")
@Query("SELECT c FROM Course c")
List<Course> findAll();
```