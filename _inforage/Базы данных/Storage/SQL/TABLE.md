## CREATE

```SQL
CREATE TABLE `new_schema`.`new_table` ( 
	`id` INT NOT NULL COMMENT 'This is a primary index', 
	PRIMARY KEY (`id`) 
);
```

## SHOW

```sql
SHOW FULL COLUMNS FROM `new_schema`.`new_table`;
```

The result should be:

|Field|Type|Collation|Null|Key|Default|Extra|Privileges|Comment|
|---|---|---|---|---|---|---|---|---|
|id|int(11)||NO|PRI|||select, insert, update, references|This is a primary index|

## DROP

```sql
DROP TABLE `new_schema`.`new_table`;
```

## CLEAN DATA

In order to regenerate the test data, we clear the table frequently, then we will use the `TRUNCATE` statement, to delete all data, but not the table. 

```sql
TRUNCATE `new_schema`.`new_table`;
```


