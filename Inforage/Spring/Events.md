---
Определение: Позволяет компонентам и сервисам общаться друг с другом через события, которые можно публиковать и слушать. Это позволяет строить более гибкую и модульную архитектуру, где различные части приложения могут взаимодействовать без жесткой связи друг с другом.
Примечание:
---
### A Simple Application Event

Let’s create **a simple event class** — just a placeholder to store the event data.
In this case, the event class holds a String message:

```java
public class CustomSpringEvent extends ApplicationEvent {
    private String message;

    public CustomSpringEvent(Object source, String message) {
        super(source);
        this.message = message;
    }
    public String getMessage() {
        return message;
    }
}
```

### A Publisher

Now let’s create **a publisher of that event.** The publisher constructs the event object and publishes it to anyone who’s listening.

To publish the event, the publisher can simply inject the _ApplicationEventPublisher_ and use the _publishEvent()_ API:

```java
@Component
public class CustomSpringEventPublisher {
    @Autowired
    private ApplicationEventPublisher applicationEventPublisher;

    public void publishCustomEvent(final String message) {
        System.out.println("Publishing custom event. ");
        CustomSpringEvent customSpringEvent = new CustomSpringEvent(this, message);
        applicationEventPublisher.publishEvent(customSpringEvent);
    }
}
```

Alternatively, the publisher class can implement the _ApplicationEventPublisherAware_ interface, and this will also inject the event publisher on the application startup. Usually, it’s simpler to just inject the publisher with _@Autowire_.

As of Spring Framework 4.2, the _ApplicationEventPublisher_ interface provides a new overload for the _[publishEvent(Object event)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationEventPublisher.html#publishEvent-java.lang.Object-)_ method that accepts any object as the event. **Therefore, Spring events no longer need to extend the _ApplicationEvent_ class.**

### A Listener

Finally, let’s create the listener.
The only requirement for the listener is to be a bean and implement _ApplicationListener_ interface:

```java
@Component
public class CustomSpringEventListener implements ApplicationListener<CustomSpringEvent> {
    @Override
    public void onApplicationEvent(CustomSpringEvent event) {
        System.out.println("Received spring custom event - " + event.getMessage());
    }
}
```

Notice how our custom listener is parametrized with the generic type of custom event, which makes the _onApplicationEvent()_ method type-safe. This also avoids having to check if the object is an instance of a specific event class and casting it.

And, as already discussed (by default **Spring events are synchronous**), the _doStuffAndPublishAnEvent()_ method blocks until all listeners finish processing the event.

## Creating Asynchronous Events

In some cases, publishing events synchronously isn’t really what we’re looking for — **we may need async handling of our events.**

We can turn that on in the configuration by creating an _ApplicationEventMulticaster_ bean with an executor.

For our purposes here, _SimpleAsyncTaskExecutor_ works well:

```java
@Configuration
public class AsynchronousSpringEventsConfig {
    @Bean(name = "applicationEventMulticaster")
    public ApplicationEventMulticaster simpleApplicationEventMulticaster() {
        SimpleApplicationEventMulticaster eventMulticaster =
          new SimpleApplicationEventMulticaster();
        
        eventMulticaster.setTaskExecutor(new SimpleAsyncTaskExecutor());
        return eventMulticaster;
    }
}
```

The event, the publisher and the listener implementations remain the same as before, but now **the listener will asynchronously deal with the event in a separate thread.**
## Existing Framework Events

Spring itself publishes a variety of events out of the box. For example, the _ApplicationContext_ will fire various framework events: _ContextRefreshedEvent_, _ContextStartedEvent_, _RequestHandledEvent_ etc.

These events provide application developers an option to hook into the life cycle of the application and the context and add in their own custom logic where needed.

Here’s a quick example of a listener listening for context refreshes:

```java
public class ContextRefreshedListener 
  implements ApplicationListener<ContextRefreshedEvent> {
    @Override
    public void onApplicationEvent(ContextRefreshedEvent cse) {
        System.out.println("Handling context re-freshed event. ");
    }
}
```

## Annotation-Driven Event Listener

Starting with Spring 4.2, an event listener is not required to be a bean implementing the _ApplicationListener_ interface — it can be registered on any _public_ method of a managed bean via the _@EventListener_ annotation:

```java
@Component
public class AnnotationDrivenEventListener {
    @EventListener
    public void handleContextStart(ContextStartedEvent cse) {
        System.out.println("Handling context started event.");
    }
}
```

As before, the method signature declares the event type it consumes.

By default, the listener is invoked synchronously. However, we can easily make it asynchronous by adding an _@Async_ annotation. We just need to remember to [enable _Async_ support](https://www.baeldung.com/spring-async#enable-async-support) in the application.

## Generics Support

It is also possible to dispatch events with generics information in the event type.

### A Generic Application Event

**Let’s create a generic event type.**
In our example, the event class holds any content and a _success_ status indicator:

```java
public class GenericSpringEvent<T> {
    private T what;
    protected boolean success;

    public GenericSpringEvent(T what, boolean success) {
        this.what = what;
        this.success = success;
    }
    // ... standard getters
}
```

Notice the difference between _GenericSpringEvent_ and _CustomSpringEvent_. We now have the flexibility to publish any arbitrary event and it’s not required to extend from _ApplicationEvent_ anymore.

### A Listener

Now let’s create **a listener of that event.**
We could define the listener by implementing the _ApplicationListener_ interface like before:

```java
@Component
public class GenericSpringEventListener 
  implements ApplicationListener<GenericSpringEvent<String>> {
    @Override
    public void onApplicationEvent(@NonNull GenericSpringEvent<String> event) {
        System.out.println("Received spring generic event - " + event.getWhat());
    }
}
```

But this definition unfortunately requires us to inherit _GenericSpringEvent_ from the _ApplicationEvent_ class. So for this tutorial, let’s make use of an annotation-driven event listener discussed previously.

It is also possible to **make the event listener conditional** by defining a boolean SpEL expression on the _@EventListener_ annotation.

In this case, the event handler will only be invoked for a successful _GenericSpringEvent_ of _String_:

```java
@Component
public class AnnotationDrivenEventListener {
    @EventListener(condition = "#event.success")
    public void handleSuccessful(GenericSpringEvent<String> event) {
        System.out.println("Handling generic event (conditional).");
    }
}
```

The [Spring Expression Language (SpEL)](https://www.baeldung.com/spring-expression-language) is a powerful expression language that’s covered in detail in another tutorial.
### A Publisher

The event publisher is similar to the one described [above](https://www.baeldung.com/spring-events#publisher). But due to type erasure, we need to publish an event that resolves the generics parameter we would filter on, for example, _class GenericStringSpringEvent extends GenericSpringEvent<\String>_.

Also, there’s **an alternative way of publishing events.** If we return a non-null value from a method annotated with _@EventListener_ as the result, Spring Framework will send that result as a new event for us. Moreover, we can publish multiple new events by returning them in a collection as the result of event processing.

## Transaction-Bound Events

This section is about using the _@TransactionalEventListener_ annotation. To learn more about transaction management, check out [Transactions With Spring and JPA](https://www.baeldung.com/transaction-configuration-with-jpa-and-spring).

Since Spring 4.2, the framework provides a new _@TransactionalEventListener_ annotation, which is an extension of _@EventListener_, that allows binding the listener of an event to a phase of the transaction.

Binding is possible to the following transaction phases:

- _AFTER_COMMIT_ (default) is used to fire the event if the transaction has **completed successfully.**
- _AFTER_ROLLBACK_ – if the transaction has **rolled back**
- _AFTER_COMPLETION_ – if the transaction has **completed** (an alias for _AFTER_COMMIT_ and _AFTER_ROLLBACK_)
- _BEFORE_COMMIT_ is used to fire the event right **before** transaction **commit.**

Here’s a quick example of a transactional event listener:

```java
@TransactionalEventListener(phase = TransactionPhase.BEFORE_COMMIT)
public void handleCustom(CustomSpringEvent event) {
    System.out.println("Handling event inside a transaction BEFORE COMMIT.");
}
```

This listener will be invoked only if there’s a transaction in which the event producer is running and it’s about to be committed.

And if no transaction is running, the event isn’t sent at all unless we override this by setting _fallbackExecution_ attribute to _true_.


## Полный пример

```java
// 1. Определение события
import org.springframework.context.ApplicationEvent;

public class UserRegisteredEvent extends ApplicationEvent {
    private String username;

    public UserRegisteredEvent(Object source, String username) {
        super(source);
        this.username = username;
    }

    public String getUsername() {
        return username;
    }
}

// 2. Публикация события
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.stereotype.Component;

@Component
public class UserService {
    private final ApplicationEventPublisher publisher;

    public UserService(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }

    public void registerUser(String username) {
        // Регистрация пользователя
        // ...

        // Публикация события
        publisher.publishEvent(new UserRegisteredEvent(this, username));
    }
}

// 3. Слушатель события
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;

@Component
public class UserEventListener {

    @EventListener
    public void handleUserRegisteredEvent(UserRegisteredEvent event) {
        String username = event.getUsername();
        // Логика обработки события
        System.out.println("New user registered: " + username);
    }
}

// 4. Основной класс для запуска приложения
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

```