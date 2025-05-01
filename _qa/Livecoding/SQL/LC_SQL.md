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

#### Задача 2

Компании 000 "Ромашка" представлена таблицами employee(id, department_id, name, salary) и department (id, name)
Вывести список сотрудников (id, name), получающих максимальную заработную плату в своем отделе

#### Задача 3

Даны две таблицы:
FISH (id int ,name varchar);
CATCH (id catch, fish_id int, dt date, quantity int);

Выбрать имена рыб, улов по которым на определенную дату D меньше N;
(улов считается меньше N также если он отсутствует вовсе)