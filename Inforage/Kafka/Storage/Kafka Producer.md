## Proccess

![[Pasted image 20240927124509.png]]

Для каждого события мы создаем объект **ProducerRecord** и указываем _топик_, _ключ_ (здесь разделяем на основе _userId_) и, наконец, полезную нагрузку события в качестве _значения_.

Метод **send()** является асинхронным, поэтому мы указываем обратный вызов, который запускается, когда мы получаем результат обратно.

Если сообщение было успешно записано в Kafka, мы печатаем метаданные, в противном случае, если возвращается исключение, мы регистрируем его.

Kafka делает много вещей под капотом, когда вызывается метод **send()**, распишем: 

1. Fetch metadata, где синхронный доступ. Кэширует данные. Ходит в Zookeeper
2. Сообщение сериализуется с помощью указанного сериализатора. key.serializer / value.serializer / StringSerializer
3. Специальная функция определяет, в какую партицию должно быть направлено сообщение.
4. Compress message / compression codec
5. Accumulate batch / batch.size linger.ms / Ждет набора пакета для отправки / Kafka внутри хранит буферы сообщений; у нас есть один буфер для каждой партиции, и каждый буфер может содержать множество пакетов сообщений, сгруппированных для каждой партиции.
6. Наконец, потоки I/O собирают эти пакеты и отправляют их брокерам.
    На данный момент наши сообщения находятся в **процессе передачи** от клиента к брокерам. Брокеры отправили/получили сетевые буферы для того, чтобы сетевые потоки смогли принимать сообщения и передавать их какому‑либо потоку I/O, чтобы фактически сохранить их на диске.
5. В leader broker сообщения записываются на диск и отправляются ведомым брокерам для репликации. Здесь следует отметить одну вещь: сообщения сначала записываются в кэш страниц и периодически сбрасываются на диск.
6. Фолловеры (синхронизированные реплики) сохраняют и отправляют обратно подтверждение о том, что они воспроизвели сообщение.
7. Ответ **RecordMetadata** отправляется обратно клиенту.
8. Если произошел сбой и мы не получили подтверждение, мы проверяем, включена ли повторная попытка сообщения, и нам нужно повторить его отправку.
9. Клиент получает ответ.

## Send

```java
producer.send (
	record,
	(metadata, exception) -> {callback}
)
```
## Producer Record

```java
new ProducerRecord <TKey, TValue> (
	topic,
	partition,
	System.currentTimeMiles(),
	key,
	value,
	headers
)
```

## Partitioner

```java
partitioner.class = 
null 
RoundRobingPartitioner 
UniformStickyPartitioner 
...
```

[[Kafka распределение партиций]]
## Режимы отправки сообщения send() / Properties send

acks - 0. Не интересует подтверждение
acks - 1. Ждет подтверждение от лидер реплики
acks - all. Ждет подтверждения также от всех in sync replica (ISR)

## Репликация

Механизм копирования данных между брокерами Kafka для повышения надежности и доступности сообщений в случае сбоя одного из брокеров.

>Операции чтения и записи производят только с Leader-репликами

- Фоловеры переодически опрашивают лидер реплику на получение новых данных
- ISR фоловеры, в них идет синхронная запись из лидера. **min.insync.replicas = 3**
- Когда лидер реплика отпадает, выбирается одна из ISR реплик

## Стратегии отправки сообщений в партиции

Round Robin - равномерное распределение по партициям
Key-based - хэширование ключа, деление на кол-во патриций
Sticky partitioner - набирает батч для партиции, отправляет ее, берет другую
Custom partitioner extends Partitioner

|**Strategy**|**Description**|
|---|---|
|Default partitioner|The key hash is used to map messages to partitions. Null key messages are sent to a partition in a round-robin fashion.|
|Round-robin partitioner|Messages are sent to partitions in a round-robin fashion.|
|Uniform sticky partitioner|Messages are sent to a sticky partition (until the batch.size is met or linger.ms time is up) to reduce latency.|
|Custom partitioner|This approach implements the `Partitioner` interface to override the `partition` method with some custom logic that defines the key-to-partition routing strategy.|
## Properties производительность 

> Нельзя писать большие файлы

```properties
batch.size = 10000 // in bytes, restrict size of batch of msgs
linger.ms = 5 // отправки каждые ... ms
max.request.size = 10000 // ограничить размер трафика
```

## Properties compression

> Данные можно сжимать для уменьшения объема передаваемых файлов

```properties
comporession.type = none | gzip | snappy | lz4 | zstd
```
## Идемпотентный продюсер

Продюсер, который может безопасно отправлять сообщения несколько раз, но они будут записаны в партицию только один раз, предотвращая дублирование.
## Многопоточность и Producer

Если хотим создавать многопоточные приложения, лучше всего создать один экземпляр producer и разделить его между потоками.

![[Pasted image 20240927123600.png]]