Spring Security предоставляет мощный и гибкий способ защищать URL-адреса вашего приложения с помощью аннотаций и Java-конфигурации. Вы можете настроить доступ к различным частям вашего веб-приложения в зависимости от ролей пользователей, статуса аутентификации и других критериев.

#### 1. **Основы защиты URL**

Spring Security позволяет вам ограничить доступ к ресурсам на уровне URL, определяя правила для каждого маршрута. Это можно сделать с помощью:

- Аннотаций, например, `@Secured`, `@PreAuthorize`, или `@RolesAllowed`.
- Java-конфигурации с использованием класса `WebSecurityConfigurerAdapter` и методов `authorizeRequests()` и других.

#### 2. **Пример базовой настройки защиты URL**

Создание класса конфигурации безопасности, который расширяет `WebSecurityConfigurerAdapter`, — это основной подход для защиты URL в Spring Security.

Пример базовой конфигурации:

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()  // Доступ к публичным страницам
                .antMatchers("/admin/**").hasRole("ADMIN") // Только для пользователей с ролью ADMIN
                .antMatchers("/user/**").hasAnyRole("USER", "ADMIN") // Доступ для пользователей с ролями USER или ADMIN
                .anyRequest().authenticated()  // Все остальные запросы требуют аутентификации
            .and()
            .formLogin() // Включаем стандартную форму логина
                .loginPage("/login") // Настраиваем свою страницу входа
                .permitAll()
            .and()
            .logout() // Настраиваем logout
                .permitAll();
    }

    // Конфигурация кодирования паролей
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```


В этом примере:

- **`/public/**`** — доступен всем пользователям, даже без аутентификации.
- **`/admin/**`** — доступен только пользователям с ролью `ADMIN`.
- **`/user/**`** — доступен пользователям с ролями `USER` или `ADMIN`.
- Все остальные запросы требуют аутентификации.

#### 3. **Описание методов защиты URL**

- **`antMatchers()`** — определяет URL-шаблоны для которых задаются правила доступа.
- **`permitAll()`** — разрешает доступ без каких-либо ограничений.
- **`hasRole(String role)`** — проверяет, обладает ли пользователь указанной ролью. Важно помнить, что Spring Security добавляет префикс `ROLE_` к ролям автоматически.
- **`hasAnyRole(String... roles)`** — проверяет, соответствует ли пользователь одной из указанных ролей.
- **`authenticated()`** — указывает, что доступ к ресурсу возможен только для аутентифицированных пользователей.

#### 4. **Дополнительные возможности**

##### 4.1. **Настройка страниц ошибки и логина**

Вы можете настроить кастомные страницы для логина и ошибок авторизации:

```java
http
    .formLogin()
        .loginPage("/custom-login")  // Своя страница логина
        .failureUrl("/login-error")  // Страница при ошибке авторизации
        .permitAll()
    .and()
    .exceptionHandling()
        .accessDeniedPage("/403");  // Страница ошибки доступа
```

##### 4.2. **CSRF-защита**

Spring Security по умолчанию включает защиту от CSRF-атак. 

Отключить в старых версиях:

```java
http
    .csrf().disable();
```

В новых: 

```java
.csrf(csrf -> csrf.disable())
```

#### 5. **Пример для проекта Nihongo Nexus**

В проекте **Nihongo Nexus**, можно защитить URL-адреса, предоставляющие доступ к различным уровням обучения и соревнованиям.

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests()
            .antMatchers("/home").permitAll()  // Главная страница доступна всем
            .antMatchers("/explore/**").hasRole("EXPLORER")  // Только для роли исследователей
            .antMatchers("/arena/**").hasRole("COMPETITOR")  // Только для участников арены
            .antMatchers("/admin/**").hasRole("ADMIN")  // Админ-панель только для админов
            .anyRequest().authenticated()  // Остальные запросы требуют аутентификации
        .and()
        .formLogin()
            .loginPage("/login")  // Кастомная страница логина
            .permitAll()
        .and()
        .logout()
            .permitAll();
}
```

#### 7. **Переопределение конфигурации через аннотации**

Для локальной настройки доступа к методам можно использовать аннотации Spring Security. Например:

```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteTask(Long taskId) {
    // метод только для пользователей с ролью ADMIN
}
```

Используя `@PreAuthorize`, можно управлять доступом на уровне методов, что добавляет дополнительную гибкость.