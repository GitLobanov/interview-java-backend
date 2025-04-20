#### Задача 1. 

Задача: У клиента может не быть лицевых счетов. По лицевому счёту может не быть транзакций. Необходимо написать SQL-запрос, возвращающий имя клиента, описание его лицевого счёта и среднюю сумму транзакций по этому счёту.  
Customer  
---------  
id (primary key)  
name  
address  
  
Account  
---------  
id(primary key)  
acc_number  
description  
customer_id(foreign key -> Customer.id)  
  
Fin_transaction  
---------------  
id(primary key)  
transact_date  
amount  
account_id(foreign_key -> Account.id)  
description