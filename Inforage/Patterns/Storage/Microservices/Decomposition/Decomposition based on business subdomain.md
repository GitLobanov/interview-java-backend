- Начинаем с выделениям под-доменов при помощи формы Domain Driven Design ([[../../../../Microservices/Storage/DDD]]).

Чтобы избежать появления God Classes, можно использовать альтернативный шаблон разложения на микросервисы — разбиение по поддоменам. Он основан на концепциях предметно-ориентированного проектирования (Domain-Driven Design, DDD).

DDD разбивает всю модель предметной области (домен) на поддомены. У каждого поддомена своя модель данных, область действия которой принято называть ограниченным контекстом (Bounded Context). Каждый микросервис будет разрабатываться внутри этого ограниченного контекста. Основная задача при использовании DDD-подхода — подобрать поддомены и границы между ними так, чтобы они были максимально независимы друг от друга.

Если вернуться к примеру с интернет-магазином, то все, что связано с заказами, можно рассматривать в рамках поддомена «Заказы» (Orders Subdomain) и именно внутри этого поддомена создавать микросервис по управлению заказами (Orders Service). Таким образом, можно сократить число микросервисов по сравнению с декомпозицией на основе бизнес-возможностей. В нашем сильно упрощенном примере четыре микросервиса были преобразованы в один.

![[Pasted image 20241215093511.png]]

![[Pasted image 20240928163950.png]]

Domain Driven Design (DDD) provides a suite of tools and methodologies to reason about the underlying domain at hand, to reflect the best available understanding of the domain in the software design and to evolve the software design as understanding of the domain grows and changes. DDD Strategic Patterns guide the creation of a [Context Map](https://www.infoq.com/articles/ddd-contextmapping) which can form the foundation of your microservices decomposition.

From this, we can consider decomposition by Domain to be guiding the system design according to an analysis of the processes and information flows .

The pro of this approach is that it can lead to a system design that closely models the reality of what is happening (or needs to happen). Hopefully the business structure already aligns with this - but where it doesn't, it can reveal inefficiencies in the existing business organisational structure.

If you have influence over the organisational structure, this can be a foundation for utilising the Inverse Conway Maneuver and to allow you to evolve the software, the dev teams and the business units to achieve alignment.

If you don't, you may end up introducing friction points where the system design becomes misaligned with the business capabilities.

Define services corresponding to Domain-Driven Design (DDD) subdomains. DDD refers to the application’s problem space - the business - as the domain. A domain is consists of multiple subdomains. Each subdomain corresponds to a different part of the business.

Subdomains can be classified as follows:

- Core - key differentiator for the business and the most valuable part of the application
- Supporting - related to what the business does but not a differentiator. These can be implemented in-house or outsourced.
- Generic - not specific to the business and are ideally implemented using off the shelf softwa

Decomposing based on business subdomain which is extension to the business capabilty one. In Domain-Driven Design (DDD) pertains to the problem space of the application, specifically the business domain. A domain comprises various sub domains, each representing a distinct aspect of the business.

Each of the subdomain service also developed through the Agile methodlogy, each service can be indpendently developed a cross-functional team that is committed to delivering business capabilities.  
Like identifying business capabilities, identifying sub domains is quite challenging task.