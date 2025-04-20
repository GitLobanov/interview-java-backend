### Основные принципы реактивного программирования:

1. **Асинхронность**: Реактивные системы не блокируют потоки для ожидания завершения операций. Вместо этого, они реагируют на события по мере их возникновения, используя подписки и обработчики событий.
2. **Неблокирующий ввод-вывод (I/O)**: В реактивных системах чтение и запись данных не блокируют поток выполнения, что позволяет обрабатывать большие объемы данных и делать множество запросов одновременно.
3. **Поток данных (Stream)**: Данные обрабатываются как непрерывный поток, который можно фильтровать, трансформировать или обрабатывать другими операциями.
4. **Backpressure (Обратное давление)**: Этот механизм позволяет управлять нагрузкой на систему, контролируя количество данных, отправляемых от источника к потребителю. Если потребитель не справляется с потоком данных, он может замедлить его или приостановить.

В **Spring** реактивное программирование реализуется через **Spring WebFlux** — реактивный стек фреймворка, созданный для работы с асинхронными запросами и потоками данных.

#### Основные компоненты Spring WebFlux:

- **WebClient**: Реактивный клиент для выполнения асинхронных HTTP-запросов.
- **Mono** и **Flux**: Это основные реактивные типы данных в Spring WebFlux.
    - **Mono**: Представляет результат одной асинхронной операции (или пустой результат).
    - **Flux**: Представляет поток данных, который может содержать 0, 1 или больше элементов.

```java
public class ReactiveExample {
    private WebClient webClient = WebClient.create("https://api.example.com");

    public Mono<String> fetchSingleData() {
        return webClient.get()
                .uri("/data/{id}", 1)
                .retrieve()
                .bodyToMono(String.class); // Возвращает один элемент (Mono)
    }

    public Flux<String> fetchMultipleData() {
        return webClient.get()
                .uri("/data/all")
                .retrieve()
                .bodyToFlux(String.class); // Возвращает поток данных (Flux)
    }

    public static void main(String[] args) {
        ReactiveExample example = new ReactiveExample();
        
        // Обработка одного элемента
        example.fetchSingleData().subscribe(data -> {
            System.out.println("Received single data: " + data);
        });

        // Обработка потока данных
        example.fetchMultipleData().subscribe(data -> {
            System.out.println("Received data: " + data);
        });
    }
}
```

В этом примере:

- `fetchSingleData()` возвращает **Mono**, так как ожидается только один элемент данных.
- `fetchMultipleData()` возвращает **Flux**, так как ожидается поток данных.

### Реактивное программирование на примере Nihongo Nexus и трекинга времени

#### 1. **Пример для Nihongo Nexus**:

Предположим, что у нас есть функциональность, которая обновляет результаты пользователя в реальном времени, например, в таблице лидеров. Когда пользователь завершает задание, его результат автоматически передается на сервер и обновляется.

```java
public class LeaderboardUpdater {
    private WebClient webClient = WebClient.create("https://nihongo-nexus.com/api");

    public Mono<Void> updateLeaderboard(String userId, int newScore) {
        return webClient.post()
                .uri("/leaderboard/update")
                .bodyValue(new LeaderboardUpdate(userId, newScore))
                .retrieve()
                .bodyToMono(Void.class); // Возвращает Mono<Void>, так как мы не ожидаем ответа с телом
    }
    
    public static void main(String[] args) {
        LeaderboardUpdater updater = new LeaderboardUpdater();
        
        // Асинхронное обновление таблицы лидеров
        updater.updateLeaderboard("user123", 1500).subscribe(
            success -> System.out.println("Leaderboard updated"),
            error -> System.err.println("Error updating leaderboard: " + error)
        );
    }
}
```

#### 2. **Пример для трекинга времени**:

Представим, что приложение для трекинга задач позволяет пользователям получать в реальном времени обновления о прогрессе задач, которые ведутся их командой. Используя реактивное программирование, мы можем получать данные потоком.

```java
public class TaskProgressTracker {
    private WebClient webClient = WebClient.create("https://time-tracker.com/api");

    public Flux<String> getTeamProgress(String teamId) {
        return webClient.get()
                .uri("/teams/{teamId}/progress", teamId)
                .retrieve()
                .bodyToFlux(String.class); // Получаем поток данных
    }

    public static void main(String[] args) {
        TaskProgressTracker tracker = new TaskProgressTracker();
        
        // Реактивное отслеживание прогресса команды
        tracker.getTeamProgress("team123").subscribe(
            progressUpdate -> System.out.println("Progress update: " + progressUpdate),
            error -> System.err.println("Error retrieving progress: " + error)
        );
    }
}
```

### Реактивные библиотеки и инструменты:

1. **Project Reactor**: Это основная реактивная библиотека, используемая в Spring WebFlux. Она предоставляет API для работы с **Mono** и **Flux**.
2. **RxJava**: Альтернатива Project Reactor, популярная в мире реактивного программирования.
3. **RSocket**: Это двунаправленный протокол для реактивных потоков, который можно использовать в связке с Spring.

### Когда использовать реактивное программирование?

- Если у вас есть приложение с высокой нагрузкой и множеством одновременных операций (например, микросервисы, работающие с тысячами клиентов).
- Когда важна асинхронная обработка данных, например, в системах реального времени или потоковой обработки.
- Если приложение работает с неблокирующими операциями ввода-вывода (например, с базами данных, внешними сервисами или очередями сообщений).