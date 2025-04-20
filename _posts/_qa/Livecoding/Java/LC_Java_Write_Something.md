#### Задача 1. 

```java
/*
Класс иммутабельный?
*/
public class ImmutableBankTransaction {
    
    public StringBuilder transactionId;
    public StringBuilder fromAccount;
    public StringBuilder toAccount;
    public BigDecimal amount;
    public LocalDateTime timestamp;
    public Map<String, StringBuilder> metadata;
    public List<String> statusHistory;

    public ImmutableBankTransaction(
            StringBuilder transactionId,
            StringBuilder fromAccount,
            StringBuilder toAccount,
            BigDecimal amount,
            LocalDateTime timestamp,
            Map<String, String> metadata,
            List<String> statusHistory) {
        this.transactionId = transactionId;
        this.fromAccount = fromAccount;
        this.toAccount = toAccount;
        this.amount = amount;
        this.timestamp = timestamp;
        this.metadata = metadata;
        this.statusHistory = statusHistory;
    }
}

public class ImmutableBankTransactions {
    
    public HashMap<StringBuilder, ImmutableBankTransaction> immutableHashMap = HashMap();

    public ImmutableBankTransactions(HashMap<StringBuilder, ImmutableBankTransaction> outsideMap) {
	    immutableHashMap = outsideMap;
    }

	public ImmutableBankTransaction getByTransactionId(StringBuilder transactionId) {
		return immutableHashMap.get(transactionId);
	}
}
```

#### Задача 2. 

```java

/*
Необходимо реализовать класс, который умеет принимать элементы
и в любой момент времени возвращать N(capacity) наибольших уникальных элементов.
Если количество элементов помещенных в класс меньше N - то требуется организовать ожидание,
до тех пор пока не наберется N элементов
 */
public class TopNElementsCollection {

    public TopNElementsCollection(int capacity) {

    }

    public void push(int value) {

    }

    public List<Integer> pull() throws InterruptedException {

    }
}
```