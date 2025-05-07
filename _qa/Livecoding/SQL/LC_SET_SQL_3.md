#### Задача 1. 

[DB money transfer](DBs/DB-money-transfer.md)
Написать процедуру move_money() на перевод денег между счетами

Решение:

```sql
CREATE OR REPLACE PROCEDURE move_money(
    from_account_id INT,
    to_account_id INT,
    amount NUMERIC
)
LANGUAGE plpgsql
AS $$
BEGIN    
	UPDATE accounts SET balance = balance - amount WHERE account_id = from_account_id;
	UPDATE accounts SET balance = balance + amount WHERE account_id = to_account_id;
	COMMIT;
END;
$$;
```