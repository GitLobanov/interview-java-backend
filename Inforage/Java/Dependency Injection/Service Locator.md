
### Пример

```java
public class ServiceLocator {
    private static Map<String, Object> services = new HashMap<>();

    public static void addService(String name, Object service) {
        services.put(name, service);
    }

    public static Object getService(String name) {
        return services.get(name);
    }
}
```

Use:

```java
public class ExampleService {
    public void execute() {
        System.out.println("Service is running...");
    }
}

public class Main {
    public static void main(String[] args) {
        ExampleService exampleService = new ExampleService();
        ServiceLocator.addService("exampleService", exampleService);

        ExampleService service = (ExampleService) ServiceLocator.getService("exampleService");
        service.execute(); // Выведет: Service is running...
    }
}
```