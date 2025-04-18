**DI-контейнер** (контейнер внедрения зависимостей) — это объект или фреймворк, который управляет созданием, конфигурацией и жизненным циклом зависимостей в приложении. DI-контейнер упрощает внедрение зависимостей ([[Dependency Injection]]), обеспечивая инверсию управления (Inversion of Control, IoC), так как контейнер берет на себя ответственность за создание и предоставление зависимостей объектам.

### Этапы работы DI-контейнера:

1. **Регистрация зависимостей**  
    Контейнер должен знать, какие классы или интерфейсы он должен использовать для разрешения зависимостей. Это этап, на котором зависимости регистрируются в контейнере. Обычно регистрируются интерфейсы и их реализации, которые должны быть предоставлены.
    
    **Пример**:
    
```java
container.register(TaskRepository.class, InMemoryTaskRepository.class);
```
    
2. **Резолвинг (разрешение) зависимостей**  
    На этом этапе контейнер определяет, какие зависимости нужны объектам, и автоматически их предоставляет. Когда запрашивается создание объекта, DI-контейнер анализирует его конструктор или сеттеры, чтобы определить, какие зависимости требуются, и предоставляет их.
    
    **Пример**:
    
```java
TaskService taskService = container.resolve(TaskService.class); 
```
    
3. **Создание и инъекция зависимостей**  
    DI-контейнер создает экземпляры зависимостей, а затем инъектирует их в нужные места. Это может происходить через конструктор, через методы (сеттеры) или поля. В этот момент зависимости передаются объекту, который их запрашивает.
    
    **Пример**:

```java
public class TaskService {
    private TaskRepository taskRepository;

    public TaskService(TaskRepository taskRepository) {
        this.taskRepository = taskRepository; // DI-контейнер передает зависимость сюда
    }
}
```
    
4. **Управление жизненным циклом объектов**  
    Контейнер также может управлять жизненным циклом объектов. Например, он может создавать объекты один раз (Singleton) или каждый раз новые (Prototype). DI-контейнер может поддерживать разные **scopes** (области видимости), например, синглтон для всего приложения или сессию для запроса в веб-приложениях.
    
    **Пример настройки жизненного цикла**:
    
```java
 container.register(TaskRepository.class, InMemoryTaskRepository.class)
         .lifecycle(Scope.SINGLETON);
```
### Реализация простого DI-контейнера

Пример простой реализации DI-контейнера для приложения по трекингу времени:

```java
public class DIContainer {
    private Map<Class<?>, Class<?>> dependencies = new HashMap<>();

    // Регистрация зависимостей
    public <T> void register(Class<T> baseClass, Class<? extends T> implClass) {
        dependencies.put(baseClass, implClass);
    }

    // Разрешение зависимостей и создание объекта
    public <T> T resolve(Class<T> baseClass) throws Exception {
        Class<?> implClass = dependencies.get(baseClass);

        if (implClass == null) {
            throw new IllegalArgumentException("No implementation found for: " + baseClass);
        }

        // Инъекция через конструктор
        return (T) implClass.getConstructor().newInstance();
    }
}
```

Использование DI-контейнера:

```java
public class InMemoryTaskRepository implements TaskRepository {
    // Реализация репозитория задач
}

public class TaskService {
    private TaskRepository taskRepository;

    public TaskService(TaskRepository taskRepository) {
        this.taskRepository = taskRepository;
    }

    public void trackTask(String taskId) {
        taskRepository.saveTask(taskId);
    }
}

public class Main {
    public static void main(String[] args) throws Exception {
        DIContainer container = new DIContainer();

        // Регистрация зависимостей
        container.register(TaskRepository.class, InMemoryTaskRepository.class);

        // Разрешение зависимостей и создание объекта
        TaskService taskService = container.resolve(TaskService.class);

        taskService.trackTask("123");
    }
}
```

### Основные этапы работы DI-контейнера:

1. **Регистрация зависимостей**: определяются связи между интерфейсами и их реализациями.
2. **Разрешение зависимостей**: контейнер понимает, какие зависимости требуются объектам.
3. **Создание и инъекция**: контейнер создает объект с уже переданными ему зависимостями.
4. **Управление жизненным циклом объектов**: контейнер может управлять созданием и удалением объектов в зависимости от области их видимости (singleton, prototype и т.д.).

Таким образом, DI-контейнер автоматизирует создание и предоставление зависимостей, что делает код более гибким, тестируемым и легко поддерживаемым.

### Список источников

- [МФТИ Core Java 2020 Лекция 13](https://www.youtube.com/watch?v=szI5sza6Wug)
