> Producer

```java
@Configuration
public class KafkaConfiguration {

	@Bean
	public NewTopic newTopic () {
		return new NewTopic ("nameTopic1",
			Partitions : 1 , ReplicationFactor : (short) 1)
	}

}

@Service
public class KafkaProducer {

	private final KafkaTemplate <String,String> kafkaTemplate () {
		this.kafkaTemplate = kafkaTemplate;
	}

	public void sendMessage (String message) {
		kafkaTemplate.send("nameTopic1", message)
		
		...
		String topic, @Nullable String data
		String topic, String key, @Nullable String data
		String topic, Integer partition, String key, @Nullable data
		ProducerRecord <String, String> record;
		Message<?> message;
		...
	}

}
```

> Consumer

```java
@Service
public class KafkaConsumer {

	@KafkaListener(topics = "nameTopice1", groupId = "my_consumer")
	public void listen (String message) {
		System.out.println("Recieved message = " + message)
	}


}

```