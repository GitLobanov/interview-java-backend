---
Определение: Использует имя бина для внедрения зависимости. Если имя бина совпадает с именем поля или метода, Spring будет искать бин с этим именем в контексте и внедрять его.
Примечание:
---
```java
@Service
public class MyService {

    @Resource(name = "myDependency")
    private Dependency dependency;

    // методы
}
```