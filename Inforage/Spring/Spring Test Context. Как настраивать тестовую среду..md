### Основные компоненты Spring Test Context

1. **`@SpringBootTest`**: Используется для запуска тестов с полным контекстом Spring Boot приложения. Эта аннотация поднимает весь Spring контекст, что полезно для интеграционных тестов.
    
2. **`@ContextConfiguration`**: Определяет, как конфигурируется контекст Spring для тестов. Используется для тестов, которые требуют кастомной конфигурации Spring.
    
3. **`@WebAppConfiguration`**: Обеспечивает поддержку веб-контекста при тестировании веб-приложений. Используется вместе с `@SpringBootTest` для тестирования веб-слоя.
    
4. **`@MockBean`**: Позволяет подменять бины в контексте Spring для тестирования. Используется для создания мок-объектов, которые заменяют настоящие бины.
    
5. **`@Autowired`**: Используется для внедрения зависимостей в тестовые классы.
    
6. **`@TestConfiguration`**: Позволяет создавать специфическую конфигурацию для тестов. Используется внутри тестовых классов для создания бинов только для тестирования.
    
7. **`@Transactional`**: Обеспечивает, что тесты выполняются в рамках транзакции, которая будет откатана после выполнения теста.

### Примеры настройки тестовой среды

#### Пример 1: Полный контекст Spring Boot

```java
@SpringBootTest
public class UserServiceIntegrationTest {

    @Autowired
    private UserService userService;

    @Test
    public void testUserCreation() {
        User user = new User("John", "Doe");
        userService.createUser(user);
        User retrievedUser = userService.getUserById(user.getId());
        assertNotNull(retrievedUser);
        assertEquals("John", retrievedUser.getFirstName());
    }
}
```

**Объяснение**:

- `@SpringBootTest` загружает весь контекст Spring Boot приложения.
- `@Autowired` внедряет сервис, который тестируется.
- Тест проверяет создание и получение пользователя.

#### Пример 2: Специфическая конфигурация для тестов

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes = {TestConfig.class})
public class CustomConfigTest {

    @Autowired
    private SomeService someService;

    @Test
    public void testService() {
        assertNotNull(someService);
        // Проверьте логику сервиса
    }

    @TestConfiguration
    static class TestConfig {
        @Bean
        public SomeService someService() {
            return new SomeServiceImpl();
        }
    }
}
```

**Объяснение**:

- `@ContextConfiguration` указывает, что конфигурация для теста предоставляется в классе `TestConfig`.
- `@TestConfiguration` используется для создания бинов, специфичных для тестов.

#### Пример 3: Использование MockBean

```java
@SpringBootTest
public class UserServiceTest {

    @Autowired
    private UserService userService;

    @MockBean
    private UserRepository userRepository;

    @Test
    public void testUserService() {
        User user = new User("Jane", "Doe");
        Mockito.when(userRepository.findById(1L)).thenReturn(Optional.of(user));

        User retrievedUser = userService.getUserById(1L);
        assertEquals("Jane", retrievedUser.getFirstName());
    }
}
```

**Объяснение**:

- `@MockBean` заменяет реальный `UserRepository` на мок-объект.
- Тест проверяет логику `UserService` без необходимости взаимодействовать с настоящим репозиторием.

### Как настроить тестовую среду

1. **Создание тестового конфигурационного файла**: В тестах можно использовать специальные конфигурации или изменить существующие для выполнения тестов.
2. **Использование моков и стаба**: Моки и стабы помогают изолировать тестируемый компонент от его зависимостей, делая тесты более быстрыми и надежными.
3. **Использование аннотаций Spring Test Context**: Используйте аннотации, такие как `@SpringBootTest`, `@ContextConfiguration`, и `@MockBean`, чтобы настроить тестовую среду в соответствии с вашими потребностями.
4. **Настройка транзакций**: Используйте `@Transactional` для управления транзакциями в тестах и автоматического отката изменений после выполнения тестов.

