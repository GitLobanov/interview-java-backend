---
Определение: Центральный компонент в архитектуре Spring MVC, который управляет процессом обработки HTTP-запросов и формированием HTTP-ответов. Он играет ключевую роль в маршрутизации запросов, связывании данных и координации всех частей Spring MVC приложения.
Примечание:
---
#### 1. Что такое DispatcherServlet?

`DispatcherServlet` — это сервлет, который обрабатывает все входящие HTTP-запросы в приложении Spring MVC. Он выступает в качестве фасада, принимая запросы, определяя, какой контроллер их обработает, и управляя процессом генерации ответа. `DispatcherServlet` инициализируется при запуске приложения и работает как основной маршрутизатор запросов.

#### 2. Инициализация DispatcherServlet

`DispatcherServlet` настраивается в конфигурации веб-приложения. В классическом подходе с использованием `web.xml` это может выглядеть так:

```xml
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

В современном подходе с использованием аннотаций и Java-конфигурации это делается через аннотацию `@ServletComponentScan` и класс конфигурации.

#### 3. Функции DispatcherServlet

1. **Маршрутизация Запросов**: `DispatcherServlet` использует `HandlerMapping` для определения, какой контроллер должен обработать запрос. `HandlerMapping` сопоставляет URL запросов с методами контроллеров.
    
2. **Валидация и Связывание**: `DispatcherServlet` передает параметры запроса в контроллер. Если используются аннотации `@ModelAttribute`, `@RequestBody` или `@RequestPart`, `DispatcherServlet` связывает параметры с соответствующими объектами и выполняет валидацию с использованием аннотаций `@Valid` или `@Validated`.
    
3. **Вызов Контроллера**: После определения подходящего контроллера `DispatcherServlet` вызывает его метод, передавая необходимые данные и параметры.
    
4. **Обработка Результата**: Метод контроллера возвращает результат, который может быть объектом модели, строкой представления, JSON или другим типом данных. `DispatcherServlet` обрабатывает этот результат, передавая его `ViewResolver` для рендеринга представления, если это необходимо.
    
5. **Рендеринг Представления**: Если метод контроллера возвращает имя представления, `DispatcherServlet` использует `ViewResolver` для поиска и рендеринга соответствующего представления, такого как JSP, Thymeleaf, FreeMarker и др.
    
6. **Формирование Ответа**: `DispatcherServlet` формирует окончательный HTTP-ответ и отправляет его клиенту через сервлет-контейнер.

#### 4. Конфигурация DispatcherServlet

Конфигурация `DispatcherServlet` включает настройку различных компонентов, таких как `HandlerMapping`, `HandlerAdapter`, `ViewResolver`, и других.

- **HandlerMapping**: Определяет, как URL-запросы сопоставляются с методами контроллеров.
- **HandlerAdapter**: Управляет вызовом методов контроллеров и поддерживает различные типы обработчиков.
- **ViewResolver**: Находит и рендерит представления, возвращаемые контроллерами.

Пример конфигурации через Java-класс:

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
    }

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }

    // Дополнительные настройки, такие как конвертеры и форматтеры
}
```

#### 5. Преимущества DispatcherServlet

- **Централизованное Управление**: `DispatcherServlet` обеспечивает централизованное управление запросами, упрощая настройку и управление запросами в приложении.
- **Гибкость**: `DispatcherServlet` может быть настроен для работы с различными компонентами и конфигурациями, что делает его гибким инструментом для разработки веб-приложений.
- **Интеграция с Spring**: Он интегрируется с другими частями Spring, такими как безопасность, транзакции и валидация, что упрощает создание сложных веб-приложений.

## Process example

```java
public class DispatcherServlet {

    // Псевдокод метода для обработки HTTP-запроса
    public void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {

        // 1. Получить подходящий обработчик (Controller) через Handler Mapping
        HandlerExecutionChain handlerChain = handlerMapping.getHandler(request);

        if (handlerChain == null) {
            // Если обработчик не найден, отправить ошибку 404
            response.sendError(HttpServletResponse.SC_NOT_FOUND, "Handler not found");
            return;
        }

        // 2. Получить сам контроллер (Handler)
        Object handler = handlerChain.getHandler();

        // 3. Вызвать метод контроллера для обработки запроса
        // (Здесь используем адаптер для вызова разных типов контроллеров)
        HandlerAdapter handlerAdapter = getHandlerAdapter(handler);
        ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);

        if (modelAndView == null) {
            // Если контроллер сам отправил ответ, ничего больше делать не нужно
            return;
        }

        // 4. Определить представление (View) с помощью ViewResolver
        String viewName = modelAndView.getViewName();
        View view = viewResolver.resolveViewName(viewName, request.getLocale());

        if (view == null) {
            // Если представление не найдено, отправить ошибку 404
            response.sendError(HttpServletResponse.SC_NOT_FOUND, "View not found for name: " + viewName);
            return;
        }

        // 5. Передать данные модели и рендерить представление (View)
        // Вызовем метод render() у представления для отображения ответа
        view.render(modelAndView.getModel(), request, response);
    }

    // Метод для получения адаптера контроллера (HandlerAdapter)
    private HandlerAdapter getHandlerAdapter(Object handler) throws Exception {
        // Поиск подходящего адаптера для данного контроллера
        for (HandlerAdapter adapter : handlerAdapters) {
            if (adapter.supports(handler)) {
                return adapter;
            }
        }
        throw new Exception("No HandlerAdapter found for handler: " + handler);
    }
}
```


1. **Handler Mapping**:
    
    - `HandlerExecutionChain handlerChain = handlerMapping.getHandler(request);`
    - Метод `getHandler()` находит нужный контроллер (Handler) для данного запроса. Если контроллер не найден, возвращаем ошибку 404.
2. **Handler (Controller)**:
    
    - `HandlerAdapter handlerAdapter = getHandlerAdapter(handler);`
    - Контроллеры могут быть разных типов (например, классы с аннотациями или классы, реализующие определенные интерфейсы). Поэтому используется **HandlerAdapter**, чтобы абстрагироваться от конкретного типа контроллера.
    - Метод `handle()` вызывается для обработки запроса, и он возвращает **ModelAndView** (содержит данные модели и имя представления).
3. **ViewResolver**:
    
    - `View view = viewResolver.resolveViewName(viewName, request.getLocale());`
    - После того как контроллер возвращает имя представления, **ViewResolver** определяет, какое представление (шаблон или JSP) будет использоваться для рендеринга.
4. **View**:
    
    - `view.render(modelAndView.getModel(), request, response);`
    - Представление (View) берет модель данных из **ModelAndView** и рендерит (генерирует) HTML-ответ, который отправляется клиенту.

### Важные моменты:

- **HandlerAdapter**: Позволяет обработчику (Controller) быть разным по типу. Например, это может быть класс, реализующий интерфейс контроллера, или класс, аннотированный как контроллер.
- **ModelAndView**: Это объект, который возвращает контроллер. Он содержит данные для представления (модель) и имя представления.
- **ViewResolver**: Определяет, какое представление (например, JSP или шаблон) будет использовано для рендеринга.
