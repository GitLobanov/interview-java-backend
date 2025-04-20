---
layout: default
title: Главная
---

## Добрых суток, добрый человек. Твоему внимаю предстает методичка для подготовки по Java Backend

- [Сборник информации](posts/_inforage/INFO_README.md)
- [Сборник вопросов](posts/_qa/Theory/THEORY_README.md)
- [Сборник лайвкодингов](posts/_qa/Livecoding/LIVECODING_README.md)
- [Сборник теоритических задачек](posts/_qa/Theory_Tasks/THEORY_TASK_README.md)


### Последние материалы
{% for post in site.inforage limit:5 %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}