---
Определение: Используются для управления конфигурацией приложения в различных средах, таких как разработка, тестирование и продакшн. Это позволяет вам настраивать разные бины и их зависимости в зависимости от текущего профиля, который активен в вашем приложении.
Примечание:
---
### Основные аспекты профилей в Spring

1. **Определение профилей**
    
    Профили позволяют разделить конфигурацию приложения на несколько групп, которые можно активировать в зависимости от условий. Это особенно полезно для управления разными конфигурациями в различных средах.
    
2. **Активирование профилей**
    
    Профили активируются через конфигурационные файлы, переменные окружения или программно.
    
3. **Пример конфигурации**
    
    Рассмотрим основные способы использования профилей.

### Примеры использования профилей

#### 1. Определение профилей в конфигурации

Вы можете определить профили в конфигурационных классах с помощью аннотации `@Profile`.

```java
@Configuration
public class AppConfig {

    @Bean
    @Profile("dev")
    public DataSource devDataSource() {
        return new DataSource("dev-url");
    }

    @Bean
    @Profile("prod")
    public DataSource prodDataSource() {
        return new DataSource("prod-url");
    }
}
```

В этом примере `devDataSource` будет создан только в профиле `dev`, а `prodDataSource` — только в профиле `prod`.

#### 2. Активирование профилей через `application.properties`

Вы можете активировать профиль в файле `application.properties` или `application.yml`.

Пример для `application.properties`:

```properties
spring.profiles.active=dev
```

Пример для `application.yml`:

```yaml
spring:
  profiles:
    active: dev
```

#### 3. Активирование профилей через переменные окружения

Профили можно также активировать через переменные окружения.

```bash
export SPRING_PROFILES_ACTIVE=dev
```

#### 4. Активирование профилей программно

Профили можно активировать программно, используя `ConfigurableApplicationContext`.

```java
public class Application {

    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(Application.class);
        app.setAdditionalProfiles("dev");
        ConfigurableApplicationContext context = app.run(args);
    }
}
```

### Дополнительные возможности

1. **Разделение конфигураций по профилям**
    
    Вы можете создать несколько файлов конфигурации для различных профилей. Например, `application-dev.properties` для профиля `dev` и `application-prod.properties` для профиля `prod`.
    
2. **Аннотации на уровне классов и методов**
    
    Профили могут применяться не только на уровне классов, но и на уровне методов. Вы можете помечать методы аннотацией `@Profile`, чтобы они выполнялись только при активированном профиле.

```java
@Component
public class MyService {

    @Bean
    @Profile("dev")
    public void devMethod() {
        // код для профиля dev
    }

    @Bean
    @Profile("prod")
    public void prodMethod() {
        // код для профиля prod
    }
}
```