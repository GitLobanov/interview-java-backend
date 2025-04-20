## 2PC

Для реализации этого алгоритма необходим `координатор` - компонент, управляющий ходом транзакции.

1. Координатор просит каждого из участников транзакции выполнить операции внутри транзакции.
2. Когда участники выполнили операции, они шлют подтверждение координатору.

`Первая фаза коммита`

1. Координатор спрашивает участников, могут ли они зафиксировать транзакцию.
2. Участники шлют ответы. Если участник отвечает положительно, то он будет `обязан` зафиксировать данные, иначе транзакция не закончится.

`Вторая фаза коммита`

1. Координатор получает ответы, и на их основании выносит решение, коммитить транзакцию или нет.
2. Решение пишется в журнал транзакции, для поддержания сохраняемости данных.
3. Если все ответы положительные, то координатор просит участников закоммитить транзакции.
4. Координатор собирает подтверждения фиксации транзакции, на этом распределенная транзакция завершается.

## Особенности 2PC

Двухфакторный коммит действительно гарантирует атомарное выполнение распределенной транзакции, но, к сожалению, он сильно влияет на производительность приложения и БД. Вот основные причины этого:

- Все участники транзакции ждут самого медленного участника, в теории это может длиться и бесконечность.
- В случае отказа координатора, все участники ждут его восстановления, удерживая при этом все взятые блокировки на данные.
- Если у участника произошел сбой после подтверждения готовности фиксации, координатор будет отправлять запросы на фиксацию, и остальные участники будут ждать подтверждения фиксации транзакции до тех пор, пока участник не восстановится, закоммитит транзакцию и подтвердит это координатору.

Еще не во всех базах данных реализована возможность использовать двухфакторный коммит.

> Обеспечивает атомарность и согласованность изменений, между распределенными ресурсами. 

> Коммитим транзакцию, отправляя подготовительный коммит, Transaction Coordinator (TC) проходит по всем сервисам, получаем результаты работы сервисов, если все хорошо отработало и результаты получены (OK), проверяет TC, разрешаем сервисам коммит изменений, в ином случае делаем abort изменений (Rollback)

![[../../../../../_res/Pasted image 20240928154155.png]]

- Единая точка отказа (транзакции висят до его восстановления) - трата ресурсов
- Все ждут завершения каждого сервиса, так как необходимо решения от TC
- 4 раунда обмена сообщениями (Commit to Transaction Coordinator (1) -> Service (2) -> Confirm in TC (3) -> Commit / Rollback (4))

Он делится на два этапа:

1. Фаза подготовки (Prepare Phase): В этом этапе координатор отправляет всем участникам (транзакциям) запрос на подготовку к коммиту (PREPARE). Участники проверяют, могут ли они завершить транзакцию, и если да, то они блокируют свои ресурсы и сообщают координатору о готовности к коммиту (YES). Если какой-либо участник не может завершить транзакцию, он сообщает об этом (NO).
2. Фаза коммита (Commit Phase): После того как координатор получил положительные ответы от всех участников, он отправляет команду на коммит (COMMIT). Все участники завершает транзакцию и снимают блокировки. Если же хотя бы один участник не готов, координатор отправляет команду на откат (ROLLBACK) всем участникам.

# Three-Phase Commit Protocol (3PC)

## What is 3PC?

The **Three-Phase Commit Protocol (3PC)** is an enhanced version of 2PC designed to address the limitations of 2PC, specifically the risk of blocking. 3PC introduces an additional phase between Prepare and Commit, known as the **Pre-Commit** phase, to ensure participants have enough time to respond, reducing the risk of indefinite blocking.

## How 3PC Works

3PC operates in three phases: **Prepare Phase**, **Pre-Commit Phase**, and **Commit Phase**.

1. **Prepare Phase:**

- Similar to 2PC, the coordinator sends a **Prepare** request to participants, asking if they can commit the transaction.
- Each participant responds with Yes or No.

**2. Pre-Commit Phase:**

- If all participants respond Yes, the coordinator sends a **Pre-Commit** message, indicating the transaction is likely to be committed.
- Participants acknowledge the Pre-Commit, preparing to commit the transaction but not yet completing it.

**3. Commit Phase:**

- If all participants acknowledge the Pre-Commit, the coordinator sends the **Commit** command.
- If any participant fails to acknowledge the Pre-Commit, the transaction is **Aborted**.

3PC mitigates blocking issues by adding a timeout mechanism, allowing participants to autonomously decide to commit or abort if they don’t receive further messages from the coordinator.

## Example Scenario for 3PC

Imagine a financial application processing a transaction across multiple bank accounts:

- A transaction coordinator oversees several bank databases to transfer funds between accounts.
- The coordinator sends a Prepare request to all databases.
- If all respond positively, the coordinator sends a Pre-Commit message to ensure all are ready.
- After receiving acknowledgments, the coordinator finally sends the Commit request to finalize the transaction. If any bank fails to acknowledge, the transaction is aborted.

## Advantages of 3PC

3PC reduces the likelihood of indefinite blocking, particularly helpful in situations with unreliable networks:

- **Timeout Mechanism**: Allows participants to make autonomous decisions if they don’t hear back from the coordinator.
- **Improved Fault Tolerance**: Reduces the risk of hanging transactions.
# Resources

- [Understanding Two-Phase and Three-Phase Commit Protocols](https://daminibansal.medium.com/understanding-two-phase-and-three-phase-commit-protocols-key-differences-use-cases-and-practical-975e7c663c67)
- 



