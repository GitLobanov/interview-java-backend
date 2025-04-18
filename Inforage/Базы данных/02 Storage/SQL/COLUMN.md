## DATA TYPES

#### Number

1. `BIGINT`, `INT`, `MEDIUMINT`, `SMALLINT`, `TINYINT`: Represent integers. The farther to the left, the larger the number that can be stored. The most common setting is `INT`, which can store values ranging from **-2147483648** to **2147483647**.
2. `DOUBLE`, `FLOAT`, `DECIMAL`: Represent decimals, `DECIMAL` can be set to `DECIMAL(5, 2)`, which means that the value of the column must be 5 digits, and two of the digits occur after the decimal point, such as a value like `666.88`.

It should be noted that the values stored in `DOUBLE` and `FLOAT` are not precise. When you store **2.5** for related operations, it is likely to be calculated as **2.500000002**. Therefore, `DECIMAL` is recommended for high-accuracy data system requirements.

#### Datetime

1. `DATE`, `MONTH`, `YEAR`: Set to date, month, and year.
2. `DATETIME`, `TIMESTAMP`: A relatively complete date data format, in addition to the year, month, and day, it will also include hours, minutes, and seconds, which is more precise than `DATE`. `DATETIME` can accept a purely datetime value format like `8888-01-01 00:00:00`, but `TIMESTAMP` is limited to between `1970-01-01 00:00:01` and `2038-01-19 03:14:07`

#### Text

1. `CHAR`, `VARCHAR`: Store plain text, the former is suitable for data with fixed text length, such as currency abbreviations. The latter is suitable for data whose length will change, and the length of the text that can be stored needs to be set. The most common default setting is `VARCHAR(45)` for MySQL, which means each item in the column can store 45 characters. This is a point where many SQL beginners make mistakes when setting the column data type. Setting a size that is too small leads to exceeding the column size limit error.
2. `TEXT`, `LONGTEXT`: If you want to store text data whose maximum length is unknown, you can use `TEXT` related data type settings.

#### Special Data Types

There are also several special data types: 1. `BINARY`, `BLOB`: Store data of file type, such as images or videos. `BLOB` ([Binary Large Object](https://techterms.com/definition/blob)) is not only for those types but also for large unstructured data. However, it is rarely used in practice because it is difficult to manage files through database software. 2. `BOOLEAN`: Store the data of the logical operand. In simple terms, it is the data **true** or **false**. The database will replace it with **1** or **0** respectively. Suppose there is a column called `is_alive`, then each value is either true or false. 3. `JSON`: A very common data format in modern data exchange. We will provide more details on this data type in the `SQL Syntax: JSON` chapter.

## ATTRIBUTE

1. `NOT NULL`: It means that the value in the field cannot be `NULL`. So when we use `NOT NULL`, we are forcing the column to **NOT** accept null values, which means the field must always contain a value.
2. `AUTO_INCREMENT`: In the database, it will automatically generate the column's values one by one using numbers. It is similar to a serial number. 
3. `DEFAULT`: You can set the default value of the column because the data to be inserted in the field may be empty in some circumstances.

In different database systems, it may appear in different forms. For example, in PostgreSQL, the function of `AUTO_INCREMENT` will be replaced by the `SERIAL` setting, and Oracle will use `IDENTITY`. Therefore, by focusing on what each function can do rather than "what each function is," you can become more adaptable to different database systems.


## CREATE

```sql
ALTER TABLE `new_schema`.`users`
ADD COLUMN `age` INT NULL AFTER `name`;
```

## UPDATE

```SQL
ALTER TABLE `new_schema`.`users`
CHANGE COLUMN `id` `id` INT(11) NOT NULL AUTO_INCREMENT,
CHANGE COLUMN `name` `user_name` VARCHAR(45) NOT NULL DEFAULT 'No Name';
```

