## Proccess

- Pool messages
- fetch metadata <- Zookeeper
- Pool data from brokers
- Consumer group / group.id / балансировка чтения данных
- offset - last consumed message by Group
- Topic consumer offsets  - хранит данные о последних прочитанных партиций / Partition: A/0, Group: x, offset 2
- at most once (miss messages) - авто коммит, потеря
- at least once (duplicate messages) - ручной коммит, дублирование
- exactly once (not missed, not duplicates) - хранение где-то offset к примеру в базе данных

Компонент, который читает сообщения из топика Kafka и обрабатывает их, поддерживая группу консумеров для горизонтального масштабирования.

![[../../../_res/Pasted image 20240927125721.png]]

Kafka использует модель извлечения данных, где основой работы consumer’а является poll loop. Он выполняет две задачи: выборку данных для обработки и отправку сигналов тревоги для координации консумеров в группе. 

Приложения поддерживают TCP-соединения с брокерами для получения данных, которые кэшируются и возвращаются через poll(). Важно правильно настроить параметры, такие как max.poll.interval.ms, чтобы избежать проблем с потерей consumer’ов при медленной обработке сообщений. Также параметры, связанные с размером выборки и тайм-аутами, должны быть тщательно протестированы для оптимальной работы системы.

```java
new KafkaConsumer <TKey, TValue> ( 
	consumerProperties,
	keyDeserializer,
	valueDeserializer
)
```

```java
// подписаться на список топиков
consumer.subcribe(topics)
// подписаться на топики по шаблону имени, для динмаоческого добавления новых топиков
consumer.subcribe(pattern)
```

```java
ConsumerRecords<UUID, Event>  records = 
			// polling data every 5 seconds
			consumer.poll(Duration.ofSeconds(5));
```

## [[Commit offset in Kafka]]

## Семантики доставки

### Кратко

- **At most once** — самое больше один
- **At least once** — минимум один
- **Exactly once** — ровно один раз, без дубликатов. 2 phase commit, дедупликация на стороне консюмер (ключ идемпотентности)
## Расширенно

![[../../../_res/Pasted image 20240928142055.png]]
### at most once - ack mode не задан (auto commit), commit before process

Когда у нас не задан ack mode, то при получении новых данных, консьюмер сразу коммитит сообщение при получении. С этим могут возникнуть проблемы, ведь сообщение может быть каким-то образом обработано, что может вызвать ошибку, вследствие чего сообщение просто потеряется.

Если продюсер не производит повторную отправку сообщения по истечении тайм-аута или получения ошибки, то сообщение может не записаться в топик Kafka, и, следовательно, оно не будет доставлено потребителю. В большинстве случаев сообщения будут доставляться, но, чтобы избежать вероятности дублирования, мы допускаем, что иногда сообщения не доходят.

```java
while (true) {
	ConsumerRecords<UUID, Event> records = 
			consumer.poll(timer.toDuration());
	consumer.commitAsync(); (1)
	process(records); (2)
}
```
### at least once - manual ack mode (manual commit), commit after process

В ситуации с manual ack mode может возникнуть другая проблема, когда был получен батч сообщений и одно из них вызвало ошибку. В таком случае консьюмер снова попытается получить этот батч и сообщения в нем, которые были обработаны до сообщения с ошибкой будут получены и обработы снова, что подразумевает дублирование сообщений. В таком случае, если очередь сообщений не идемпотентна (отслеживание изменений имени пользователя), то могут возникнуть различные проблемы.

Если продюсер получает подтверждение от брокера Kafka, и при этом `acks=all`, это означает, что сообщение было записано в топик Kafka строго однократно. Но если продюсер не получает подтверждение по истечении тайм-аута или получает ошибку, то он может попробовать снова отправить сообщение, считая, что оно не было записано в топик Kafka. Если брокер дал сбой непосредственно перед отправкой подтверждения, но после того, как сообщение было успешно записано в топик Kafka, эта повторная попытка отправки приведёт к тому, что сообщение будет записано и отправлено конечному потребителю дважды. Все будут довольны неутомимостью раздающего, но такой подход приводит к дублированию работы и некорректным результатам.

```java
while (true) {
	ConsumerRecords<UUID, Event> records = 
			consumer.poll(timer.toDuration());
	process(records); (1)
	consumer.commitAsync(); (2)
}
```
### exactly once - написана своя логика, которая не полагается на Kafka offset
Если того требует бизнес задача, чтобы таких проблем не возникало, стоит написать свою систему управления смещениями, которая будет сохранять в нужное место смещение сообщения, которое удалось успешно обработать.

- Хранения транзакций в бд, получаем сообщение из кафки, смотрим есть ли оно в бд, если нет выполняем и сохраняем
- in/outbox + kafka
- kafka транзакции

#### Транзакции: атомарные записи в нескольких разделах

Теперь Kafka поддерживает атомарную запись в нескольких разделах при помощи новых транзакционных API. Это позволяет продюсеру отправлять пакеты сообщений в несколько разделов так, что либо все сообщения из пакета будут видны любому потребителю, либо ни одно из них не будет видно никому. Данная функция также позволяет осуществлять смещение потребителя в одной транзакции с данными, которые вы обработали, а, следовательно, делает возможной сквозную семантику exactly-once. Вот сниппет с примером кода, который демонстрирует использование транзакционного API:

```
producer.initTransactions();
try {
 producer.beginTransaction();
 producer.send(record1);
 producer.send(record2);
 producer.commitTransaction();
} catch(ProducerFencedException e) {
 producer.close();
} catch(KafkaException e) {
 producer.abortTransaction();
}
```

https://ru.wikipedia.org/wiki/Задача_двух_генералов
## Стратегии распределения консюмеров

| **Strategy**             | **Description**                                                                                                                                                                                                                                                                                               |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Range assignor (default) | (Total number of partitions) / (Number of consumers) partitions are assigned to each consumer. Например, если у вас есть 6 партиций и 2 консюмера, первый консюмер получит партиции 1-3, а второй — 4-6.                                                                                                      |
| Round-robin assignor     | Partitions are picked individually and assigned to consumers (in any rational order, say from first to last). When all the consumers are used up but some partitions still remain unassigned, they are assigned again, starting from the first consumer. The aim is to maximize the number of consumers used. |
| Sticky assignor          | This approach works similar to round robin assignor but preserves as many existing assignments as possible when reassignment of partitions occurs. The aim is to reduce or completely avoid partition movement during rebalancing.                                                                            |
| Custom assignor          | Extends the `AbstractPartitionAssignor` class and overrides the `assign` method with custom logic.                                                                                                                                                                                                            |
## Consumer group

![[../../../_res/Pasted image 20240928142525.png]]

 > объединение потребителей для многопоточного (многопользовательского) использования топиков Kafka.
 
```properties
 group.id = consumer-group-name
```

- id — номер группы, который присваивается ей при создании для возможности подключения потребителей, использующих в качестве параметра соединения этот идентификатор (id). Следовательно, для параллельного использования группы, потребители используют один и тот же group.id;
- брокер Kafka назначает разделы топика потребителю в группе таким образом, что каждый раздел потребляется ровно одним потребителем в группе;
- потребители видят сообщение в том порядке, в котором они были сохранены в журнале, независимо от того, в какой момент времени они подключились к группе;
- максимальный параллелизм группы достигается лишь тогда, когда в топике нет разделов

## Consumer rebalancing

- **Eager rebalancing:** All the consumers stop consuming, give up ownership of their partitions, rejoin the group, and then get new partitions assigned to them. This causes a small window of downtime (consumer unavailability) when there are no consumers in the entire group.
- **Cooperative rebalancing:** Also called incremental rebalancing, this strategy performs the rebalancing in multiple phases. It involves reassigning a small subset of partitions from one consumer to another, allowing consumers to continue processing messages from partitions that are not reassigned and avoiding total unavailability.

## Масштабирование стороны consumer’а

Группы consumer’ов - это способ Кафки разделить работу между разными consumer’ами, а также уровень параллелизма. Самый высокий уровень параллелизма, которого вы можете достичь с помощью Kafka, — это наличие одного consumer’а, потребляющего из каждой партиции темы.
## Шаблон consumer

 У вас могут быть consumer’ы в вашей группе, потребляющие из одного или нескольких партиций темы, но затем они распространяют фактическую обработку на другие потоки.

Он обеспечивает три гарантии:

_Unordered_ — не дает никаких гарантий.
_Keyed_ — гарантирует упорядочение по ключу, но с оговоркой, что пространство ключей должно быть довольно большим, иначе вы можете не заметить значительного улучшения производительности.
_Partition_ — в любой момент на каждую партицию будет обработано только одно сообщение.

![[../../../_res/Pasted image 20240927130901.png]]


## Resources

- [Kafka partition strategy—Tutorial & strategies](https://www.redpanda.com/guides/kafka-tutorial-kafka-partition-strategy)
- https://ru.wikipedia.org/wiki/Задача_двух_генералов