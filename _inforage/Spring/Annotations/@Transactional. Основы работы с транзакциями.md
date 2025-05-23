#### 1. **Что такое транзакция?**

Транзакция — это последовательность операций, которая должна выполняться как единое целое. В контексте баз данных это обычно набор операций чтения и записи, которые должны либо полностью завершиться, либо полностью откатиться. Основные свойства транзакций известны как ACID:

- **Atomicity (Атомарность)**: Транзакция выполняется полностью или не выполняется вовсе.
- **Consistency (Согласованность)**: Транзакция переводит систему из одного согласованного состояния в другое.
- **Isolation (Изолированность)**: Результаты транзакции видны только после ее завершения.
- **Durability (Долговечность)**: После завершения транзакции ее изменения сохраняются и не могут быть потеряны.

#### 2. **Аннотация `@Transactional`**

Аннотация `@Transactional` в Spring используется для управления транзакциями. Она может быть применена к методам или классам. Основные функции и параметры аннотации:

- **`@Transactional` на методе**: Применяется к конкретному методу и означает, что транзакция начнется при вызове метода и закончится при его завершении.
- **`@Transactional` на классе**: Применяется ко всем методам класса (если метод не переопределен).

```java
@Service
public class UserService {

    @Transactional
    public void createUser(User user) {
        // Операции, которые должны быть выполнены в одной транзакции
        userRepository.save(user);
        // Дополнительные операции
    }
}
```

#### 3. **Конфигурация и управление транзакциями**

Spring поддерживает несколько способов управления транзакциями:

- **Программное управление**: Используется через `TransactionTemplate` и `PlatformTransactionManager`. Подходит для сложных сценариев, когда требуется полный контроль над транзакциями.

```java
@Service
public class UserService {

    private final TransactionTemplate transactionTemplate;

    public UserService(PlatformTransactionManager transactionManager) {
        this.transactionTemplate = new TransactionTemplate(transactionManager);
    }

    public void createUser(User user) {
        transactionTemplate.execute(status -> {
            userRepository.save(user);
            // Дополнительные операции
            return null;
        });
    }
}
```

- **Объявленное управление**: Использует аннотацию `@Transactional` для автоматического управления транзакциями. Подходит для большинства случаев.

#### 4. **Параметры аннотации `@Transactional`**

- **`propagation`**: Определяет, как транзакции должны обрабатываться, если уже существует активная транзакция. Примеры значений: `REQUIRED`, `REQUIRES_NEW`, `MANDATORY`.
    
- **`isolation`**: Определяет уровень изоляции транзакции. Примеры значений: `READ_COMMITTED`, `SERIALIZABLE`.
    
- **`timeout`**: Определяет время в секундах, через которое транзакция будет автоматически завершена.
    
- **`readOnly`**: Указывает, что транзакция предназначена только для чтения. Это может оптимизировать производительность.
    
- **`rollbackFor` и `noRollbackFor`**: Определяют, какие исключения должны приводить к откату транзакции.

```java
@Service
public class UserService {

    @Transactional(propagation = Propagation.REQUIRED, isolation = Isolation.READ_COMMITTED, timeout = 30, readOnly = true)
    public void getUserDetails(Long userId) {
        // Операции чтения данных
    }
}
```

#### 5. **Работа с исключениями и откат транзакций**

По умолчанию транзакция откатывается при возникновении непроверяемых исключений (RuntimeException). Проверяемые исключения не приводят к откату, если только это явно не указано в параметрах аннотации.

```java
@Transactional(rollbackFor = CustomCheckedException.class)
public void process() throws CustomCheckedException {
    try {
        // Операции, которые могут вызвать CustomCheckedException
    } catch (CustomCheckedException e) {
        // Логирование и перекидывание исключения для отката
        throw e;
    }
}
```


#### 6. **Примеры использования в проектах**

##### Проект Nihongo Nexus:

В проекте Nihongo Nexus можно использовать транзакции для обеспечения целостности данных при выполнении сложных операций, таких как обновление словаря или взаимодействие с пользовательскими записями.

```java
@Service
public class VocabularyService {

    @Transactional
    public void addWordToVocabulary(Word word) {
        // Добавление нового слова
        wordRepository.save(word);
        // Обновление связанных данных
        updateRelatedData(word);
    }

    private void updateRelatedData(Word word) {
        // Обновление данных
    }
}
```

### Почему важно использовать транзакции?

1. **Целостность данных**
    - **Атомарность операций**: Транзакции обеспечивают, что несколько связанных операций выполняются как единое целое. Если одна операция не удалась, все изменения откатываются. Это предотвращает частичное выполнение, что может привести к непоследовательности данных.
2. **Изоляция**
    - **Конкуренция и согласованность**: Транзакции позволяют контролировать уровень изоляции между операциями, что помогает избежать проблем, таких как "грязное чтение" или "фантомные записи", когда параллельные транзакции влияют друг на друга.
3. **Управление ошибками**
    - **Автоматический откат**: Если происходит исключение, транзакция может быть автоматически откатана. Это избавляет от необходимости вручную управлять состоянием и откатом в случае ошибок.
4. **Повышение надежности**
    - **Консистентное состояние**: При правильном использовании транзакций данные остаются консистентными даже в случае сбоя системы или ошибок приложения.