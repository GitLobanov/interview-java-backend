Это набор гарантий, которые предоставляет спецификация JVM на которые можно опираться при работе в многопоточном режиме. 

- An unlock action on monitor `m` _happens-before_ all subsequent lock actions on `m`
- A write to a volatile variable `v` happens-before all subsequent reads of `v` by any thread
- The final action in a thread `T1` _happens-before_ any action in another thread `T2` that detects that `T1` has terminated.
- An action that starts a thread _happens-before_ the first action in the thread it starts.
- If thread `T1` interrupts thread `T2`, the interrupt by `T1` _happens-before_ any point where any other thread (including `T2`) determines that `T2` has been interrupted (by having an `InterruptedException` thrown or by invoking `Thread.interrupted` or `Thread.isInterrupted`).
- The write of the default value (`zero`, `false`, or `null`) to each variable _happens-before_ the first action in every thread.

- Не только освобождение монитора, но и все действия, идущие в порядке программы до освобождения, будут видны другому треду после захвата этого же монитора
- Не только запись в volatile поле, но и все действия, идущие в порядке программы до записи, будут видны другому треду после чтения этого же поля
- Не только финальное действие, но и все предыдущие действия треда T1 будут видны другому треду после завершения `T1.join()`
- Не только первое действие в треде, но и все последующие действия увидят дефолтную инициализацию переменной