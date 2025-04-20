---
Определение: 
Примечание:
---



### Создание 

#### Using Application Context

```java
@Component
public class UseEmployeePrototype {
    private ApplicationContext applicationContext;

    @Autowired
    public UseEmployeePrototype(ApplicationContext applicationContext) {
        this.applicationContext = applicationContext;
    }

    public void usePrototype() {
        Employee employee = (Employee) applicationContext.getBean("Employee", "sachin");
        employee.printName();
    }
}
```

As we see here, we’re **tightly coupling the bean creation** to the _ApplicationContext_. Thus, the approach may be impacted if we change our bean implementation.

#### Using the Factory Method

Spring provides the ObjectFactory<\T> interface to produce on-demand objects of the given type.
Let’s create an EmployeeFactory for our Employee bean using ObjectFactory:

```java
public class EmployeeBeanUsingObjectFactory {
    @Autowired
    private ObjectFactory employeeObjectFactory;

    public Employee getEmployee() {
        return employeeObjectFactory.getObject();
    }
}
```

Here, with every call to _getEmployee()_, Spring will return a new _Employee_ object.

#### Using [[@Lookup]]

Alternatively, method injection using the [[@Lookup]] annotation can solve the problem. Any method that we inject with a _@Lookup_ annotation will be overridden by the Spring container, which will then return the named bean for that method.

```java
@Component
public class EmployeeBeanUsingLookUp {
    @Lookup
    public Employee getEmployee(String arg) {
        return null;
    }
}
```

A method annotated with @_Lookup_, such as _getEmployee_(), will be overridden by Spring. As a result, the bean is registered in the application context. A new _Employee_ instance is returned each time we call the _getEmployee_() method.

#### Using _Function_

Spring provides another option, _Function_, for creating prototype beans at run time. We can also apply the arguments to newly created prototype bean instances.

First, let’s create a component using _Function_ where the name field will be added to the instance:

```java
@Component
public class EmployeeBeanUsingFunction {
    @Autowired
    private Function<String, Employee> beanFactory;

    public Employee getEmployee(String name) {
        Employee employee = beanFactory.apply(name);
        return employee;
    }
}
```

Further, now, let’s add a new _beanFactory()_ in our bean configuration:

```java
@Configuration
public class EmployeeConfig {
    @Bean
    @Scope(value = "prototype")
    public Employee getEmployee(String name) {
        return new Employee(name);
    }

    @Bean
    public Function<String, Employee> beanFactory() {
        return name -> getEmployee(name);
    }
}
```

#### Using _ObjectProvider_

Spring provides ObjectProvider<\T> , which is an extension of the existing _ObjectFactory_ interface.
Let’s inject the _ObjectProvider_ and get the _Employee_ object using _ObjectProvider_:

```java
public class EmployeeBeanUsingObjectProvider {
    @Autowired
    private org.springframework.beans.factory.ObjectProvider objectProvider;

    public Employee getEmployee(String name) {
        Employee employee = objectProvider.getObject(name);
        return employee;
    }
}
```


