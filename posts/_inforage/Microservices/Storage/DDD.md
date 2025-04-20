>DDD помогает создать архитектуру системы, основанную на реальных бизнес-процессах и понятиях. 

- Domain: бизнес-область, которую вы моделируете. Это основная часть системы, представляющая реальные объекты и процессы.
- Value Object: объект без уникального идентификатора, который определяется своими атрибутами. Пример: дата или адрес.
- Aggregate: группа связанных сущностей и объектов-значений, которые образуют целостную единицу. Например, заказ может быть агрегатом, который включает в себя заказанные товары (сущности) и стоимость доставки (объект-значение).
- Также Entity, Factory, Service, Repository


- Подход, подразумевающий построение сложного приложения, **центром** которого **выступает** объектно-ориентировная **модель доменов**.
- **Режим домена**, схватывает **знание о** домене в форме, которая может быть использована для **решения проблем** внутри **домена**
- Определяет **словарный запас**, которая использует команда
- Содержит два концепта, которые невероятно полезны, когда подразумевается микросервесная архитектура: **под-домены и связанные контексты**.

DDD is quite different than the traditional approach to enterprise modeling, which creates a single model for the entire enterprise. In such a model there would be, for example, a single definition of each business entity, such as customer, order, and so on. The problem with this kind of modeling is that getting different parts of an organization to agree on a single model is a monumental task. Also, it means that from the perspective of a given part of the organization, the model is overly complex for their needs. Moreover, the domain model can be confusing because different parts of the organization might use either the same term for different concepts or different terms for the same concept. DDD avoids these problems by defining multiple domain models, each with an explicit scope.

**DDD defines a separate domain model for each subdomain**. A subdomain is a part of the _domain_, DDD’s term for the application’s problem space. Subdomains are identified using the same approach as identifying business capabilities: analyze the business and identify the different areas of expertise. The end result is very likely to be subdomains that are similar to the business capabilities. The examples of subdomains in FTGO include Order taking, Order management, Kitchen management, Delivery, and Financials. As you can see, these subdomains are very similar to the business capabilities described earlier.

DDD calls the scope of a domain model a _bounded context_. A bounded context includes the code artifacts that implement the model. When using the microservice architecture, each bounded context is a service or possibly a set of services. We can create a microservice architecture by applying DDD and defining a service for each subdomain. Figure shows how the subdomains map to services, each with its own domain model.

![[../../../_res/Pasted image 20241009130650.jpg]]

DDD and the microservice architecture are in almost perfect alignment. The DDD concept of subdomains and bounded contexts maps nicely to services within a microservice architecture. Also, the microservice architecture’s concept of autonomous teams owning services is completely aligned with the DDD’s concept of each domain model being owned and developed by a single team. Even better, as I describe later in this section, the concept of a subdomain with its own domain model is a great way to eliminate god classes and thereby make decomposition easier.

Decompose by subdomain and Decompose by business capability are the two main patterns for defining an application’s microservice architecture. There are, however, some useful guidelines for decomposition that have their roots in object-oriented design. Let’s take a look at them.

# Ресурсы

- Chapter 2.2.3. DEFINING SERVICES BY APPLYING THE DECOMPOSE BY SUB-DOMAIN PATTERN [[Microservice-Patterns.pdf]] 
- [Domain-Driven Design: чистая архитектура снизу доверху](https://habr.com/ru/companies/sberbank/articles/781612/)
- 


