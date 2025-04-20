### Фильтры (Filters)

**Фильтры** являются частью Java Servlet API и применяются на уровне сервлета. Они обрабатываются до того, как запрос достигнет сервлета, и после того, как ответ был отправлен клиенту.

#### Основные характеристики:

- **Цель**: Фильтры используются для выполнения обработки до или после запроса и ответа. Они могут выполнять задачи, такие как аутентификация, логирование, и модификация запросов или ответов.
    
- **API**: Фильтры реализуются через интерфейс `javax.servlet.Filter`. В этом интерфейсе определены методы `doFilter()`, `init()`, и `destroy()`.
    
- **Регистрация**: Фильтры регистрируются в веб-конфигурации через `web.xml` или программно через `FilterRegistrationBean` в конфигурации Spring Boot.
    
- **Область применения**: Фильтры работают на уровне сервлета и обрабатываются контейнером сервлетов (например, Tomcat) до того, как запрос достигнет сервлета или контроллера.
    
- **Работа с запросами и ответами**: Фильтры могут изменять запрос и ответ, но не имеют доступа к обработке бизнес-логики Spring.

#### Пример создания фильтра:

```java
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // Инициализация фильтра
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        // Логирование, модификация запросов/ответов
        System.out.println("Request URL: " + httpRequest.getRequestURL());

        // Передача запроса дальше по цепочке
        chain.doFilter(request, response);
    }

    @Override
    public void destroy() {
        // Очистка ресурсов
    }
}
```

### Интерцепторы (Interceptors)

**Интерцепторы** являются частью Spring Framework и применяются на уровне Spring MVC. Они обрабатываются в контексте обработки запросов Spring MVC и позволяют взаимодействовать с запросами и ответами, управляемыми Spring.
#### Основные характеристики:

- **Цель**: Интерцепторы используются для выполнения кода до и после обработки запроса в рамках Spring MVC. Они позволяют перехватывать запросы и ответы на уровне Spring MVC.
    
- **API**: Интерцепторы реализуются через интерфейс `org.springframework.web.servlet.HandlerInterceptor`. В этом интерфейсе определены методы `preHandle()`, `postHandle()`, и `afterCompletion()`.
    
- **Регистрация**: Интерцепторы регистрируются через конфигурацию Spring MVC, обычно в `WebMvcConfigurer` или через XML конфигурацию с использованием `<mvc:interceptors>`.
    
- **Область применения**: Интерцепторы работают на уровне Spring MVC и перехватывают запросы после того, как они обработаны контроллером, но до того, как результат будет отправлен клиенту.
    
- **Работа с запросами и ответами**: Интерцепторы могут выполнять дополнительные задачи, такие как проверка авторизации, логирование, или настройка атрибутов модели, но они работают в рамках Spring MVC и имеют доступ к обработке бизнес-логики.
#### Пример создания интерцептора:

```java
import org.springframework.web.servlet.HandlerInterceptor;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        // Логирование, аутентификация, модификация запросов
        System.out.println("Pre-handle: " + request.getRequestURL());
        return true; // Продолжить выполнение контроллера
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
                           ModelAndView modelAndView) throws Exception {
        // Модификация модели и представления
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
            throws Exception {
        // Очистка ресурсов, логирование
        System.out.println("After completion");
    }
}
```


### Основные отличия:

1. **Уровень обработки:**
    - Фильтры работают на уровне сервлета и обрабатываются до и после выполнения сервлета.
    - Интерцепторы работают на уровне Spring MVC и обрабатываются до и после выполнения контроллера.
2. **Гибкость и интеграция:**
    - Фильтры предоставляют более низкоуровневый доступ и могут быть использованы для глобальных задач (например, фильтрация запросов, сжатие ответов).
    - Интерцепторы интегрированы с механизмом Spring MVC и предоставляют более высокоуровневые возможности для обработки запросов и взаимодействия с контроллерами.
3. **Конфигурация:**
    - Фильтры конфигурируются в `web.xml` или с помощью аннотаций `@WebFilter`.
    - Интерцепторы конфигурируются через `WebMvcConfigurer` или XML-конфигурацию Spring MVC.

![[../../_res/Pasted image 20240922164959.jpg]]

Фильтры перехватывают запросы до того, как они достигнут `DispatcherServlet`, что делает их идеальными для задач с грубой детализацией, таких как:

- Аутентификация
- Логирование и аудит
- Сжатие изображений и данных
- Любая функциональность, которую мы хотим отделить от Spring MVC

С другой стороны, `HandlerInterceptor` перехватывает запросы между `DispatcherServlet` и нашими контроллерами. Это выполняется в рамках Spring MVC, предоставляя доступ к объектам `Handler` и `ModelAndView`. Это снижает дублирование и позволяет реализовать более тонкую функциональность, такую как:

- Обработка сквозных задач, таких как логирование приложения
- Детализированные проверки авторизации
- Манипулирование контекстом Spring или моделью

# Источники

- https://www.baeldung.com/spring-mvc-handlerinterceptor-vs-filter