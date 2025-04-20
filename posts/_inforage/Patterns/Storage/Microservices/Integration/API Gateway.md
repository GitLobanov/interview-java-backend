> Единая точка входа для приложения с минимальной логикой.  Клиенты обращаются в единый API Gateway и после этого gateway уже сам решает, в какой сервис им нужно пойти. 

Наиболее очевидный способ обращения к микросервисам — прямое обращение от клиента к сервису. И его вполне можно применять в небольших проектах. Однако в приложениях корпоративного масштаба с большим числом микросервисов рекомендуется использовать шаблон API Gateway.

Этот паттерн основан на применении шлюза, который находится между клиентским приложением и микросервисами, обеспечивая единую точку входа для клиента.

В зависимости от конкретной цели использования паттерна иногда выделяют следующие его разновидности:

- **Gateway Routing.** Шлюз используется как обратный Proxy, перенаправляющий запросы клиента на соответствующий сервис.
- **Gateway Aggregation.** Шлюз используется для разветвления клиентского запроса на несколько микросервисов и возвращения агрегированных ответов клиенту.
- **Gateway Offloading.** Шлюз решает сквозные задачи, которые являются общими для сервисов: аутентификация, авторизация, SSL, ведение журналов и так далее.

Применение паттерна сокращает число вызовов, обеспечивает независимость клиента от протоколов, используемых в сервисах: REST, AMQP, gRPC и так далее, обеспечивает централизованное управление сквозной функциональностью. Однако шлюз может стать единой точкой отказа, требует тщательного мониторинга и при отсутствии масштабирования бывает узким местом системы.

![[../../../../../_res/Pasted image 20241215101035.png]]

![[../../../../../_res/Pasted image 20240930101002.png]]


Nowadays, the majority of the applications we are currently developing are designed for various user interface (UI) interfaces, such as desktop, mobile, and tablet versions. Consequently, users can access these applications from any of these UI interfaces. Depending on the UI interface being used, the response from the underlying services may vary, as desktop applications may require more fields compared to their mobile counterparts.

In order to achieve this functionality, it is necessary to implement the API Gateway pattern. This pattern allows us to configure throttling, rate-limiting, and handle requests with an appropriate routing mechanism. As a result, the API Gateway becomes the single point of entry for all requests.

In fact, it is possible to create separate API Gateway services for web applications, mobile applications, and third-party applications.

# Паттерны

Паттерн "душитель" (Throttling) используется для ограничения количества запросов или событий, которые система может обрабатывать за определённый период времени. Он предотвращает перегрузку системы и может быть полезен при работе с внешними сервисами или при защите от пиковых нагрузок.

Пример: [[API Gateway]] может ограничить количество запросов, которые клиент может отправить на сервер в течение минуты, чтобы избежать перегрузки.




https://habr.com/ru/articles/562498/