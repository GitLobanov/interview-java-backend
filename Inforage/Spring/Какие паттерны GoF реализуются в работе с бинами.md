В работе с бинами в Spring Framework реализуются многие паттерны, предложенные в книге _Design Patterns: Elements of Reusable Object-Oriented Software_ (Gang of Four, [[../Patterns/GoF]]). 
### 1. Singleton (Одиночка)

**Описание**: Паттерн Singleton гарантирует, что у класса будет только один экземпляр, и предоставляет глобальную точку доступа к этому экземпляру.

**Применение в Spring**: По умолчанию все бины в Spring создаются как синглтоны. Это означает, что для каждого бина создаётся один экземпляр на контейнер. Это достигается через механизм управления жизненным циклом бинов в контексте Spring.

### 2. Factory Method (Фабричный метод)

**Описание**: Паттерн Factory Method определяет интерфейс для создания объекта, но позволяет подклассам изменять тип создаваемых объектов.

**Применение в Spring**: В Spring Factory Method реализуется через аннотации `@Bean` или при использовании интерфейса `FactoryBean`.

```java
@Configuration
public class AppConfig {

    @Bean
    public LessonService lessonService() {
        return new LessonServiceImpl();
    }
}
```

### 3. Prototype (Прототип)

**Описание**: Паттерн Prototype позволяет создавать новые объекты на основе существующего объекта через клонирование.

**Применение в Spring**: Бин можно объявить как `prototype`, что означает создание нового экземпляра бина при каждом запросе.

```java
@Service
@Scope("prototype")
public class TaskService {
    public void executeTask() {
        // Выполнение задачи
    }
}
```

### 4. Dependency Injection (Внедрение зависимости)

**Описание**: Этот паттерн обеспечивает внедрение зависимостей в объекты, снижая их связанность и делая классы более гибкими.

**Применение в Spring**: Dependency Injection (DI) является ключевым паттерном Spring. Зависимости внедряются либо через конструктор, либо через методы сеттеров, либо через поля.

```java
@Service
public class LessonService {
    private final LessonRepository lessonRepository;

    @Autowired
    public LessonService(LessonRepository lessonRepository) {
        this.lessonRepository = lessonRepository;
    }

    public void getLesson() {
        lessonRepository.findLesson();
    }
}
```

### 5. Proxy (Заместитель)

**Описание**: Паттерн Proxy предоставляет суррогат для другого объекта, контролируя доступ к нему.

**Применение в Spring**: В Spring паттерн Proxy часто используется в контексте AOP (аспектно-ориентированного программирования) для добавления дополнительной функциональности к бину без изменения его исходного кода. Прокси-объекты могут быть созданы динамически.
\
```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.LessonService.*(..))")
    public void logBefore() {
        System.out.println("Логирование перед вызовом метода");
    }
}
```

### 6. Adapter (Адаптер)

**Описание**: Паттерн Adapter позволяет объектам с несовместимыми интерфейсами работать вместе, преобразуя интерфейс одного класса в интерфейс другого.

**Применение в Spring**: В Spring адаптеры используются, когда необходимо преобразовать один интерфейс в другой. Например, в REST-контроллерах Spring использует адаптеры для обработки HTTP-запросов.

```java
@RestController
public class LessonController {

    @GetMapping("/lesson/{id}")
    public ResponseEntity<Lesson> getLesson(@PathVariable Long id) {
        // Логика получения урока
        return ResponseEntity.ok(new Lesson(id, "Lesson 1"));
    }
}
```

### 7. Template Method (Шаблонный метод)

**Описание**: Паттерн Template Method задает основу алгоритма в методе и позволяет подклассам переопределять отдельные шаги алгоритма без изменения его структуры.

**Применение в Spring**: В Spring этот паттерн часто используется в абстрактных классах или интерфейсах для создания последовательности действий, которую можно изменять в подклассах.

```java
public abstract class AbstractLessonProcessor {

    public void processLesson() {
        validateLesson();
        saveLesson();
        notifyStudent();
    }

    protected abstract void validateLesson();
    protected abstract void saveLesson();
    protected abstract void notifyStudent();
}
```

### 8. Observer (Наблюдатель)

**Описание**: Паттерн Observer определяет зависимость "один ко многим" между объектами, при которой изменение состояния одного объекта уведомляет все зависимые от него объекты.

**Применение в Spring**: В Spring наблюдатели реализуются через события (events). Можно опубликовать событие и подписаться на него для выполнения определённых действий при возникновении события.

```java
@Component
public class LessonCreatedEventPublisher {

    @Autowired
    private ApplicationEventPublisher applicationEventPublisher;

    public void publishEvent(Lesson lesson) {
        applicationEventPublisher.publishEvent(new LessonCreatedEvent(this, lesson));
    }
}

public class LessonCreatedEvent extends ApplicationEvent {
    private final Lesson lesson;

    public LessonCreatedEvent(Object source, Lesson lesson) {
        super(source);
        this.lesson = lesson;
    }

    public Lesson getLesson() {
        return lesson;
    }
}
```


### 9. Strategy (Стратегия)

**Описание**: Паттерн Strategy позволяет изменять алгоритм независимо от клиента, который использует этот алгоритм.

**Применение в Spring**: В Spring можно использовать стратегию для выбора разных способов выполнения задач. Это особенно полезно при внедрении логики, которая может изменяться в зависимости от условий.

```java
public interface LessonStrategy {
    void execute();
}

@Component
public class OnlineLessonStrategy implements LessonStrategy {
    public void execute() {
        System.out.println("Онлайн урок");
    }
}

@Component
public class OfflineLessonStrategy implements LessonStrategy {
    public void execute() {
        System.out.println("Оффлайн урок");
    }
}
```

### 10. Chain of Responsibility (Цепочка обязанностей)

**Описание**: Паттерн Chain of Responsibility передаёт запрос вдоль цепочки обработчиков, где каждый обработчик решает, может ли он обработать запрос или передать его дальше.

**Применение в Spring**: В Spring этот паттерн можно реализовать для обработки запросов в фильтрах или AOP-аспектах.

```java
@Component
public class LessonHandlerChain {

    private final List<LessonHandler> handlers;

    @Autowired
    public LessonHandlerChain(List<LessonHandler> handlers) {
        this.handlers = handlers;
    }

    public void handle(Lesson lesson) {
        for (LessonHandler handler : handlers) {
            if (handler.handle(lesson)) {
                break;
            }
        }
    }
}
```



