### 1. **RestTemplate**: Классический способ работы с HTTP

`RestTemplate` был основным инструментом в Spring для выполнения HTTP-запросов до появления **WebClient**. Он предоставляет синхронный API, где каждый HTTP-запрос блокирует поток, пока не придет ответ.

```java
public class RestTemplateExample {
    private static final String API_URL = "https://api.example.com/users";

    public static void main(String[] args) {
        RestTemplate restTemplate = new RestTemplate();

        // GET запрос
        ResponseEntity<String> response = restTemplate.getForEntity(API_URL, String.class);
        System.out.println("Response: " + response.getBody());

        // POST запрос
        User user = new User("John", "Doe");
        User createdUser = restTemplate.postForObject(API_URL, user, User.class);
        System.out.println("Created User: " + createdUser);
    }
}
```

### 2. **WebClient**: Современный и асинхронный

`WebClient` — это новый клиент для HTTP-запросов в Spring, который является частью **Spring WebFlux**. Он поддерживает как синхронные, так и асинхронные вызовы и может работать в реактивных приложениях.

#### Основные особенности:

- **Асинхронный и неблокирующий**: `WebClient` может отправлять запросы и получать ответы в асинхронном режиме, что делает его более подходящим для высоконагруженных систем и микросервисов.
- Поддержка реактивного программирования: `WebClient` поддерживает работу с реактивными типами, такими как `Mono` и `Flux`, которые позволяют обрабатывать данные по мере их поступления.
- Мощные функции конфигурации и расширяемости: можно настраивать заголовки, параметры и интерсепторы для запросов.
- Поддержка различных кодеков для сериализации/десериализации данных, включая JSON, XML, и другие.

```java
import reactor.core.publisher.Mono;
public class WebClientExample {
    private static final String API_URL = "https://api.example.com/users";

    public static void main(String[] args) {
        WebClient webClient = WebClient.create();

        // Асинхронный GET запрос
        Mono<String> responseMono = webClient.get()
            .uri(API_URL)
            .retrieve()
            .bodyToMono(String.class);

        // Блокируем, чтобы получить ответ (не обязательно в реальных асинхронных приложениях)
        String response = responseMono.block();
        System.out.println("Response: " + response);

        // Асинхронный POST запрос
        User user = new User("Jane", "Doe");
        Mono<User> createdUserMono = webClient.post()
            .uri(API_URL)
            .bodyValue(user)
            .retrieve()
            .bodyToMono(User.class);

        User createdUser = createdUserMono.block();
        System.out.println("Created User: " + createdUser);
    }
}
```

### Сравнение

|Функциональность|RestTemplate|WebClient|
|---|---|---|
|Архитектура|Синхронная (блокирующая)|Асинхронная и синхронная|
|Поддержка реактивного программирования|Нет|Да|
|Производительность|Ограничена из-за блокировки потоков|Высокая из-за неблокирующей природы|
|Поддержка Spring Boot|Поддерживается|Рекомендуется для новых проектов|
|Простота использования|Проще для синхронных запросов|Более гибкий, но сложнее|
