Мы уже обсуждали, что один из важных критериев хорошего unit теста - это то, что он тестирует только небольшой кусок функционала, а не все сразу. Теперь давайте попробуем протестировать `UserService` из предыдущего урока. Какие у нас возникнут проблемы? Сервис использует внешние зависимости (`UserRepository` и `PasswordEncoder`) и надо как-то с этим быть в тестах.

Можно перед выполнением теста создать все зависимости и просто передать их в класс, но тут есть несколько проблем. Во-первых, не всегда это можно сделать. Зависимость может работать с внешним сервисом, доступа до которого у вас нет с локальной машины. Или использовать системные api, которые недоступны. Во-вторых, если пойти этим путем, то что вы будете тестировать? Мы хотим протестировать, как работает наш `UserService`, но если использовать реальные зависимости, то получается, что мы будем тестировать еще и как работают `UserRepository` или `PasswordEncoder`. Это увеличивает сложность самого теста и поиска ошибок впоследствии.

Так как же быть? Одним из наиболее популярных решений являются моки. Это имитация публичного интерфейса класса.

Если класс и его публичный интерфейс соответствует каким-то ожиданиям функции, то ничто не мешает вам сделать класс, который снаружи имеет такой же набор функций, но в то же время будет вместо реального выполнения кода отдавать заранее заданное значение. К примеру, вместо вычисления хеша пароля в тесте будем отдавать всегда определенное значение.

Моки также обладают уникальным свойством — запоминают вызовы методов класса. Иногда бывает интересно, а тот код, который использует этот объект, он вообще вызывает его методы? Может, программист случайно забыл воспользоваться этим объектом, или забыл убрать заглушку, которая возвращающая одно и то же значение и т. д. И случайно это значение совпало с тем, что мы ожидали, и тест вроде как рабочий, но на деле у нас есть баг. Звучит как безумная ситуация, но на практике встречается часто. В этом случае хорошо бы проверять, что вызовы методов этого класса вообще были сделаны.

Как моки создавать? Можно делать всю эту работу руками, но более удобное решение — специализированные фреймворки для создания заглушек. Мы будем рассматривать Mockito.

Mockito позволяет создать одной строчкой кода mock любого класса. Для такого mock сразу после создания характерно некое поведение по умолчанию, все методы возвращают заранее известные значения — обычно это null либо 0. Можно переопределить это поведение желаемым образом, проконтролировать с нужной степенью детальности обращения к ним и так далее. В результате mock становится заглушкой с требуемыми свойствами.

Давайте посмотрим, как это работает на примере с `UserService`.

```java
private UserService service;

@BeforeAll
public void setUp() {
    UserRepository userRepositoryMock = Mockito.mock(UserRepository.class);
    PasswordEncoder passwordEncoderMock = Mockito.mock(PasswordEncoder.class);

    Mockito.when(userRepositoryMock.findAll()).thenReturn(Collections.emptyList());
    Mockito.when(passwordEncoderMock.encode(anyString())).thenReturn("md5hash");

    service = new UserService(userRepositoryMock, passwordEncoderMock);
}
```

Тут мы сначала создаем моки, используя `Mockito.mock()`, дальше настраиваем их поведение и после передаем их в конструктор `UserService`. С помощью `Mockito.when()` мы переопределили метод `findAll` у `UserRepository` и теперь он будет возвращать пустой список. Сюда можно передать любой объект, подходящий по типу, например фиксированный список пользователей. Остальные методы класса, к примеру `findById`, будут возвращать `null`, так как их поведение мы не трогали.

У `PasswordEncoder` метод `encode` есть один строковый аргумент, поэтому надо указать Mockito, что с ним делать. Самый простой способ, это просто указать, что для любых значений надо возвращать заданное значение. Можно также указать точные значения, с которыми работать. Выглядеть это будет так:

```csharp
Mockito.when(passwordEncoderMock.encode("superPass")).thenReturn("&$^%^");
Mockito.when(passwordEncoderMock.encode("weak1234")).thenReturn("meh");
```

В таком случае Mockito проверит переопределено ли поведение для заданного набора аргументов и вернет заготовленный результат или дефолтное значение. Для данного примера это будет работать следующим образом:

`passwordEncoderMock.encode("superPass") -> "&$^%^"`

`passwordEncoderMock.encode("some new password") -> ""`

Теперь можно писать тесты для `UserService` и не думать о том, как работают `UserRepository` и `PasswordEncoder`.

Как проверить, что метод у объекта был вызван? Давайте сразу посмотрим на код:

```typescript
@Test
public void passwordAlwaysShouldBeEncoded() {
    service.save(new UserDto());
    Mockito.verify(passwordEncoderMock, Mockito.times(1)).encode(anyString());
}
```

`Mockito.verify` как раз отвечает за эту проверку. В качестве аргументов передаем интересующий нас mock, количество вызовов и какой метод мы хотим проверить.

В `spring-boot-starter-test` уже подключен Mockito, так что не надо ничего дополнительно настраивать, можно сразу писать тесты с использованием mock'ов. [Ссылка на документацию](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html)

### Тестирование MVC

Описанный выше подход хорошо подходит для тестирования сервисных классов, но у нас еще есть web-слой, который тоже хочется протестировать. Для этого в Spring есть два подхода.

Первый - поднять приложение целиком и протестировать, как оно работает. Для этого есть специальная аннотация `@SpringBootTest`, она указывает что для тестов необходимо поднять контекст. (После создания приложения через Spring Initializr у вас уже будет создан класс `DemoApplicationTests` с одним пустым тестом `contextLoads()`, это простой способ проверить, что контекст поднимается, и нет ошибок в конфигурации) Давайте напишем тест, который будет проверять один из наших endpoint'ов:

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class UserControllerTest {
    @LocalServerPort
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    public void greetingShouldReturnDefaultMessage() {
        assertThat(this.restTemplate.getForObject("http://localhost:" + port + "/admin/user/new",
                String.class)).contains("<title>Please sign in</title>");
    }
}
```

`webEnvironment=RANDOM_PORT` указывает, что надо поднять приложение на рандомном порту, это позволяет избежать конфликтов в тестовой среде. `@LocalServerPort` внедряет этот порт в переменную в тесте. Spring Boot предоставляет заранее сконфигурированный `TestRestTemplate`, который можно внедрить через `@Autowired`.

Другой полезный подход это не поднимать сервер, а тестировать только web слой. Spring обрабатывает входящие HTTP запросы и передает их вашему контроллеру. Таким способом ваш код будет вызван точно так же, как и в реальной жизни, но без затрат на поднятие сервера. Чтобы сделать это, надо использовать Spring’s MockMvc и добавить аннотацию `@AutoConfigureMockMvc` на ваш класс.

```less
@SpringBootTest
@AutoConfigureMockMvc
public class UserControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testUsrForm() throws Exception {
        this.mockMvc.perform(get("/admin/user/new"))
                .andDo(print())
                .andExpect(status().is3xxRedirection());
    }
}
```

Давайте напишем тесты чуть посложнее:

- поиска пользователя по id действительно возвращает этого пользователя. Для этого создадим пользователя через репозиторий и попробуем получить его, вызвав метод контроллера.
- создание пользователя. Сначала проверим, что пользователя нет в базе, попробуем его создать через контроллер и попробуем найти этого пользователя в базе еще раз.

```java
@AutoConfigureMockMvc
@SpringBootTest
public class UserControllerTest {

    @Autowired
    private MockMvc mvc;
    @Autowired
    private UserRepository userRepository;

    @Test
    @WithMockUser(value = "admin", password = "123", roles = {"ADMIN"})
    public void testUserDetails() throws Exception {
        User user = new User();
        user.setUsername("testUser");
        user.setPassword("testPass");

        User savedUser = userRepository.save(new User());

        mvc.perform(get("/admin/user/" + savedUser.getId()).with(csrf()))
                .andExpect(status().is2xxSuccessful())
                .andExpect(view().name("user_form"))
                .andExpect(model().attributeExists("user"))
                .andExpect(model().attribute("user", new BaseMatcher<UserDto>() {
                    @Override
                    public void describeTo(Description description) {
                    }

                    @Override
                    public boolean matches(Object o) {
                        if (o instanceof UserDto) {
                            UserDto userRepo = (UserDto) o;
                            return userRepo.getId().equals(savedUser.getId());
                        }
                        return false;
                    }
                }));
    }

    @Test
    @WithMockUser(value = "admin", password = "123", roles = {"ADMIN"})
    public void testUserCreation() throws Exception {
        Optional<User> user = userRepository.findUserByUsername("testUser");
        assertFalse(user.isPresent());

        mvc.perform(post("/admin/user")
                .contentType(MediaType.APPLICATION_FORM_URLENCODED)
                .param("username", "testUser")
                .param("password", "testPass")
                .with(csrf()))
                .andExpect(status().is3xxRedirection())
                .andExpect(view().name("redirect:/user"));

        user = userRepository.findUserByUsername("testUser");
        assertTrue(user.isPresent());
    }
}
```

Для того чтобы тест заработал, надо не забыть добавить зависимость в pom.xml.

```php-template
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <scope>test</scope>
</dependency>
```

## Тестирование Web-слоя в изоляции

Тесты, описанные выше, являются примерами [End-to-end (E2E)](https://www.katalon.com/resources-center/blog/end-to-end-e2e-testing/) тестов. Проще говоря, мы проверяем, что запрос работает корректно, проходя через всю систему. Такие тесты важны, однако в некоторых ситуациях являются избыточными. Иногда достаточно протестировать Web-слой изолированно.

Предположим, что у нас есть контроллер, который регистрирует события посещения страницы пользователем.

```typescript
@RestController
@RequestMapping("/audit")
@Validated
public class AuditController {

  private final AuditService auditService;

  public AuditController(AuditService auditService) {
      this.auditService = auditService;
  }

  @PostMapping
  public void registerPageVisitEvent(@RequestParam @NotEmpty String pageName,
                                     @RequestBody @NotNull Map<String, Object> attributes) {
      auditService.registerEvent(SUCCESS, pageName, attributes);
  }

  @ExceptionHandler(ConstraintViolationException.class)
  @ResponseStatus(BAD_REQUEST)
  public ResponseEntity<String> handleConstraintViolationException(ConstraintViolationException e) {
      return new ResponseEntity<>("not valid due to validation error: " + e.getMessage(), BAD_REQUEST);
  }
}
```

Пусть мы хотим проверить, что валидация параметров работает корректно и ответ содержит ожидаемый HTTP-статус. Причем нам не важна функциональность самого `AuditService`. В данном случае можно обойтись мокированием сервиса.

> Стоит заострить внимание на аннотации `@Validated` и описанном Exception Handler. Дело в том, что по умолчанию Spring производит валидацию только тела запроса (`@RequestBody`). Остальные параметры пропускаются, даже при наличии соответствующих аннотаций. `@Validated` сигнализирует о том, что следует включить проверку для всех параметров, которые передаются в методы. Однако в этом случае будет выброшено другое исключение - `ConstraintViolationException`. По умолчанию оно преобразуется в HTTP-статус 500, что не является ожидаемым поведением. Мы добавляем соответствующий Exception Handler, чтобы исправить это.

Объявим тестовый класс.

```java
@WebMvcTest(AuditController.class)
class AuditControllerTest {
  @Autowired
  private MockMvc mockMvc;
  @Autowired
  private ObjectMapper objectMapper;
  @MockBean
  private AuditService auditService;
  ...
}
```

В отличие от `@SpringBootTest`, `@WebMvcTest` поднимает не весь контекст, а ту только ту его часть, которая относится к web-слою. В реальных приложениях это может значительно ускорить старт и выполнение тестов.

> Подробнее о Spring Test Slices можно почитать в [документации](https://spring.io/blog/2016/08/30/custom-test-slice-with-spring-boot-1-4).

Бины `MockMvc` и `ObjectMapper` можно получить с помощью `@Autowired`. Дополнительная аннотация `@AutoConfigureMockMvc` не требуется, так как `@WebMvcTest` уже содержит ее в себе.

В качестве параметра `@WebMvcTest` принимает список контроллеров, которые должны быть добавлены в Spring Context. Если параметр отсутствует, добавлены будут все существующие.

Теперь напишем простой тест на happy-path. Если `pageName` не пустое и `attributes` не равно `null`, запрос должен завершиться успешно.

```java
@Test
void shouldReturn200IfParamsValidationPasses() throws Exception {
    String pageName = "Страница заказов";
    Map<String, Object> attributes = Map.of("itemsCount", 20);

    mockMvc.perform(
        post("/audit")
            .queryParam("pageName", pageName)
            .contentType(APPLICATION_JSON)
            .content(objectMapper.writeValueAsString(attributes))
    ).andDo(print()).andExpect(status().is2xxSuccessful());

    verify(auditService, times(1)).registerEvent(
        eq(SUCCESS),
        eq(pageName),
        eq(attributes)
    );
}
```

С помощью `ObjectMapper` мы сериализуем `attributes` в JSON. После выполнения запроса проверяем, что `auditService.registerEvent` был вызван нужное кол-во раз с ожидаемым набором аргументов.

Есть более сложный вариант. Например, можем проверить, что в случае, если `pageName` равен пустой строке, валидация не пройдет и код возврата будет равен 400.

```perl
@Test
void shouldReturn400IfPageNameIsEmpty() throws Exception {
    Map<String, Object> attributes = Map.of("itemsCount", 20);

    mockMvc.perform(
        post("/audit")
            .queryParam("pageName", "")
            .contentType(APPLICATION_JSON)
            .content(objectMapper.writeValueAsString(attributes))
    ).andDo(print()).andExpect(status().isBadRequest());

    verify(auditService, times(0)).registerEvent(
        eq(SUCCESS),
        eq(""),
        eq(attributes)
    );
}
```

Заметьте, что в функцию `verify` теперь передается параметр `times(0)`. Из-за того, что валидация не была пройдена успешно, бизнес-логика не должна быть вызвана.

Аналогично можно протестировать кейс, когда тело запроса отсутствует.

```java
@Test
void shouldReturn400IfAttributesIsNull() throws Exception {
    String pageName = "Карточка товара";

    mockMvc.perform(
        post("/audit")
            .queryParam("pageName", pageName)
            .contentType(APPLICATION_JSON)
    ).andDo(print()).andExpect(status().isBadRequest());

    verify(auditService, times(0)).registerEvent(
        eq(SUCCESS),
        eq(pageName),
        eq(null)
    );
}
```


# Resources

- [Mocks](https://stepik.org/lesson/747559/step/1?unit=749367)
- [Вебинар №6 по теме «Testing»](https://www.youtube.com/watch?v=VzsGBHnjgEA)
- 