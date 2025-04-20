Представим таблицу с заказами, в которой хранится информация за несколько лет:

```SQL
CREATE TABLE Orders (
  OrderID INT,
  OrderDate DATE,
  CustomerID INT,
  Amount DECIMAL(10, 2)
) PARTITION BY RANGE (YEAR(OrderDate)) (
  PARTITION p2022 VALUES LESS THAN (2023),
  PARTITION p2023 VALUES LESS THAN (2024),
  PARTITION p2024 VALUES LESS THAN (2025)
);
```

В этом примере заказы за разные годы хранятся в разных партициях, что облегчает запросы к данным, если нас интересуют заказы за конкретный год.

Партиционирование чаще всего используется в системах, где объем данных очень велик ([[../../Microservices/Storage/Архитектура Data Warehouse и Data Lake#Data Warehouse (Хранилище данных)|Data Warehouses]], OLAP-системы) или когда важна производительность при работе с огромными таблицами (например, в аналитических базах данных).