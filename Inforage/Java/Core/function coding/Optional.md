Помогает избежать NPE, предоставляя удобный API для работы с null

```java
Optional.of() - возвращает объект Optional, если переданный объект null - прокинет исключение

Optional.ofNullable() - как of(), только в случае null не будет прокидывать исключение

Optional.empty() - вернет пустой Optional

orElse(obj) - если Optional пустой, то вернет дефолтное значение obj

orElseGet(Supplier) - очень похож на orElse, только из-за наличия Supplier будет инициализировать дефолтное значение только при пустом Optional.

or(Supplier<Optional>) - вернет другой Optional в случае если текущий пустой
```