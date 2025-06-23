![[../../_res/Pasted image 20241007153622.png]]

1. @Component - Определяет класс как компонент Spring. Это базовая аннотация для создания бина.
2. @Service -  Специальный стереотип для бизнес-логики, часто используемый для обозначения сервисных классов.
3. @Repository - Специальный стереотип для работы с данными (DAO), обычно используемый для классов, которые взаимодействуют с базой данных. Работает с Spring Data, оборачивает ошибки связанные с бд, в runtime exceptions.
4. @Controller - Определяет класс как контроллер в веб-приложении, который обрабатывает HTTP-запросы.
5. @RestController - Сочетает `@Controller` и `@ResponseBody`, делая класс контроллером RESTful веб-сервиса. Возвращает value, к примеру json.
6. [[Annotations/@Autowired]] - Автоматическое внедрение зависимостей. Можно использовать для полей, сеттеров или конструкторов.
7.  [[Annotations/@Qualifier]] Уточняет, какой конкретно бин следует использовать при внедрении, если есть несколько бинов одного типа.
8. @Value - Внедрение значений из конфигурационных файлов (например, application.properties).

```java
@Value("${my.property}")
private String myProperty;
```

9. @Configuration - Определяет класс, содержащий методы для конфигурации бинов. Эти методы помечаются аннотацией @Bean.

```java
@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}
```

10. @Bean - Используется в конфигурационных классах для определения методов, которые создают и настраивают бины.
11. @PostConstruct - Метод будет вызван после завершения инициализации бина.
12. [[Events|@EventListener]] - Аннотация для методов, которые обрабатывают события Spring.
13. @RequestMapping - Определяет URL-шаблон, по которому будет доступен метод контроллера (используется в Spring MVC).
14. @ResponseBody - Указывает, что метод возвращает данные напрямую в теле ответа (используется в RESTful контроллерах).

```java
@ResponseBody
@RequestMapping("/data")
public MyData getData() {
    return new MyData();
}
```

16. @PathVariable -  Извлечение значений из URI шаблона в контроллерах.

```java
@GetMapping("/items/{id}")
public Item getItem(@PathVariable("id") Long id) {
    return itemService.getItem(id);
}
```

17. @RequestParam - Извлечение параметров запроса из URL.

```java
@GetMapping("/items")
public List<Item> getItems(@RequestParam("type") String type) {
    return itemService.getItemsByType(type);
}
```

18. [[Annotations/@Lookup]] - Используется для внедрения бина из контекста приложения, при этом каждый вызов метода, помеченного этой аннотацией, будет возвращать новый экземпляр бина. Это особенно полезно для получения прототипных бинов из синглтон-бина.
19. SpringBootApplication - Содержит в себе: @Configuration, @EnableAutoConfiguration, @ComponentScan
20. EnableAutoConfiguration - Спринг сам конфигурирует исходя из classpath
21. EnableConfigServer - Обращаем приложение в сервер, откуда другие апсы могут взять конфигурацию
22. EnableEurekaServer - Делает наше приложение Eureka discovery server. другие апсы могут получить сервисы отсюда
23. EnableDiscoveryClient - Регистрирует приложение в discovery server, и дает возможность обнаружить другие апсы в нем
24. EnableCircuitBreaker - Конфигурирует Hytrix circuit break protocols
25. ComponentScan - Сканирует пакет в поисках @Configuration классов
26. Scheduled - Метод запускается переодически
27. @EnableScheduling - Включает спринговый таск планировщик
28. SpringBootTest - Интеграционные спринг тесты
29. AutoConfigureMockMvc - Конфигурирует MockMvc объект для текстов HTTP запросов
30. EnableJpaRepositories - Запускает поиск классов помеченных @Repository
31. @DataJpaTest
- Создает отдельную среду выполнения
- По умолчанию использует H2
- Автоматически настраивает EntityManager
32. ConfigurationProperties - Импортирует настройки из проперти файла
33. EnableTransactionManagement - Включает Spring'DB transaction management though @Bean objects
34. @Transactional
35. @EnableCaching
36. @Cacheable - The simplest way to enable caching behavior for a method is to demarcate it with @Cacheable, and parameterize it with the name of the cache where the results would be stored