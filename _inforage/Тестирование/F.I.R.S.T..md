1. Fast - юнит тесты должны быть быстрыми, так как они часто запускаются и их много  
2. Isolated - они должны быть изолированы друг от друга и не использовать общее состояние. Это может сломать тесты при изменении порядка их выполнения.  
3. Repeatable - результат должен быть один и тот же, сколько бы раз он ни был запущен  
4. Self-validated - должно быть очевидно, что тест прошел, либо же зафейлился. Не должно быть промежуточных состояний, которые нужно проверять вручную, чтобы убедиться в успешности теста  
5. Timely - тесты должны писаться и выполняться вовремя


- **F**: Fast (Быстрые) — Тесты должны выполняться быстро.
- **I**: Independent (Независимые) — Тесты должны быть независимы друг от друга.
- **R**: Repeatable (Повторяемые) — Тесты должны давать одинаковый результат при каждом запуске.
- **S**: Self-validating (Автопроверяемые) — Результаты тестов должны быть легко интерпретируемыми (истина или ложь).
- **T**: Timely (Своевременные) — Тесты должны быть написаны в разумные сроки, как часть разработки.