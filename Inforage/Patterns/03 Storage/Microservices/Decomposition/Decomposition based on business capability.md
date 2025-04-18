- Разделяем сервисы по бизнес-функциям

> Декомпозиция происходит на основе возможностей бизнеса (то, что бизнес делает), а не его структуры или данных. Мы фокусируемся на том, какие основные функциональные возможности необходимы для бизнеса (например, бухгалтерия, управление запасами, клиентское обслуживание).

> Грань проходит по бизнес-функциям, которые представляют собой что бизнес может делать. В результате появляются модули, которые могут быть общими для всех контекстов, но решают одну бизнес-задачу

![[3_1_ca7e40a78e.webp]]

При разбиении по бизнес-возможностям могут появиться так называемые «божественные классы» (God Classes) — сущности, которые будут общими для нескольких микросервисов. Как правило, их очень сложно разделить.

Например, в приложении для интернет-магазина такой сущностью может стать заказ. В приведенном выше примере он используется сразу в нескольких сервисах: создание заказов (Orders Creation), доставка заказов (Orders Delivery), оповещения о заказах (Orders Alerts), предзаказы (Preorders).

![[Pasted image 20240928163929.png]]

With an understanding of these concepts, we can consider decomposition by Business Capability to be guiding the system design according to the way the business is structured. This echos Conway's law.

The pro of this approach is it helps to ensure alignment between development teams and business structural units. The con is it may bake business inefficiencies that arose before an automated system was considered, into the design of your system.

In a microservice architecture, we will decompose the services based on business capabilities.  
A business capability is a fundamental concept in business architecture modelling, referring to the specific activities that a business undertakes to create values architecture modelling.

By decomposing based on business capability, we will achieve the cohesive, loosely coupled, and stable servcies. These services can be developed, tested, and deployed independently. Each of the decomposed services adheres to the following design principles [Single Responsiblity Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle "Single Responsiblity Principle") and [Common Closure Principle (CCP)](http://https//blog.devgenius.io/common-closure-principle-the-story-of-an-evolving-architecture-6919b452c8db "Common Closure Principle (CCP)").

Nowadays most of the enterprise applications developed through the Agile methodology, where each service is independently developed by a cross-functional team that is committed to delivering business capabilities

One of the challenges tasks is to identity the business capabilities for the service. It is neccessary to utilize the bounded context based on the organizational strucuture and high-level domain model.

