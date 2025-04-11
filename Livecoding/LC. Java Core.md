

```java
try {
	return Integer.parseInt(number);
} catch (Exception e) {
	throw new RuntimeException("Ошибка", e);
} finally {
    return 0;
}
```