Spring Boot представляет собой расширение Spring Framework, которое упрощает разработку и конфигурацию приложений, минимизируя необходимость написания шаблонного кода и конфигурационных файлов. Основная цель Spring Boot — сделать создание и развертывание приложений более быстрым и простым. Для этого он предоставляет мощные возможности автоматической конфигурации и преднастроенных шаблонов.

#### **Аннотация `@SpringBootApplication`**

Основной класс приложения помечается аннотацией `@SpringBootApplication`. Эта аннотация объединяет три важные аннотации:

- `@SpringBootConfiguration`
    - Является расширением аннотации `@Configuration`, указывая, что класс может содержать определение бинов.
    - Задает текущий класс как источник конфигурации Spring.
- **`@EnableAutoConfiguration`**:
    - Включает автоконфигурацию Spring Boot.
    - Автоматически настраивает компоненты на основе зависимостей, найденных в класспасе.
    - Использует файлы `META-INF/spring.factories` для определения автоконфигурационных классов.
- **`@ComponentScan`**:
    - Определяет, какие пакеты следует сканировать для поиска компонентов, сервисов, репозиториев и других бинов.
    - По умолчанию сканируются все пакеты под пакетом, где находится класс с аннотацией `@SpringBootApplication`.
#### **Автоматическая конфигурация**

Spring Boot автоматически настраивает ваше приложение на основе зависимостей, указанных в `pom.xml` или `build.gradle`. Например, если вы добавите зависимость `spring-boot-starter-data-jpa`, Spring Boot автоматически настроит DataSource и EntityManagerFactory для работы с базой данных JPA.

Метод `SpringApplication.run()` выполняет следующие действия:

- Создает контекст приложения.
- Загружает конфигурацию.
- Инициализирует бины и их зависимости.
- Запускает встроенный сервер (если применимо, например, для веб-приложений).

##### Вы можете настроить поведение `SpringApplication` перед запуском приложения:

```java
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication application = new SpringApplication(MyApplication.class);
        application.setAddCommandLineProperties(true);
        application.setBannerMode(Banner.Mode.OFF);
        application.run(args);
    }
}
```

#### **Стартовые зависимости (Starters)**

Spring Boot предоставляет стартовые зависимости (starters), которые включают в себя наборы предварительно настроенных зависимостей для различных задач. Это упрощает добавление и настройку функциональности в вашем приложении.

Примеры стартовых зависимостей:

- **`spring-boot-starter-web`**: Для создания веб-приложений, включает в себя Spring MVC, Jackson, Tomcat и другие зависимости.
- **`spring-boot-starter-data-jpa`**: Для работы с JPA, включает Hibernate и Spring Data JPA.
- **`spring-boot-starter-security`**: Для добавления базовой безопасности и аутентификации.

#### **Компоненты Spring Boot**

1. **Автоконфигурация**: Упрощает настройку, автоматически настраивая компоненты вашего приложения.
2. **Embedded Server**: Spring Boot может встроить сервер, такой как Tomcat или Jetty, так что вам не нужно настраивать внешний сервер для разработки и развертывания.
3. **Actuator**: Предоставляет встроенные эндпоинты для мониторинга и управления вашим приложением (например, `/actuator/health` и `/actuator/metrics`).

