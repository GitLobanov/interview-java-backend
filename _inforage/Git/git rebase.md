## Использование перебазирования

  
Для доступа к этой функции используйте параметр `rebase` с указанием имени ветки. Перебазирование — это процесс объединения или перемещения последовательности коммитов на новый родительский снимок.  
  

```js
git rebase branch_name
```


Эта команда изменит основу ветки с одного коммита на другой, как если бы вы начали ветку с другого коммита. В Git это достигается за счёт создания новых коммитов и применения их к указанному базовому коммиту. Необходимо понимать, что, хотя ветка и выглядит такой же, она состоит из совершенно новых коммитов.

```js
A---B---C   (main)
     \
      D---E   (feature)
```

```js
git rebase main
```

```js
A---B---C   (main)
         \
          D'---E'   (feature)
```

Now, the `feature` branch appears as if it was based on the latest version of `main`. Note that `D'` and `E'` are new commits—copies of `D` and `E`—with potentially different commit hashes.