#### 1. **`@Secured`**

Аннотация `@Secured` используется для указания списка ролей, которые могут вызывать конкретный метод или доступ к ресурсу.

```java
@Secured("ROLE_ADMIN")
public void adminMethod() {
    // Этот метод доступен только для пользователей с ролью "ADMIN"
}
```

- Использование:
    - Аннотация работает только с ролями, начинающимися с префикса `ROLE_`.
    - Поддерживается только на уровне методов (не используется для классов).
- Особенности:
    - Простая и лаконичная, но не поддерживает выражений для более сложных условий безопасности.
- Целесообразность: Полезна для ограничения доступа к конкретным методам на основе ролей без сложной логики.

#### 2. **`@PreAuthorize`**

`@PreAuthorize` — более мощная аннотация, позволяющая определять выражения безопасности на основе ролей, пользователей и других условий до выполнения метода.

```java
@PreAuthorize("hasRole('ADMIN')")
public void adminTask() {
    // Метод только для пользователей с ролью "ADMIN"
}

@PreAuthorize("hasRole('USER') and #id == authentication.principal.id")
public void viewUser(Long id) {
    // Метод доступен пользователям с ролью "USER", если идентификатор пользователя совпадает с id
}
```

- Использование:
    - Может работать с ролевыми проверками, выражениями на основе SpEL (Spring Expression Language), а также с доступом на основе данных пользователей.
    - Можно использовать как на уровне классов, так и на уровне методов.
- Особенности:
    - Поддерживает сложные выражения и логику безопасности (например, проверка ролей, соответствие данных пользователей).
    - Применяется перед выполнением метода.

#### 3. **`@PostAuthorize`**

`@PostAuthorize` — аналог `@PreAuthorize`, но применяется **после** выполнения метода. В основном используется для проверки доступа на основе возвращаемых значений.

```java
@PostAuthorize("returnObject.owner == authentication.name")
public Task getTask(Long id) {
    // Проверяет, что пользователь является владельцем задачи после того, как задача была возвращена
    return taskService.getTask(id);
}
```

- Использование:
    - Применяется для постфактум проверки возвращаемого результата.
    - Может быть полезно для методов, возвращающих объекты, на основе которых можно принять решение о безопасности.
- Целесообразность: Используется реже, но может быть полезно для методов, которые возвращают данные, проверка которых возможна только после выполнения метода.
    

#### 4. **`@RolesAllowed`**

Аннотация `@RolesAllowed` является частью спецификации Java EE и используется для указания ролей, которым разрешен доступ к определенному методу.

```java
@RolesAllowed("ADMIN")
public void adminOnly() {
    // Метод доступен только для пользователей с ролью ADMIN
}
```

- Особенности:
    - Похож на `@Secured`, но является частью стандарта Java EE.
    - Может использоваться на уровне методов и классов.

#### 5. **`@EnableGlobalMethodSecurity`** **Deprecated**

Аннотация `@EnableGlobalMethodSecurity` не используется непосредственно для ограничения доступа, но активирует использование таких аннотаций, как `@Secured`, `@PreAuthorize`, и `@PostAuthorize`.

```java
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    // Включает поддержку аннотаций @PreAuthorize, @Secured и других
}
```

Особенности:
- Необходима для активации аннотаций методного уровня в Spring Security.
- Поддерживает активацию различных типов аннотаций.

#### 6. **`@AuthenticationPrincipal`**

Эта аннотация используется для внедрения текущего аутентифицированного пользователя в параметры метода.

```java
@GetMapping("/profile")
public String getProfile(@AuthenticationPrincipal UserDetails userDetails) {
    // Возвращает профиль текущего аутентифицированного пользователя
    return userDetails.getUsername();
}
```

- Особенности:
    - Позволяет легко получить доступ к данным текущего пользователя, который аутентифицирован через Spring Security.

#### 7. **`@PermitAll` и `@DenyAll`**

Аннотации `@PermitAll` и `@DenyAll` используются для полного разрешения или запрета доступа ко всем пользователям.


```java
@PermitAll
public void openMethod() {
    // Этот метод доступен всем
}

@DenyAll
public void restrictedMethod() {
    // Этот метод запрещен для всех
}
```

- Использование:
    - Полезны для явного указания, что метод либо доступен для всех, либо заблокирован.

#### 8. **`@EnableWebSecurity`**

Аннотация `@EnableWebSecurity` используется для включения поддержки Spring Security в вашем приложении. Она активирует фильтры и настройки безопасности.

```java
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    // Конфигурация безопасности
}
```

- Целесообразность: Эта аннотация необходима для любой конфигурации безопасности в Spring.

#### 9. **`@WithMockUser`**

Эта аннотация полезна для написания тестов. Она позволяет замокировать аутентифицированного пользователя при выполнении тестов.

```java
@Test
@WithMockUser(username = "admin", roles = {"ADMIN"})
public void testAdminAccess() {
    // Тест для проверки доступа под пользователем admin
}
```

Особенности:

- Очень полезна при тестировании контроллеров и сервисов, которые требуют аутентификации.
- Экономит время на настройку тестов, имитируя сессию пользователя.

#### 10. **`@EnableResourceServer`**

Аннотация `@EnableResourceServer` используется для настройки защиты ресурсов в приложении, часто в контексте OAuth2.

```java
@EnableResourceServer
public class ResourceServerConfig extends ResourceServerConfigurerAdapter {
    // Настройка ресурсного сервера для OAuth2
}
```

Целесообразность: Используется для защиты API и ресурсов в приложениях, которые работают с OAuth2.