Spring Security — это мощный и гибкий фреймворк, который используется для защиты приложений на базе Spring. Он предоставляет все необходимые инструменты для настройки аутентификации, авторизации, защиты от CSRF-атак и других аспектов безопасности.

### 1. Подключение Spring Security к проекту

Чтобы начать работу с Spring Security, нужно добавить зависимость в ваш проект. Если вы используете **Maven**, это можно сделать, добавив следующий блок в `pom.xml`:

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

После добавления зависимости Spring Security автоматически активируется в проекте.

### 2. Базовая конфигурация безопасности

По умолчанию, когда вы добавляете Spring Security в проект, приложение автоматически защищается и требует аутентификации. Spring Security создает базовую конфигурацию, которая включает:

- **Форму логина** по умолчанию.
- **Базовый механизм аутентификации** (встроенный пользователь с именем `user` и сгенерированным паролем).
- Защиту всех URL-адресов, которые требуют аутентификации.

#### Как это работает по умолчанию:

Когда вы запускаете приложение с включенным Spring Security, оно будет требовать от пользователя аутентификации для любого запроса. Spring сгенерирует случайный пароль, который появится в консоли.

### 3. Кастомизация базовой конфигурации

#### 3.1. Создание собственного пользователя

Чтобы настроить своего пользователя, нужно определить **конфигурацию безопасности**. В Spring Boot это делается с помощью класса, который расширяет `WebSecurityConfigurerAdapter` (до Spring 2.7) или использует компонент `SecurityFilterChain` (начиная с версии Spring 2.7).

Пример настройки пользователей:

### 4. Объяснение конфигурации

#### 4.1. Настройка пользователей

Метод `inMemoryAuthentication()` позволяет создать пользователей в памяти приложения для тестирования и базовых конфигураций. Для каждого пользователя задается имя, пароль и роли.

- `password("{noop}password")` — `{noop}` означает, что пароль не хэшируется (только для тестов).
- `roles("ADMIN")` — назначает роль пользователю.

#### 4.2. Настройка авторизации

Метод `authorizeRequests()` определяет, какие URL требуют авторизации:

- `antMatchers("/admin/**").hasRole("ADMIN")` — страницы с URL, начинающимся с `/admin/`, могут быть доступны только пользователям с ролью "ADMIN".
- `antMatchers("/user/**").hasRole("USER")` — страницы с URL `/user/**` доступны для пользователей с ролью "USER".
- `antMatchers("/", "/home").permitAll()` — разрешает доступ ко всем пользователям на главную страницу.

#### 4.3. Настройка логина

Метод `formLogin()` настраивает форму логина. Можно кастомизировать URL для логина, регистрацию и отображение формы входа.

- `loginPage("/login")` — указывает, что кастомная страница входа доступна по адресу `/login`.
- `permitAll()` — позволяет любому пользователю обращаться к странице логина.

### 5. Настройка паролей

В Spring Security крайне важно использовать безопасные методы для хранения паролей. По умолчанию используется **BCryptPasswordEncoder** для хэширования паролей.

Для этого создается `PasswordEncoder`, который будет хэшировать пароли:

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

Вместо `{noop}` для паролей теперь будет использоваться реальный механизм хэширования:

```java
.auth
    .inMemoryAuthentication()
    .withUser("admin")
    .password(passwordEncoder().encode("password"))
    .roles("ADMIN");
```


### 6. Реализация кастомных страниц входа

Если вы хотите создать свою форму для входа, нужно определить контроллер и страницу логина.

Пример контроллера:

```java
@Controller
public class LoginController {

    @GetMapping("/login")
    public String login() {
        return "login";
    }
}
```

Пример HTML-страницы:

```java
<form method="post" action="/login">
    <div>
        <label for="username">Username:</label>
        <input type="text" id="username" name="username"/>
    </div>
    <div>
        <label for="password">Password:</label>
        <input type="password" id="password" name="password"/>
    </div>
    <div>
        <button type="submit">Login</button>
    </div>
</form>
```

### 7. CSRF защита

**CSRF** (Cross-Site Request Forgery) — это атака, при которой злоумышленник может заставить пользователя выполнить нежелательные действия в веб-приложении, в котором он аутентифицирован.

Spring Security по умолчанию защищает приложения от CSRF-атак. В большинстве случаев эту защиту лучше не отключать. Однако для REST API можно ее отключить, если нет сессий:

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .csrf().disable()
        .authorizeRequests()
        .antMatchers("/api/**").permitAll();
}
```

### 8. Основные шаги для понимания Spring Security:

1. **Аутентификация**: Процесс проверки пользователя (кто вы?).
2. **Авторизация**: Процесс предоставления доступа (что вам разрешено делать?).
3. **Шифрование паролей**: Никогда не храните пароли в открытом виде — используйте `BCryptPasswordEncoder`.
4. **CSRF-защита**: По умолчанию включена в Spring Security, предотвращает атаки на запросы.
5. **Формы логина**: Могут быть настроены для поддержки кастомных страниц для входа.


# Источники

- [JWT-аутентификация при помощи Spring Boot 3 и Spring Security 6](https://habr.com/ru/articles/784508/)
- [Documentation](https://docs.spring.io/spring-security/reference/getting-spring-security.html)
- [Stepik. MTS-teta middle. Spring security](https://stepik.org/lesson/747552/step/2?unit=749360)
- 