Можно просматривать список изменений, внесённых в репозиторий, используя параметр `diff`. По умолчанию отображаются только изменения, не подготовленные для фиксации.    

```js
git diff
```

Для просмотра подготовленных изменений необходимо добавить флаг `--staged`.    

```js
git diff --staged
```

Также можно указать имя файла как параметр и просмотреть изменения, внесённые только в этот файл.  

```js
git diff somefile.js
```

If `git diff` opens in a terminal pager like `less` (which is often the default), you can exit by pressing the `q` key.