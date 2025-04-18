---
Определение: событие в Spring, которое публикуется в момент, когда контекст приложения (ApplicationContext) был обновлён. Оно является частью механизма событий Spring и используется для уведомления о том, что контекст приложения был перезагружен, что может быть полезно для выполнения некоторых действий после инициализации или изменения контекста.
Примечание:
---
### Основные аспекты `ContextRefreshEvent`

1. **Что такое `ContextRefreshEvent`?**
    
    `ContextRefreshEvent` — это [[Events|Event]], который публикуется в Spring при обновлении контекста приложения. Это происходит, например, после вызова метода `refresh()` на `ConfigurableApplicationContext`, что обычно происходит при запуске приложения или при явной перезагрузке контекста.
    
2. **Когда возникает `ContextRefreshEvent`?**
    
    Событие `ContextRefreshEvent` возникает в следующих случаях:
    
    - При запуске приложения, когда контекст загружается и инициализируется.
    - При явном вызове `refresh()` на `ConfigurableApplicationContext`, который перезагружает контекст и его бины.
3. **Использование `ContextRefreshEvent`**
    
    Вы можете создать слушатель для обработки `ContextRefreshEvent`, чтобы выполнять определенные действия после того, как контекст приложения будет обновлен.

```java
@Component
public class ContextRefreshEventListener implements ApplicationListener<ContextRefreshEvent> {

    @Override
    public void onApplicationEvent(ContextRefreshEvent event) {
        // Выполнение действий при обновлении контекста
        System.out.println("Контекст приложения был обновлен!");
    }
}
```

4. **Использование аннотации `@EventListener`**

Вы также можете использовать аннотацию `@EventListener` для обработки `ContextRefreshEvent`.

```java
@Component
public class ContextRefreshEventHandler {

    @EventListener
    public void handleContextRefresh(ContextRefreshEvent event) {
        // Выполнение действий при обновлении контекста
        System.out.println("Контекст приложения был обновлен!");
    }
}
```