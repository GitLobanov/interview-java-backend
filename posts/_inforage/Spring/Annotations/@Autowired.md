---
Определение: Используется для автоматического внедрения зависимостей в компоненты, такие как сервисы, контроллеры и репозитории. Она позволяет Spring автоматически находить и предоставлять необходимые бины для классов, что упрощает конфигурацию и управление зависимостями в приложении.
Примечание: 
Пакет: org.springframework.beans.factory.annotation
---
● Используется для автоматического внедрения зависимостей
● Тянет бины по типу (в отличии от @Resource, который тянет по имени)
● Можно ставить над конструктором, полем или сеттером. А можно вообще не ставить, если в классе один конструктор.

```java
конструктор

@Service
public class MyService {

    private final AnotherService anotherService;

    @Autowired
    public MyService(AnotherService anotherService) {
        this.anotherService = anotherService;
    }

}

сеттер

@Service
public class MyService {

    private AnotherService anotherService;

    @Autowired
    public void setAnotherService(AnotherService anotherService) {
        this.anotherService = anotherService;
    }

}

поле

@Service
public class MyService {

    @Autowired
    private AnotherService anotherService;

}
```

● Есть атрибут required, который позволяет настроить обязательность бина.

```java
@Service
public class MyService {

    @Autowired(required = false)
    private OptionalDependency optionalDependency;

}
```

В этом случае, если `optionalDependency` не найдена в контексте, Spring не выбросит исключение.