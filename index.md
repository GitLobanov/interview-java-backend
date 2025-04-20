---
layout: default
title: Главная
---

## Добрых суток, добрый человек. Твоему внимаю предстает методичка для подготовки по Java Backend

- [Сборник информации](_posts/_inforage/INFO_README.md)
- [Сборник вопросов](_posts/_qa/Theory/THEORY_README.md)
- [Сборник лайвкодингов](_posts/_qa/Livecoding/LIVECODING_README.md)
- [Сборник теоритических задачек](_posts/_qa/Theory_Tasks/THEORY_TASK_README.md)


### Последние материалы
{% for post in site.inforage limit:5 %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}