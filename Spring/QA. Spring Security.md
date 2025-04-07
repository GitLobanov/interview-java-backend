> 1. Что такое аутентификация и авторизация?

Аутентификация — процесс верификации пользователя компьютерной системы.  Вот как он происходит в Spring:

1. Полученные пароль и имя пользователя преобразуются в экземпляр [UsernamePasswordAuthenticationToken](https://docs.spring.io/spring-security/site/docs/4.2.12.RELEASE/apidocs/org/springframework/security/authentication/UsernamePasswordAuthenticationToken.html). Он реализует интерфейс [Authentication](https://docs.spring.io/spring-security/site/docs/4.2.11.RELEASE/apidocs/org/springframework/security/core/Authentication.html).
2. Токен передается объекту `AuthenticationManager` для проверки
3. В случае удачной проверки AM возвращает заполненный объект `Authentication`
4. Устанавливается security context, с помощью вызова `SecurityContextHolder.getContext().setAuthentication(...)`

Авторизация — это процесс удостоверения в том, что у пользователя есть роль, требуемая чтобы сделать какое-либо действие. При авторизации проверяется, есть ли у вас соответствующие права на доступ к ресурсу.

Процесс:

1. По принципалу(principal) пользователя отображается его роль
2. Роль пользователя сверяется с ролью ресурса

Сначала происходит аутентификация, а потом — авторизация.

> 2. Как Security работает внутри?

- Используя Spring AOP proxy, которые наследуются от класса AbstractSecurityInterceptor. Применяется для методов вызывающих авторизацию.
- Веб-инфраструктура в Security основана на servlet-фильтрах.

Первым делом конфигурируется фильтр `DelegatingFilterProxy`. Он делегирует запрос в `FilterChainProxy`. `FilterChainProxy` — это бин, который принимает в конструкторе один или несколько `SecurityFilterChain` `SeccurityFilterChain` сравнивает URL в запросе со списком фильтров.

![[Pasted image 20241002142328.png]]

> 3. Основные объекты, участвующие в Spring Security

[`SecurityContextHolder`](https://docs.spring.io/spring-security/site/docs/4.2.12.RELEASE/apidocs/org/springframework/security/core/context/SecurityContextHolder.html) — содержит и предоставляет доступ к SecurityContext в приложении.
[`SecurityContext`](https://docs.spring.io/spring-security/site/docs/4.2.12.RELEASE/apidocs/org/springframework/security/core/context/SecurityContext.html) — дефолтная реализация Spring Security содержащая объект Authentication.
[`Authentication`](https://docs.spring.io/spring-security/site/docs/4.2.11.RELEASE/apidocs/org/springframework/security/core/Authentication.html) — предоставляет токен для запроса аутентификации или для принципала, который прошел аутентификацию. Также содержит список полномочий, к которым получил доступ принципал.
[`GrantedAuthority`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/core/GrantedAuthority.html) — содержит полномочия выданные прошедшему проверку принципалу.
[`UserDetails`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/core/userdetails/UserDetails.html) — содержит информацию о пользователе: пароль, логин, полномочия. Эта информация используется для создания объекта Authentication после удачной аутентификации.
[`UserDetailsService`](https://docs.spring.io/spring-security/site/docs/4.2.12.RELEASE/apidocs/org/springframework/security/core/userdetails/UserDetailsService.html) — этот сервис извлекает информацию о пользователе из хранилища(память программы, бд, и т.п.) и кладет ее в UserDetails.

> 4. Что такое делегирующий прокси фильтр?

Класс `DelegatingFilterProxy` — это класс, который реализует интерфейс `javax.Servlet.Filter`. Это специальный фильтр, который делегирует работу другим бинам, которые также являются фильтрами.

> 5. **Что такое security filter chain?**

Цепочка фильтров имплементит интерфейс [`SecurityFilterChain`](https://docs.spring.io/spring-security/site/docs/4.2.12.RELEASE/apidocs/org/springframework/security/web/SecurityFilterChain.html). Имплементацией, поставляемой Spring Security, является DefaultSecurityFilterChain.

Конструктор DSFC принимает несколько параметров. Первый параметр — request matcher. Остальные параметры — это фильтры, реализующие интерфейс servlet.Filter. Вот все фильтры, принимаемые DSFC:

- `ChannelProcessingFilter`
- `SecurityContextPersistenceFilter`
- `ConcurrentSessionFilter`
- Любой auth. фильтр: `UserNamePasswordAuthenticationFilter` / `CasAythenticationFilter` / `BasicAuthenticationFilter`
- `SecurityContextHolderAwareRequestFilter`
- `JaasApiIntegrationFilter`
- `RemeberMeAuthenticationFilter`
- `AnonymusAuthenticationFilter`
- `ExceptionTranslationFilter`
- `FilterSecurityInterceptor`

> 6. Что такое security context?

Основной объект — это `SecurityContextHolder`. Это место, где хранятся детали о текущем security context, например детали принципала который в текущий момент пользуется приложением. По умолчанию для хранения используется `ThreadLocal`.

```
//получение 
SecurityContext SecurityHolderContext.getContext()
```

Объект, возвращаемый методом `getContext()` это `SecurityContext`. Он позволяет получать и устанавливать объект `Authentication`. `Authentication` представляет следующие свойства:

- Коллекцию полномочий выданных принципалу
- Данные для удостоверения пользователя(логин, пароль)
- Details — доп. информация, если она нужна. Может быть равно null
- Принципал
- Authentication flag — boolean переменная, которая показывает успешно ли прошел проверку принципал

> 7. Как установить перехват перехода пользователя по определенным URL?


```java
@Override
protected void configure(HttpSecurity http) throws Exception {

      http.authorizeRequests()
       //игнорирование всех запросов на /resources
        .antMatchers("/resources/**").permitAll() 
        //для остальных запросов требуется одна из 2 ролей
        .antMatchers("/").hasAnyRole("ANONYMOUS", "USER")
        .antMatchers("/login)*").hasAnyRole("ANONYMOUS", "USER")  
        .antMatchers("/logoutr").hasAnyRole("ANONYMOUS", "USER") 
        //запрос на ресурсы ниже требуют роль ADMIN
        .antMatchers("iadmin/*").hasRole("ADMIN") 
        .antMatchers("/events/").hasRole("ADMIN")
}
```

> 8. Что означает * в методах antMatchers и mvcMatchers()?

Это выражение означает “любой”.

Есть 2 вида:

- `*` — перехватывает только на том уровне, на котором используется. Например, паттерн “/orders/*” проверит права пользователя, если пользователь перейдет по
    /orders/aliens или /orders/1, но не /orders/alien/1.
- `**` — перехватывает на всех уровнях.  Будут проверены любые запросы, /orders/aliens, /orders/1, /orders/alien/1.

> 9. Почему mvcMatcher более защищенный чем antMatcher?

Потому что `antMatcher(“/service”)` сопоставляет путь запроса только с `“/service”`, в то время как `mvcMatcher(“/service”)` сопоставляет с `“/service”`, `“/service.html”`, “/service.abc”.

> 10. Spring поддерживает хэширование паролей? Что такое соль?

Да, поддерживает. Для хэширования существует интерфейс `PasswordEncoder`, который содержит только один метод.

`static PasswordEncoder createDelegatingPasswordEncoder()`, который возвращает `DelegatePasswordEncoder`, настроенный по умолчанию.

Соль используется для вычисления хеш-значения пароля. Это последовательность рандомных чисел, которые используются для преобразования текстового пароля в хеш. Соль хранится в открытом виде рядом с хеш-паролем и может использоваться в дальнейшем при конвертации чистого пароля в хеш при новом логине пользователя.

> 11. Зачем нужна защита для методов? Как ее установить?

Spring Security поддерживает защиту отдельных методов в бинах(например, в контроллерах). Это дополнительный слой защиты для приложения. Ее требуется указать явно, используя аннотацию `@EnableGlobalMethodSecurity`.

> 12. Что делает аннотация @RolesAllowed?

Эта аннотация основана на JSR-250.  
`@RolesAllowed` позволяет настроить доступ к методам(например, в классе-контроллере) с помощью ролей.  

Пример: `@RolesAllowed(“ADMIN”)` будет пропускать только пользователей с ролью `ADMIN` Для использования нужно установить `@EnableGlobalMethodSecurity(jsr250Enabled=true)` на @Configuration классе + нужно чтобы эта аннотация была в classpath.


> 13. Расскажите про @PreAuthorize

`@PreAuthorize` позволяет настроить доступ к методу используя SpEL.  Для использования нужно установить `@EnableGlobalMethodSecurity(prePostEnabled=true)`

> 14. Как реализованы аннотации @RolesAllowed, @PreAuthorize?

Используется сквозная функциональность, с помощью Spring AOP(прокси-объекты). 

15. На каких паттернах строится Spring Security?

- Фабрика (Factory): Класс `AuthenticationManagerFactory` для создания аутентификационных менеджеров.
- Билдер (Builder): Класс `HttpSecurity`, который использует паттерн Builder для настройки безопасности приложения.
- Стратегия (Strategy): Интерфейс `AuthenticationProvider` для реализации различных стратегий аутентификации (например, `DaoAuthenticationProvider`, `OAuth2AuthenticationProvider`).
- Декоратор (Decorator): Класс `SecurityContextHolder`, который оборачивает контекст безопасности.
- Адаптер (Adapter): Класс `WebAuthenticationDetailsSource`, который адаптирует запросы для аутентификации.

16. 






