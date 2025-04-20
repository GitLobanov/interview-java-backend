![[Pasted image 20241007153622.png]]

1. **`@Component`**
    
    - **Назначение**: Определяет класс как компонент Spring. Это базовая аннотация для создания бина.

2. **`@Service`**

- **Назначение**: Специальный стереотип для бизнес-логики, часто используемый для обозначения сервисных классов.

3. **`@Repository`**

- **Назначение**: Специальный стереотип для работы с данными (DAO), обычно используемый для классов, которые взаимодействуют с базой данных. Работает с Spring Data, оборачивает ошибки связанные с бд, в runtime exceptions.

4. **`@Controller`**

- **Назначение**: Определяет класс как контроллер в веб-приложении, который обрабатывает HTTP-запросы.

5. **`@RestController`**

- **Назначение**: Сочетает `@Controller` и `@ResponseBody`, делая класс контроллером RESTful веб-сервиса.

6. [[@Autowired]]

- **Назначение**: Автоматическое внедрение зависимостей. Можно использовать для полей, сеттеров или конструкторов.

7.  [[@Qualifier]]

- **Назначение**: Уточняет, какой конкретно бин следует использовать при внедрении, если есть несколько бинов одного типа.

8. **`@Value`**

- **Назначение**: Внедрение значений из конфигурационных файлов (например, `application.properties`).

```java
@Value("${my.property}")
private String myProperty;
```

9. **`@Configuration`**

- **Назначение**: Определяет класс, содержащий методы для конфигурации бинов. Эти методы помечаются аннотацией `@Bean`.

```java
@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}
```

10. **`@Bean`**

- **Назначение**: Используется в конфигурационных классах для определения методов, которые создают и настраивают бины.

11. **`@PostConstruct`**

- **Назначение**: Метод будет вызван после завершения инициализации бина.

12. **`@PreDestroy`**

- **Назначение**: Метод будет вызван перед уничтожением бина.

13. [[Events|@EventListener]]

- **Назначение**: Аннотация для методов, которые обрабатывают события Spring.

14. **`@RequestMapping`**

- **Назначение**: Определяет URL-шаблон, по которому будет доступен метод контроллера (используется в Spring MVC).

15. **`@ResponseBody`**

- **Назначение**: Указывает, что метод возвращает данные напрямую в теле ответа (используется в RESTful контроллерах).

```java
@ResponseBody
@RequestMapping("/data")
public MyData getData() {
    return new MyData();
}
```

16. **`@PathVariable`**

- **Назначение**: Извлечение значений из URI шаблона в контроллерах.

```java
@GetMapping("/items/{id}")
public Item getItem(@PathVariable("id") Long id) {
    return itemService.getItem(id);
}
```

17. **`@RequestParam`**

- **Назначение**: Извлечение параметров запроса из URL.

```java
@GetMapping("/items")
public List<Item> getItems(@RequestParam("type") String type) {
    return itemService.getItemsByType(type);
}
```


18. [[@Lookup]]

- Используется для внедрения бина из контекста приложения, при этом каждый вызов метода, помеченного этой аннотацией, будет возвращать новый экземпляр бина. Это особенно полезно для получения прототипных бинов из синглтон-бина.

19. SpringBootApplication

- Содержит в себе: @Configuration, @EnableAutoConfiguration, @ComponentScan

20. EnableAutoConfiguration

- Спринг сам конфигурирует исходя из classpath

21. EnableConfigServer

- Обращаем приложение в сервер, откуда другие апсы могут взять конфигурацию

22. EnableEurekaServer 

- Делает наше приложение Eureka discovery server. другие апсы могут получить сервисы отсюда

23. EnableDiscoveryClient 

- Регистрирует приложение в discovery server, и дает возможность обнаружить другие апсы в нем

24. EnableCircuitBreaker 

- Конфигурирует Hytrix circuit break protocols

25. @HystrixCommand(fallbackMethod = "fallbackMethodName")

- Маркирует метод to fall back в другой метод, если он не может успешно завершится

26. ComponentScan

- Сканирует пакет в поисках @Configuration классов

27. Required 

- fall the configuration, если зависимость не может быть заинжектена

28. Scheduled

- Метод запускается переодически

29. @EnableScheduling

- Включает спринговый таск планировщик

30. SpringBootTest

- Интеграционные спринг тесты

31. AutoConfigureMockMvc

- Конфигурирует MockMvc объект для текстов HTTP запросов

32. EnableJpaRepositories

- Запускает поиск классов помеченных @Repository

33. @DataJpaTest

- Создает отдельную среду выполнения
- По умолчанию использует H2
- Автоматически настраивает EntityManager

34. ConfigurationProperties

- Импортирует настройки из проперти файла

35. EnableTransactionManagement

- Включает Spring'DB transaction management though @Bean objects

36. Transactional