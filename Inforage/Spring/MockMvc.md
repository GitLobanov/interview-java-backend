`MockMvc` — это инструмент в Spring Framework, предназначенный для тестирования контроллеров веб-приложений. Он позволяет выполнять запросы к вашему приложению и проверять ответы, не поднимая полноценный веб-сервер. Это делает тестирование более быстрым и изолированным.

#### Основные возможности MockMvc

1. **Изолированное тестирование**: Позволяет тестировать контроллеры без необходимости запускать сервер.
2. **Поддержка всех типов HTTP-запросов**: GET, POST, PUT, DELETE и другие.
3. **Проверка результатов**: Позволяет проверять статус коды, заголовки и тело ответа.

### Настройка MockMvc

Для использования `MockMvc` необходимо сначала добавить зависимость в ваш проект. Если вы используете Maven, добавьте следующий фрагмент в ваш `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

### Пример использования MockMvc

#### 1. Основной пример

В этом примере мы будем тестировать контроллер, который управляет объектами `User`.

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.createUser(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
}
```

Тест с использованием MockMvc:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(UserController.class)
public class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    public void testCreateUser() throws Exception {
        User user = new User("John", "Doe");
        Mockito.when(userService.createUser(Mockito.any(User.class))).thenReturn(user);

        mockMvc.perform(post("/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"firstName\":\"John\",\"lastName\":\"Doe\"}"))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.firstName").value("John"))
                .andExpect(jsonPath("$.lastName").value("Doe"));
    }

    @Test
    public void testGetUser() throws Exception {
        User user = new User("Jane", "Doe");
        Mockito.when(userService.getUserById(1L)).thenReturn(user);

        mockMvc.perform(get("/users/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.firstName").value("Jane"))
                .andExpect(jsonPath("$.lastName").value("Doe"));
    }
}
```

**Объяснение:**

- `@RunWith(SpringRunner.class)` и `@WebMvcTest(UserController.class)` загружают контекст Spring для тестирования контроллеров.
- `@Autowired` внедряет `MockMvc`, который используется для выполнения запросов.
- `@MockBean` создает мок-объекты для `UserService`, которые заменяют настоящие зависимости.
- `mockMvc.perform(...)` выполняет запрос и проверяет ответ с помощью методов `andExpect`.

