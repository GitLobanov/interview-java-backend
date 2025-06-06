## Просмотр истории коммитов с изменениями

  
Просматривать изменения, внесённые в репозиторий, можно с помощью параметра `log`. Он отображает список последних коммитов в порядке выполнения. Кроме того, добавив флаг `-p`, вы можете подробно изучить изменения, внесённые в каждый файл.  
  

```js
git log -p
```

## Отображение журнала фиксации в виде графика для текущей или всех веток

  
Просмотреть историю коммитов в виде графика для текущей ветки можно с помощью параметра log и флагов `--graph --oneline --decorate`. Опция `--graph` выведет график в формате ASCII, отражающий структуру ветвления истории коммитов. В связке с флагами `--oneline` и `--decorate`, этот флаг упрощает понимание того, к какой ветке относится каждый коммит.  
  

```js
git log --graph --oneline --decorate
```

  
Для просмотра истории коммитов по всем веткам используется флаг `--all`.  
  

```js
git log --all --graph --oneline --decorate
```

Пример выполнения:

```js
* f3f6c18 (HEAD -> main, origin/main) Fix login bug
* d3a3b50 Add user authentication
| * 7e93c7f (feature-branch) Implement feature X
| * 4e92b4b Start working on feature X
|/
* 87d3a56 Initial commit
```


