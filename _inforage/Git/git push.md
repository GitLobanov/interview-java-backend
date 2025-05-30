## Отправка изменений в удалённый репозиторий

  
Отправлять изменения в удалённый репозиторий можно параметром `push` с указанием имени репозитория и ветки.  
  

```js
git push origin main
```

  
Эта команда передаёт локальные изменения в центральный репозиторий, где с ними могут ознакомиться другие участники проекта.


## Отправка новой ветки в удалённый репозиторий

  
Передать новую ветку в удалённый репозиторий можно параметром push с флагом `-u`, указав имя репозитория и имя ветки.  
  

```js
git push -u origin new_branch
```

## Удаление удаленной ветки

Если хотите <mark style="background: #ADCCFFA6;">стереть удалённую ветку</mark>, выполните следующую команду, указав имя удалённого репозитория и имя ветки.

```js
git push origin --delete existing_branch_name
```

