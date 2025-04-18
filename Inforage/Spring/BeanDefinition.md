

## Примеры

### Пример 1: Динамическое создание бинов на основе конфигурации

#### Задача

Предположим, в приложении нужно динамически создавать бины в зависимости от внешней конфигурации (например, на основе данных, полученных из базы данных или конфигурационных файлов). При этом состав этих бинов может меняться во время работы приложения.

#### Решение с использованием `BeanDefinition`

Для решения такой задачи можно динамически регистрировать бины с помощью `BeanDefinition`, основываясь на данных из конфигурации.

```java
@Configuration
public class DynamicBeanConfig implements BeanFactoryPostProcessor {

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        BeanDefinitionRegistry registry = (BeanDefinitionRegistry) beanFactory;

        // Получаем конфигурацию из внешнего источника
        String[] serviceNames = getExternalServiceNames(); // Динамически получаем имена сервисов

        for (String serviceName : serviceNames) {
            GenericBeanDefinition beanDefinition = new GenericBeanDefinition();
            beanDefinition.setBeanClass(DynamicService.class);
            beanDefinition.setScope(BeanDefinition.SCOPE_SINGLETON);
            beanDefinition.getPropertyValues().add("serviceName", serviceName);

            // Регистрируем бин в контейнере
            registry.registerBeanDefinition(serviceName + "Service", beanDefinition);
        }
    }

    private String[] getExternalServiceNames() {
        // Пример: получение имен сервисов из внешнего источника
        return new String[]{"EmailService", "SMSService"};
    }
}
```

#### Объяснение:

- Мы используем `BeanFactoryPostProcessor` для получения конфигурации перед созданием бинов и динамически регистрируем бины с разными именами.
- Для каждого сервиса создается отдельный бин `DynamicService`, где каждому бину задается уникальное свойство `serviceName`.

#### Целесообразность:

Это решение целесообразно, когда состав бинов может меняться динамически, и их невозможно предсказать на этапе компиляции. Такой подход позволяет гибко адаптировать приложение под разные конфигурации без необходимости менять исходный код.

### Пример 2: Адаптация бинов на основе среды выполнения (профиля)

#### Задача

Требуется создавать разные реализации одного и того же бина в зависимости от среды выполнения приложения (например, для тестовой и продуктивной среды).

#### Решение с использованием `BeanDefinition`

Можно использовать `BeanDefinition` для регистрации разных реализаций бинов на основе активного профиля среды выполнения.

```java
@Configuration
public class ProfileBasedBeanConfig implements BeanFactoryPostProcessor {

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        BeanDefinitionRegistry registry = (BeanDefinitionRegistry) beanFactory;

        String activeProfile = getActiveProfile(); // Получаем активный профиль

        GenericBeanDefinition beanDefinition = new GenericBeanDefinition();
        
        if ("prod".equals(activeProfile)) {
            beanDefinition.setBeanClass(ProductionService.class);
        } else {
            beanDefinition.setBeanClass(DevelopmentService.class);
        }

        // Регистрируем бин с именем 'myService'
        registry.registerBeanDefinition("myService", beanDefinition);
    }

    private String getActiveProfile() {
        // Пример получения активного профиля
        return "prod"; // Например, можно брать из System.getenv() или свойств среды
    }
}
```


#### Объяснение:

- В зависимости от активного профиля (`prod` или `dev`), мы регистрируем либо `ProductionService`, либо `DevelopmentService`.
- Такое решение дает гибкость в управлении окружением и реализации бинов в зависимости от условий работы приложения.

#### Целесообразность:

Это решение полезно в проектах, где нужно подбирать бины в зависимости от профиля, что облегчает тестирование и деплой в разных средах (например, `production`, `development`, `staging`).

### Пример 3: Программное создание сложной цепочки обязанностей

#### Задача

В приложении требуется создать цепочку обязанностей, где бины будут взаимодействовать друг с другом. Порядок создания и связывания этих бинов должен быть гибким и настраиваемым.

#### Решение с использованием `BeanDefinition`

Можно использовать `BeanDefinition` для программного создания цепочки обязанностей (например, для обработки запросов).

```java
@Configuration
public class ChainOfResponsibilityConfig implements BeanFactoryPostProcessor {

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        BeanDefinitionRegistry registry = (BeanDefinitionRegistry) beanFactory;

        // Создаем BeanDefinition для каждого звена цепочки
        GenericBeanDefinition handler1Def = new GenericBeanDefinition();
        handler1Def.setBeanClass(RequestHandler.class);
        handler1Def.getPropertyValues().add("handlerName", "Handler1");

        GenericBeanDefinition handler2Def = new GenericBeanDefinition();
        handler2Def.setBeanClass(RequestHandler.class);
        handler2Def.getPropertyValues().add("handlerName", "Handler2");

        GenericBeanDefinition handler3Def = new GenericBeanDefinition();
        handler3Def.setBeanClass(RequestHandler.class);
        handler3Def.getPropertyValues().add("handlerName", "Handler3");

        // Регистрация бинов в контексте
        registry.registerBeanDefinition("handler1", handler1Def);
        registry.registerBeanDefinition("handler2", handler2Def);
        registry.registerBeanDefinition("handler3", handler3Def);

        // Связывание цепочки
        handler1Def.getPropertyValues().add("nextHandler", beanFactory.getBean("handler2"));
        handler2Def.getPropertyValues().add("nextHandler", beanFactory.getBean("handler3"));
    }
}

public class RequestHandler {
    private String handlerName;
    private RequestHandler nextHandler;

    public void setHandlerName(String handlerName) {
        this.handlerName = handlerName;
    }

    public void setNextHandler(RequestHandler nextHandler) {
        this.nextHandler = nextHandler;
    }

    public void handleRequest() {
        System.out.println("Handling request by: " + handlerName);
        if (nextHandler != null) {
            nextHandler.handleRequest();
        }
    }
}
```

#### Объяснение:

- Здесь создается цепочка обработчиков запросов с помощью `BeanDefinition`.
- Каждый бин представляет собой обработчик, который может передавать запрос следующему обработчику в цепочке.
- Связывание бинов происходит в момент создания, используя метод `getPropertyValues()` для установки зависимостей.

#### Целесообразность:

Такой подход полезен, если цепочка обязанностей может изменяться в зависимости от конфигурации или условий выполнения программы. Программное создание цепочек позволяет гибко управлять их структурой и легко настраивать новые звенья.
