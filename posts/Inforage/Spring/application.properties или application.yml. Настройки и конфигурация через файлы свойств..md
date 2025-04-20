В Spring Boot, файлы конфигурации `application.properties` и `application.yml` используются для задания настроек и параметров приложения. Эти файлы позволяют управлять различными аспектами работы приложения без необходимости вносить изменения в код. В этом разделе мы рассмотрим основные особенности и примеры использования обоих типов файлов конфигурации.

### **Файлы конфигурации в Spring Boot**

#### **1. Формат файлов**

- **application.properties**: Файл конфигурации в формате ключ-значение. Каждая строка представляет собой пару ключ-значение, разделенную символом "=" Это наиболее простой и часто используемый формат.

```properties
server.port=8080
spring.datasource.url=jdbc:mysql://localhost:3306/mydatabase
spring.datasource.username=root
spring.datasource.password=root
```

**`application.yml`**: Файл конфигурации в формате YAML (Yet Another Markup Language), который представляет данные в иерархической структуре. YAML более читаем и удобен для сложных конфигураций, поддерживает вложенные структуры.

```yaml
server:
  port: 8080
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydatabase
    username: root
    password: root
```

#### **2. Настройка параметров приложения**

Оба типа файлов поддерживают одни и те же настройки и параметры. Выбор между `application.properties` и `application.yml` зависит от предпочтений и сложности конфигурации.

##### Примеры настройки параметров

- Настройка порта сервера

```properties
server.port=8080
```

```yaml
server:
  port: 8080
```

- Настройка логирования:

```properties
logging.level.org.springframework=INFO
```

```yaml
logging:
  level:
    org.springframework: INFO
```

#### Профили конфигурации

Spring Boot поддерживает использование различных профилей для конфигурации, что позволяет задавать различные параметры для различных сред (например, разработка, тестирование, продакшн). Профили могут быть указаны в конфигурационных файлах через суффиксы.

Пример использования профилей:

Файл конфигурации для разработки `application-dev.yml`:

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/devdatabase
    username: devuser
    password: devpassword
```

Файл конфигурации для продакшн `application-prod.yml`:

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/proddatabase
    username: produser
    password: prodpassword
```

Чтобы активировать профиль, укажите его в `application.properties` или через переменные окружения:

**application.properties:**

```properties
spring.profiles.active=dev
```

#### Использование переменных окружения

Spring Boot позволяет использовать переменные окружения и системные свойства в конфигурационных файлах. Это удобно для настройки приложений в различных средах без изменения конфигурационных файлов.

Пример использования переменных окружения в `application.properties`:

```properties
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
```

Здесь `${DB_URL}`, `${DB_USERNAME}`, и `${DB_PASSWORD}` будут заменены на значения переменных окружения `DB_URL`, `DB_USERNAME` и `DB_PASSWORD`.

#### **5. Программная конфигурация**

Хотя файлы конфигурации являются основным способом настройки, Spring Boot также поддерживает программную конфигурацию через Java-код. Это может быть полезно для динамических или более сложных конфигураций.

Пример программной конфигурации:

```java
@Configuration
public class AppConfig {
    
    @Bean
    public DataSource dataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setUrl(System.getenv("DB_URL"));
        dataSource.setUsername(System.getenv("DB_USERNAME"));
        dataSource.setPassword(System.getenv("DB_PASSWORD"));
        return dataSource;
    }
}
```

