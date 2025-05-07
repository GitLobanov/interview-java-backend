[Мануал по работе с бд](Мануал.md)

```SQL
-- Создаем таблицу клиентов
CREATE TABLE Clients (
    client_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    birth_date DATE NOT NULL,
    passport_number VARCHAR(20) UNIQUE NOT NULL,
    phone_number VARCHAR(15) NOT NULL,
    email VARCHAR(100),
    registration_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(10) CHECK (status IN ('active', 'inactive', 'blocked')) DEFAULT 'active'
);

-- Таблица счетов
CREATE TABLE Accounts (
    account_id SERIAL PRIMARY KEY,
    client_id INT NOT NULL REFERENCES Clients(client_id),
    account_number VARCHAR(20) UNIQUE NOT NULL,
    account_type VARCHAR(10) CHECK (account_type IN ('checking', 'savings', 'credit', 'deposit')) NOT NULL,
    currency VARCHAR(3) DEFAULT 'RUB',
    balance NUMERIC(15, 2) DEFAULT 0.00,
    open_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    close_date TIMESTAMP,
    status VARCHAR(10) CHECK (status IN ('active', 'closed', 'frozen')) DEFAULT 'active'
);

-- Таблица карт
CREATE TABLE Cards (
    card_id SERIAL PRIMARY KEY,
    account_id INT NOT NULL REFERENCES Accounts(account_id),
    card_number VARCHAR(16) UNIQUE NOT NULL,
    card_type VARCHAR(10) CHECK (card_type IN ('debit', 'credit', 'prepaid')) NOT NULL,
    expiration_date DATE NOT NULL,
    cvv VARCHAR(3) NOT NULL,
    issue_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(10) CHECK (status IN ('active', 'blocked', 'expired')) DEFAULT 'active',
    daily_limit NUMERIC(15, 2)
);

-- Добавляем клиентов
INSERT INTO Clients (first_name, last_name, birth_date, passport_number, phone_number, email)
VALUES 
('Иван', 'Иванов', '1985-05-15', '1234567890', '+79161234567', 'ivanov@example.com'),
('Петр', 'Петров', '1990-07-22', '0987654321', '+79167654321', 'petrov@example.com'),
('Мария', 'Сидорова', '1982-11-30', '1122334455', '+79165554433', 'sidorova@example.com'),
('Анна', 'Кузнецова', '1995-03-10', '5566778899', '+79168887766', 'kuznetsova@example.com');

-- Добавляем счета клиентов
INSERT INTO Accounts (client_id, account_number, account_type, balance)
VALUES 
(1, '40817810000000000001', 'checking', 150000.00),
(1, '40817810000000000002', 'savings', 500000.00),
(2, '40817810000000000003', 'checking', 75000.50),
(3, '40817810000000000004', 'credit', -100000.00),
(4, '40817810000000000005', 'checking', 25000.75);

-- Добавляем карты
INSERT INTO Cards (account_id, card_number, card_type, expiration_date, cvv, daily_limit)
VALUES 
(1, '4276123456789012', 'debit', '2025-12-31', '123', 100000.00),
(1, '4276987654321098', 'credit', '2026-06-30', '456', 50000.00),
(3, '4276543210987654', 'debit', '2024-09-30', '789', 50000.00),
(5, '4276098765432109', 'debit', '2025-03-31', '321', 30000.00);

-- Добавляем транзакции
INSERT INTO Transactions (account_id, card_id, amount, transaction_type, description)
VALUES 
(1, 1, 5000.00, 'payment', 'Оплата ресторана'),
(1, NULL, 100000.00, 'deposit', 'Пополнение счета'),
(3, 3, 15000.50, 'withdrawal', 'Снятие наличных'),
(5, 4, 5000.75, 'payment', 'Оплата услуг ЖКХ'),
(1, 2, 20000.00, 'payment', 'Покупка техники');
```