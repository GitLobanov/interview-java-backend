## QA

###### Какие компоненты знаешь?

Spring Cloud  
Spring Cloud Commons  
Spring Cloud OpenFeign  
Spring Cloud Circuit Breaker  
Spring Cloud Security  
Spring Cloud Schema Registry  
Spring Cloud Sleuth  
Spring Cloud Contract  
Spring Cloud Cluster

Spring Cloud Kubernetes  
Spring Vault  
Spring Cloud Connectors

Spring Cloud Function  
Spring Cloud Stream  
Spring Cloud Stream Applications  
Spring Cloud Task  
Spring Cloud Task App Starters

Spring Cloud Consul

Spring Session Core  
**Session-репозитории**  
Spring Session Data Redis  
Spring Session MongoDB  
Spring Session JDBC  
Spring Session Hazelcast  
Spring Session for Apache Geode

**Сервис с ролью Gateway**  
Spring Cloud Gateway  
**Сервисы с ролью Config Server**  
Spring Cloud Config  
Spring Cloud Vault  
Spring Cloud Bus  
**Сервисы с ролью Service Discovery**  
Spring Cloud Netflix (Eureka)

###### Чем можно заменить Discovery Service Eureka используя кубер

Service Mesh Istio, Consul

###### Что такое Spring Cloud и для чего он используется?

Spring Cloud — это набор инструментов и библиотек для упрощения разработки и управления микросервисными архитектурами. Он предоставляет готовые решения для типичных задач распределенных систем, таких как конфигурация, обнаружение сервисов, балансировка нагрузки и устойчивость к сбоям. Например, вместо того чтобы писать свой сервис обнаружения, можно использовать Spring Cloud Netflix Eureka.

###### Какие основные проблемы решает Spring Cloud?

- **Централизованная конфигурация** (единое хранилище настроек для всех сервисов).
- **Обнаружение и регистрация сервисов** (как сервисы находят друг друга).
- **Балансировка нагрузки** (распределение запросов между экземплярами сервиса).
- **Обработка ошибок и устойчивость** (например, Circuit Breaker через Hystrix).
- **Распределенное логирование и трассировка** (отслеживание запроса через несколько сервисов).
- **Маршрутизация и API Gateway** (управление внешними запросами к микросервисам).

###### Перечислите несколько проектов, входящих в состав Spring Cloud.

- **Spring Cloud Config** — управление конфигурациями.
- **Spring Cloud Netflix** (Eureka, Hystrix, Zuul) — сервис обнаружения, Circuit Breaker, маршрутизация.
- **Spring Cloud Gateway** — современный API Gateway.
- **Spring Cloud Sleuth/Zipkin** — трассировка и логирование.
- **Spring Cloud Stream** — интеграция с брокерами сообщений (Kafka, RabbitMQ).
- **Spring Cloud Bus** — рассылка событий (например, обновление конфигураций).

###### Как Spring Cloud упрощает работу с конфигурационными файлами в микросервисной архитектуре?

Через **Spring Cloud Config Server**, который хранит конфигурации в централизованном хранилище (Git, файловая система). Сервисы запрашивают настройки при старте, а обновления можно применять динамически (например, через `/actuator/refresh` или Spring Cloud Bus). Это избавляет от дублирования настроек и упрощает управление разными средами (dev, prod).

###### Чем отличается Spring Cloud Netflix от Spring Cloud?

Spring Cloud Netflix — это подпроект Spring Cloud, реализующий инструменты от Netflix (Eureka, Hystrix, Zuul). Однако многие компоненты (например, Hystrix, Zuul) теперь в maintenance mode, и Spring Cloud предлагает альтернативы:
- Вместо Zuul — **Spring Cloud Gateway**.
- Вместо Ribbon — **Spring Cloud LoadBalancer**.
- Вместо Hystrix — **Resilience4J**.

###### Как в Spring Cloud реализована балансировка нагрузки?

- **Клиентская балансировка**: через интеграцию с **Spring Cloud LoadBalancer** (ранее Ribbon). Сервис при запросе к другому сервису получает список его экземпляров от Eureka и выбирает один (например, round-robin).

 ###### Опишите процесс настройки централизованного логирования в Spring Cloud.

- Добавить **Spring Cloud Sleuth** для генерации уникальных `traceId` и `spanId` для каждого запроса.
    
- Интегрировать с **Zipkin** для визуализации трейсов:
    - Запустить Zipkin-сервер.
    - Добавить зависимость `spring-cloud-starter-zipkin`.
        
- Настроить отправку логов в ELK-стек (Elasticsearch, Logstash, Kibana):
    - Использовать Logback или Log4j2 для форматирования логов с `traceId`.
    - Направить логи в Logstash, который сохраняет их в Elasticsearch.

###### Каким образом Spring Cloud Stream обрабатывает сообщения в микросервисной архитектуре?

Spring Cloud Stream абстрагирует работу с брокерами сообщений (Kafka, RabbitMQ) через Binder.

###### Как в Spring Cloud реализован паттерн цепочки обязанностей (Chain of Responsibility) для обработки запросов?

Реализуется через **фильтры в Spring Cloud Gateway** (или Zuul). Каждый фильтр выполняет свою задачу (аутентификация, логирование, модификация запроса) и передает запрос следующему фильтру в цепочке.

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: auth_route
          uri: lb://auth-service
          predicates:
            - Path=/auth/**
          filters:
            - AddRequestHeader=X-User-Id, 123  # Фильтр 1
            - LoggingFilter                    # Фильтр 2
```

## Resources

- [Обзор Spring-компонентов. Часть 2 – Spring Cloud](https://habr.com/ru/articles/674882/)
- 