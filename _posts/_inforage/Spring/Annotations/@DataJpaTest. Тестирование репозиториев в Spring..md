`@DataJpaTest` — это аннотация в Spring Boot, предназначенная для тестирования компонентов Spring Data JPA. Она предоставляет настроенный контекст для работы с JPA, автоматически конфигурирует `EntityManager`, настраивает встроенную базу данных и выполняет тесты в изолированном окружении.

Тестирование репозиториев с `@DataJpaTest` помогает гарантировать, что ваш слой данных работает правильно, не влияя на реальную базу данных и другие компоненты приложения.

#### Основные особенности `@DataJpaTest`

1. **Изолированная среда**: Создает отдельную среду для тестирования, позволяя тестировать репозитории без влияния на реальную базу данных.
2. **Автоматическая настройка**: Автоматически настраивает `EntityManager`, репозитории и другие компоненты Spring Data JPA.
3. **Использование встроенной базы данных**: По умолчанию использует H2, что позволяет легко создавать и управлять тестовыми данными.

### Пример использования @DataJpaTest

#### 1. Написание теста с использованием @DataJpaTest

```java
@DataJpaTest
public class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testSaveAndFindUser() {
        // Создание и сохранение нового пользователя
        User user = new User();
        user.setFirstName("John");
        user.setLastName("Doe");
        user = userRepository.save(user);

        // Проверка, что пользователь был сохранен
        assertThat(user).isNotNull();
        assertThat(user.getId()).isNotNull();

        // Поиск пользователя по имени
        User foundUser = userRepository.findByFirstName("John");

        // Проверка, что найденный пользователь соответствует ожиданиям
        assertThat(foundUser).isNotNull();
        assertThat(foundUser.getLastName()).isEqualTo("Doe");
    }
}
```

Объяснение:

- `@DataJpaTest` настраивает тестовую среду для работы с JPA, включая встроенную базу данных H2.
- `@Autowired` внедряет репозиторий, который мы тестируем.
- В тесте создается новый пользователь, сохраняется в базу данных и затем проверяется его наличие и корректность извлечения.

#### 2. Использование In-Memory базы данных

По умолчанию `@DataJpaTest` использует H2 в памяти, но вы также можете настроить другие базы данных для тестирования. Встроенные базы данных подходят для большинства тестов, но для более сложных сценариев вы можете использовать, например, PostgreSQL или MySQL в тестовой среде.

Настройка базы данных:

```java
# src/test/resources/application.properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
```

#### 3. Тестирование с настройками

Для тестирования с разными настройками можно использовать аннотации `@AutoConfigureTestDatabase` и `@TestConfiguration`.

##### Пример с @AutoConfigureTestDatabase:

```java
@DataJpaTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
public class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testSaveAndFindUser() {
        // Создание и сохранение нового пользователя
        User user = new User();
        user.setFirstName("John");
        user.setLastName("Doe");
        user = userRepository.save(user);

        // Проверка, что пользователь был сохранен
        assertThat(user).isNotNull();
        assertThat(user.getId()).isNotNull();

        // Поиск пользователя по имени
        User foundUser = userRepository.findByFirstName("John");

        // Проверка, что найденный пользователь соответствует ожиданиям
        assertThat(foundUser).isNotNull();
        assertThat(foundUser.getLastName()).isEqualTo("Doe");
    }
}
```

`@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)` отключает замену базы данных по умолчанию, что позволяет использовать реальную базу данных, настроенную в тестовом окружении.

#### 4. Использования профиля для тестов

```java
@DataJpaTest
@ActiveProfiles("test")  // Указывает Spring использовать профиль "test"
public class UserRepositoryTest {
...
```

#### 5. Использование `@TestConfiguration` для создания тестового конфига

```java
@TestConfiguration
public class TestDatabaseConfig {

    @Bean
    public DataSource dataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setUrl("jdbc:postgresql://localhost:5432/testdb");
        dataSource.setUsername("testuser");
        dataSource.setPassword("testpass");
        dataSource.setDriverClassName("org.postgresql.Driver");
        return dataSource;
    }
}
```

```java
@DataJpaTest
@Import(TestDatabaseConfig.class)
public class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testSaveAndFindUser() {
	...
    }
}
```





