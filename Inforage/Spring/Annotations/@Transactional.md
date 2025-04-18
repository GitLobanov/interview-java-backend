### Основы `@Transactional`

Аннотация `@Transactional` используется для указания, что методы или классы должны быть выполнены в рамках транзакции. Это позволяет гарантировать, что все операции с базой данных в рамках одной транзакции будут выполнены атомарно — либо все операции будут успешно завершены, либо, в случае ошибки, все изменения будут откатаны.

```java
public class UserService {

    @Transactional
    public Long registerUser(User user) {
        // выполнить некоторый SQL, который, например,
        // вставляет пользователя в базу данных 
        // и извлекает автогенерированный id
        // userDao.save(user);
        return id;
    }
}
```

- Убедитесь, что ваша `Configuration` **_Spring_** сопровождена аннотацией `@EnableTransactionManagement` **Spring boot** _это будет сделано автоматически_).
- Убедитесь, что вы указали менеджер транзакций в вашей `Configuration` **_Spring_** (это необходимо сделать в любом случае).

Метод `registerUser()` действительно просто вызывает `userDao.save(user)`, и нет способа изменить это на лету.

Но у Spring есть преимущество. По своей сути это IoC container (_контейнер инверсии контроля_). Он создаёт экземпляр `UserService` и обеспечивает автоподключение этого `UserService` к любому другому бины, которому нужен этот `UserService`.

Теперь, когда вы используете `@Transactional` над бинами, **Spring** использует маленькую хитрость. Он не просто инстанцирует `UserService`, но и транзакционный _прокси_ этого `UserService`.

Он делает это с помощью метода под названием `proxy-through-subclassing` с помощью [библиотеки Cglib](https://github.com/cglib/cglib). Существуют и другие способы построения прокси (например, [Dynamic JDK proxies](https://docs.oracle.com/javase/8/docs/technotes/guides/reflection/proxy.html)), но пока оставим это на потом.

![[Pasted image 20240921164545.png]]

Как видно из этой диаграммы, у конкретного прокси есть одна задача.

- Открытие и закрытие соединений/транзакций с базой данных.
- А затем делегирование настоящему `UserService`, тому, который вы написали.
- А другие бины, такие как ваш `UserRestController`, никогда не узнают, что они разговаривают с прокси, а не с настоящим.

#### Принципы работы. ACID

1. **Атомарность**: Гарантирует, что транзакция будет завершена успешно или откатится полностью в случае сбоя. Если одна из операций не удалась, все изменения, сделанные в рамках транзакции, будут отменены.
2. **Согласованность**: Гарантирует, что транзакция приведет базу данных в согласованное состояние. Если транзакция была успешной, все изменения должны быть сохранены. К примеру только положительное, не ссылаться на не существующие ключи
3. **Изолированность**: Гарантирует, что изменения, сделанные в рамках одной транзакции, не будут видны другим транзакциям до тех пор, пока текущая транзакция не будет завершена.
4. **Долговечность**: Гарантирует, что после завершения транзакции все изменения будут сохранены и не будут потеряны, даже в случае сбоя системы.
    
### Как использовать `@Transactional`

Аннотацию `@Transactional` можно применять как к методам, так и к классам. При применении к классу, она влияет на все методы класса. Применение к методу означает, что только этот метод будет выполняться в рамках транзакции.

### Конфигурация `@Transactional`

По умолчанию Spring использует JDBC транзакции через `DataSourceTransactionManager`. В зависимости от используемой базы данных или JPA-провайдера, Spring может использовать разные менеджеры транзакций.

```java
@Configuration
@EnableTransactionManagement
public class AppConfig {

    @Bean
    public PlatformTransactionManager transactionManager(EntityManagerFactory entityManagerFactory) {
        return new JpaTransactionManager(entityManagerFactory);
    }
}
```

### Атрибуты аннотации `@Transactional`

1. **propagation**: Определяет, как транзакции должны быть объединены или создавать новые транзакции. Возможные значения:
    
    - `REQUIRED` (по умолчанию): Использует существующую транзакцию или создает новую.
    - `REQUIRES_NEW`: Всегда создает новую транзакцию.
    - `MANDATORY`: Требует существующей транзакции; вызывает исключение, если нет.
    - `SUPPORTS`: Использует существующую транзакцию, если она есть, иначе работает без транзакции.
    - `NOT_SUPPORTED`: Отключает транзакции и работает вне транзакции.
    - `NEVER`: Не поддерживает транзакции и вызывает исключение, если транзакция уже существует.
2. **isolation**: Определяет уровень изоляции транзакций. Возможные значения:
    
    - `DEFAULT`: Использует уровень изоляции по умолчанию базы данных.
    - `READ_UNCOMMITTED`: Позволяет чтение неподтвержденных данных.
    - `READ_COMMITTED`: Позволяет чтение только подтвержденных данных.
    - `REPEATABLE_READ`: Гарантирует повторяемость чтений в рамках транзакции.
    - `SERIALIZABLE`: Наивысший уровень изоляции, предотвращает любые конкурентные изменения.
3. **timeout**: Определяет время ожидания транзакции. Если транзакция не завершается за указанное время, она будет принудительно прервана.
    
4. **readOnly**: Определяет, является ли транзакция только для чтения. Это может оптимизировать производительность, так как базы данных могут оптимизировать операции только для чтения.
    
5. **rollbackFor**: Указывает, какие исключения должны вызывать откат транзакции.
    
6. **noRollbackFor**: Указывает, какие исключения не должны вызывать откат транзакции.


```java
@Transactional(propagation = Propagation.REQUIRES_NEW, isolation = Isolation.SERIALIZABLE, timeout = 30, readOnly = false)
public void updateUser(User user) {
    userRepository.save(user);
    // Другие операции с базой данных
}
```

### Исключения и обработка

В случае возникновения исключений, если они не указаны в атрибуте `rollbackFor`, транзакция может не откатиться. Это важно учитывать при настройке транзакционного поведения.
## Внешние и внутренние вызовы методов

Ещё один интересующий момент — почему аннотация `@Transactional` работает только для внешних вызовов методов и не работает для внутренних (самовызовов) методов.

Это связано с тем, как работает механизм прокси. Прокси класс перехватывает только внешние вызовы методов, приходящие через прокси. Если метод объекта вызывает другой метод этого же объекта напрямую (т.е. самовызов), то этот вызов обходит прокси, и, следовательно, не перехватывается им.

Иными словами, если внутри транзакционного метода вызывается другой транзакционный метод того же объекта, то этот внутренний вызов не будет обернут в новую транзакцию, а будет выполняться в рамках уже существующей транзакции.

### TransactionTemplate / TransactionOperations

```java
@Service
public class UserService {

    @Autowired
    private TransactionTemplate template;

    public Long registerUser(User user) {
        Long id = template.execute(status ->  {
            // выполнить некоторый SQL, который, например,
            // вставляет пользователя в базу данных 
            // и возвращает автогенерированный идентификатор
          return id;
        });
    }
}
```
### PlatformTransactionManager

Ваш `UserService` проксируется на лету, и прокси управляет транзакциями для вас. Но не сам прокси управляет всем этим транзакционным состоянием (открыть, зафиксировать, закрыть), прокси делегирует эту работу менеджеру транзакций.

**Spring** предлагает вам интерфейс `PlatformTransactionManager` / `TransactionManager`, который по умолчанию поставляется с парой удобных реализаций. Одна из них - менеджер транзакций источника данных.

Он делает то же самое, что вы делали до сих пор для управления транзакциями, но сначала давайте рассмотрим необходимую конфигурацию **Spring**:

```java
@Bean
public DataSource dataSource() {
    return new MysqlDataSource(); // (1)
}

@Bean
public PlatformTransactionManager txManager() {
    return new DataSourceTransactionManager(dataSource()); // (2)
}
```

- Здесь вы создаёте источник данных, специфичный для базы данных или для пула соединений. В данном примере используется **MySQL**.
- Здесь вы создаёте свой менеджер транзакций, которому нужен источник данных, чтобы иметь возможность управлять транзакциями.

Все менеджеры транзакций имеют методы типа "`doBegin`" (для запуска транзакции) или "`doCommit`", которые выглядят следующим образом - взяты прямо из исходного кода **Spring** с некоторым упрощением:

```java
public class DataSourceTransactionManager implements PlatformTransactionManager {

    @Override
    protected void doBegin(Object transaction, TransactionDefinition definition) {
        Connection newCon = obtainDataSource().getConnection();
        // ...
        con.setAutoCommit(false);
    }

    @Override
    protected void doCommit(DefaultTransactionStatus status) {
        // ...
        Connection connection = status.getTransaction().getConnectionHolder().getConnection();
        try {
            con.commit();
        } catch (SQLException ex) {
            throw new TransactionSystemException("Could not commit JDBC transaction", ex);
        }
    }
}
```

- Если **Spring** обнаруживает аннотацию `@Transactional` на бины, он создаёт динамический прокси этого бины.
- Прокси имеет доступ к менеджеру транзакций и будет просить его открывать и закрывать транзакции/соединения.
- Сам менеджер транзакций будет просто делать то, что вы делали в разделе "[Простой JDBC](https://habr.com/ru/articles/682362/#%D0%BF%D1%80%D0%BE%D1%81%D1%82%D0%BE%D0%B9_%D0%BF%D1%80%D0%B8%D0%BC%D0%B5%D1%80_JDBC)": Управлять старым добрым соединением **JDBC**.

### В чём разница между физическими и логическими транзакциями?

```java
@Service
public class UserService {

    @Autowired
    private InvoiceService invoiceService;

    @Transactional
    public void invoice() {
        invoiceService.createPdf();
        // отправка счёта по электронной почте и т.д...
    }
}

@Service
public class InvoiceService {

    @Transactional
    public void createPdf() {
        // ...
    }
}
```

`UserService` имеет транзакционный метод `invoice()`. Который вызывает другой транзакционный метод, `createPdf()` на `InvoiceService`.

Теперь, с точки зрения транзакций базы данных, это должна быть всего лишь одна транзакция базы данных. (Помните: `getConnection().setAutocommit(false).commit())`. **Spring** называет это _физической_ транзакцией, хотя сначала это может показаться немного запутанным.

Однако со стороны **Spring** происходит две _логические транзакции_: Первая - в `UserService`, вторая - в `InvoiceService`. **Spring** должен быть достаточно умным, чтобы знать, что оба метода `@Transacional` должны использовать _в основе_ одну и ту же _физическую_ транзакцию базы данных.

```java
@Service
public class InvoiceService {

    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void createPdf() {
        // ...
    }
}
```

Изменение режима распространения (propagation) на `requires_new` говорит **Spring**, что `createPDF()` должна выполняться в собственной транзакции, независимо от любой другой, уже существующей транзакции.

Это означает, что ваш код будет открывать **два** (физических) соединения/транзакции с базой данных. (Снова: `getConnection()` _x2_`. setAutocommit(false)` _x2_`. commit()` _x2_). **Spring** теперь должен быть достаточно умным, чтобы две логические транзакции (`invoice()`/`createPdf()`) теперь также отображались на две разные физические транзакции базы данных.

- Физические транзакции: это ваши фактические транзакции **JDBC**.
- Логические транзакции: это (потенциально вложенные) аннотированные `@Transactional` методы **Spring**.

### Взаимодействие с Hibernate 

```java
@Service
public class UserService {

    @Autowired
    private SessionFactory sessionFactory; // (1)

    @Transactional
    public void registerUser(User user) {
        sessionFactory.getCurrentSession().save(user); // (2)
    }

}
```

Вместо использования [DataSourcePlatformTransactionManager](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/jdbc/datasource/DataSourceTransactionManager.html) в конфигурации **Spring**, вы будете использовать [HibernateTransactionManager](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/orm/hibernate5/HibernateTransactionManager.html) (при использовании обычного Hibernate) или [JpaTransactionManager](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/orm/jpa/JpaTransactionManager.html) (при использовании **Hibernate** через **JPA**).

Специализированный `HibernateTransactionManager` обеспечит:

- Управлять транзакциями через **Hibernate**, т.е. через `SessionFactory`.
- Быть достаточно умным, чтобы позволить **Spring** использовать ту же самую транзакцию в коде **Spring**, не относящуюся к **Hibernate**, т.е. `@Transactional`.
### Источники

- [Управление транзакциями в Spring: @Transactional в деталях](https://habr.com/ru/articles/682362/)
- 



