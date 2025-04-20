---
layout: default
title: Главная
---

## Добрых суток, добрый человек. Твоему внимаю предстает методичка для подготовки по Java Backend

- [Сборник информации](Inforage/INFO_README.md)
- [Сборник вопросов](QA/Theory/THEORY_README.md)
- [Сборник лайвкодингов](QA/Livecoding/LIVECODING_README.md)
- [Сборник теоритических задачек](QA/Theory_Tasks/THEORY_TASK_README.md)


### Последние материалы
{% for post in site.inforage limit:5 %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}