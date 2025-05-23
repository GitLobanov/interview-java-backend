- Используется для уменьшения кол-ва походов в другие сервисы за данными
- На стороне сервиса-команды будет БД на чтение, которое будет обновляться через events со стороны сервиса-провайдера данных

- Плюсы:
	- Уменьшение кол-ва межсервисных взаимодействий
- Минусы:
	- усложняет сервис-провайдер, так как ему теперь необходимо передавать данные при каждом изменении сущности
	- сервису-команде нужно поддерживать структуру модели данных сервиса-провайдера
	- при большом кол-ве изменений нужно будет передавать большое количество events

![[../../../../../_res/Pasted image 20241009140323.png]]

Этот шаблон решает несколько задач, включая поддержку нескольких технологических стеков, разделение по характеристикам, упрощение взаимодействий и минимизацию связывания во время выполнения. Однако он также имеет недостатки, такие как усложнение службы команд, возможное увеличение координации в команде, замедление конвейеров развертывания и возможное увеличение связывания на этапе проектирования. Кроме того, он может потребовать более сложных взаимодействий и привести к публикации большого объема событий, что может повлиять на эффективность. В целом, этот шаблон предлагает преимущества, но также создает проблемы, которые необходимо тщательно учитывать при его реализации.
# Связанные паттерны

- The [[Database per service]] creates the need for this pattern
- The [[SAGA]] is an alternative solution
- The [[../../../../Microservices/Storage/Domain event]] pattern generates the events
- This pattern is structurally identical to [[CQRS]]