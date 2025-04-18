---
Определение: Определяют, как и когда создаются и управляются экземпляры бинов. Spring предоставляет несколько типов областей видимости для бинов, каждая из которых определяет жизненный цикл и область действия бина в контексте приложения.
Примечание:
---
- Singleton - один на весь контекст. Будет создан при поднятии контекста.
- Prototype - создается каждый раз, когда мы запрашиваем бин из контекста. После создания не
- управляется IoC контейнером.
- Request - один бин на жизненный цикл http запроса. При поступлении запроса DispatcherServlet
- будет создаваться бин, а после того, как запрос будет обработан - бин будет удаляться.
- Session - бин будет создаваться при создании юзер-сессии и удаляться вместе с ней
- Application - один бин на Servlet Container
- Websocket - один бин на жизненный цикл Websocket
### 1. Singleton (По умолчанию)

- **Описание**: Один единственный экземпляр бина создается и используется для всего контейнера Spring. Этот бин создается один раз при запуске контейнера и остается в контейнере до его закрытия.
- **Использование**: `@Scope("singleton")` или по умолчанию без явного указания области видимости.

```java
@Component
@Scope("singleton")
public class MySingletonBean {
    // Реализация
}
```

### 2. Prototype

- **Описание**: Каждый раз, когда запрашивается бин, создается новый экземпляр. Это означает, что при каждом вызове `getBean()` из контейнера Spring будет возвращаться новый объект.
- **Использование**: `@Scope("prototype")`

```java
@Component
@Scope("prototype")
public class MyPrototypeBean {
    // Реализация
}
```

### 3. Request

- **Описание**: Веб-специфичная область видимости. Один экземпляр бина создается для каждого HTTP-запроса. Это означает, что бин будет создан заново для каждого запроса и уничтожен после завершения запроса.
- **Использование**: `@Scope("request")`

Request - один бин на жизненный цикл http запроса. При поступлении запроса DispatcherServlet будет создаваться бин, а после того, как запрос будет обработан - бин будет удаляться.

 Only valid in the context of a web-aware Spring `ApplicationContext`.
 
```xml
<bean id="loginAction" class="com.something.LoginAction" scope="request"/>
```

The Spring container creates a new instance of the `LoginAction` bean by using the `loginAction` bean definition for each and every HTTP request. That is, the `loginAction` bean is scoped at the HTTP request level. You can change the internal state of the instance that is created as much as you want, because other instances created from the same `loginAction` bean definition do not see these changes in state. They are particular to an individual request. When the request completes processing, the bean that is scoped to the request is discarded.

When using annotation-driven components or Java configuration, the `@RequestScope` annotation can be used to assign a component to the `request` scope. The following example shows how to do so:

```java
@RequestScope
@Component
public class LoginAction {
	// ...
}
```

### 4. Session

- **Описание**: Веб-специфичная область видимости. Один экземпляр бина создается для каждой HTTP-сессии. Это означает, что бин будет сохраняться в рамках одной сессии и будет создан заново для каждой новой сессии.
- **Использование**: `@Scope("session")`

Session - бин будет создаваться при создании юзер-сессии и удаляться вместе с ней

```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session"/>
```

The Spring container creates a new instance of the `UserPreferences` bean by using the `userPreferences` bean definition for the lifetime of a single HTTP `Session`. In other words, the `userPreferences` bean is effectively scoped at the HTTP `Session` level. As with request-scoped beans, you can change the internal state of the instance that is created as much as you want, knowing that other HTTP `Session` instances that are also using instances created from the same `userPreferences` bean definition do not see these changes in state, because they are particular to an individual HTTP `Session`. When the HTTP `Session` is eventually discarded, the bean that is scoped to that particular HTTP `Session` is also discarded.

When using annotation-driven components or Java configuration, you can use the `@SessionScope` annotation to assign a component to the `session` scope.

```java
@SessionScope
@Component
public class UserPreferences {
	// ...
}
```


### 5. Application

- **Описание**: Веб-специфичная область видимости. Один экземпляр бина создается на уровне всего веб-приложения. Этот бин живет на протяжении всего времени работы приложения и не создается заново для каждого запроса или сессии.
- **Использование**: `@Scope("application")`

Application - один бин на Servlet Container


```xml
<bean id="appPreferences" class="com.something.AppPreferences" scope="application"/>
```

The Spring container creates a new instance of the `AppPreferences` bean by using the `appPreferences` bean definition once for the entire web application. That is, the `appPreferences` bean is scoped at the `ServletContext` level and stored as a regular `ServletContext` attribute. This is somewhat similar to a Spring singleton bean but differs in two important ways: It is a singleton per `ServletContext`, not per Spring `ApplicationContext` (for which there may be several in any given web application), and it is actually exposed and therefore visible as a `ServletContext` attribute.

When using annotation-driven components or Java configuration, you can use the `@ApplicationScope` annotation to assign a component to the `application` scope. The following example shows how to do so:

```java
@ApplicationScope
@Component
public class AppPreferences {
	// ...
}
```

### 6. WebSocket

- **Описание**: Веб-специфичная область видимости. Один экземпляр бина создается для каждого WebSocket-соединения.
- **Использование**: `@Scope("websocket")`

Websocket - один бин на жизненный цикл Websocket

### Примеры использования:

- **Singleton**: Используется для сервисов, которые не требуют состояния между запросами и могут быть общими для всего приложения, например, сервисы для доступа к данным или реализации бизнес-логики.
    
- **Prototype**: Используется для создания объектов с состоянием, где каждый запрос требует нового экземпляра, например, объекты, которые хранят данные, специфичные для каждого запроса.
    
- **Request**: Полезен для хранения данных в рамках одного HTTP-запроса, например, для передачи данных между компонентами в рамках одного запроса.
    
- **Session**: Полезен для хранения данных в рамках одного HTTP-сеанса пользователя, например, пользовательские настройки или сессии.
    
- **Application**: Используется для хранения данных, которые должны быть доступны во всей области веб-приложения, например, общие ресурсы или конфигурации.
    
- **WebSocket**: Полезен для хранения данных, специфичных для одного WebSocket-соединения, например, состояние общения между клиентом и сервером через WebSocket.

# Ресурсы

- [Bean Scopes](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html)
- 