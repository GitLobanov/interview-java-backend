---
layout: default
title: Java Backend Подготовка
---

## Добрых суток, добрый человек. Твоему внимаю предстает методичка для подготовки по Java Backend

- [Сборник информации]({% link _inforage/INFO_README.md %})
- [Сборник вопросов]({% link _qa/Theory/THEORY_README.md %})
- [Сборник лайвкодингов]({% link _qa/Livecoding/LIVECODING_README.md %})
- [Сборник теоретических задачек]({% link _qa/Theory_Tasks/THEORY_TASK_README.md %})


### Последние материалы
{% for post in site.inforage limit:5 %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}