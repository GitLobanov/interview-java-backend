```SQL
CREATE TABLE products (
    id INT UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NULL,
    count INTEGER NULL,
    price INTEGER NULL
);
INSERT INTO products (id, name, count, price)
VALUES
    (1, 'Стиральная машина', 5, 12000),
    (2, 'Холодильник', 11, 17800),
    (3, 'Микроволновка', 3, 4100),
    (4, 'Пылесос', 2, 4500),
    (5, 'Вентилятор', 8, 700),
    (6, 'Телевизор', 7, 31740),
    (7, 'Тостер', 2, 2500),
    (8, 'Принтер', 4, 3000),
    (9, 'XBOX', 5, 19900),
    (10, 'Флешка 8Gb', 14, 700);
```