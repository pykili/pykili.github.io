---
title: 15 &mdash; Южный парк
---

# На прошлом занятии
* вложенные структуры

# План этого занятия
* парсинг диалогов «Южного парке»

# Пример вложенной структуры

Будем работать с уникальным набором данных: набором всех реплик персонажей сериала «South Park» за первые 18 сезонов. Для работы понадобится файл [all-seasons.csv](https://github.com/walkerkq/textmining_southpark/blob/master/data/raw%20data/all-seasons.csv)

Для решения целого ряда прикладных задач с данными будет удобно создать словарь `d`, который будет содержать в качестве ключей номер сезона, а в качестве элементов — словари, которые, в свою очередь, будут содержать в качестве ключа номер эпизода, а в качестве элемента — словари, содержащие в качестве ключа имя персонажа, а качестве значения список его реплик, произнесенных в данном эпизоде. Звучит сложно, но давайте представим как будет выглядеть такой словарь `d`:
```python
d = {
    'сезон1': {
        'эпизод1': {
            'персонаж1': [
                'реплика1',
                'реплика2',
                'реплика3',
            ],
            'персонаж2': [
                'реплика1',
                'реплика2',
                'реплика3',
            ],
        },
        'эпизод2': {
            'персонаж1': [
                'реплика1',
            ],
        },
    },
    'сезон2': {
        'эпизод1': {
            'персонаж1': [
                'реплика1',
                'реплика2',
                'реплика3',
            ],
        },
        'эпизод2': {
            'персонаж1': [
                'реплика1',
                'реплика2',
            ],
            'персонаж2': [
                'реплика1',
                'реплика2',
                'реплика3',
            ],

        },
    },
}
```

Забегая вперед отметим, что для того, что бы получить первую реплику Кайла в первом эпизоде пятого сезона нужно будет просто обратиться к элементу `d['5']['1']['Kyle'][0]`. Здесь используются номера сезона и эпизода в кавычках для простоты написания кода, можно было сделать и более аккуратно.

Вот пример кода, который создает такой сложный словарь из нашего файла:
```python
def get_south_park_dict(filename):
    d = {}
    with open(filename, encoding='utf-8') as f:
        quote = ''
        for line in f:
            if line == 'Season,Episode,Character,Line\n':
                continue
            if quote == '':
                # Именно здесь у нас переменные season и episode задаются.
                # Видно, что они являются строковыми переменными, хотя,
                # конечно, это можно легко изменить, но делать это мы не будем
                season, episode, char, quote = line.split(',', maxsplit=3)
            else:
                quote += line
            if line == '"\n':
                season_dict = d.setdefault(season, {})
                episode_dict = season_dict.setdefault(episode, {})
                quote_list = episode_dict.setdefault(char, [])
                quote_list.append(quote)
                quote = ''
    return d
```

## Примеры задач, для которых удобна такая вложенная структура

### Отсортированный список персонажей, разговаривавших в эпизоде
```python
def get_characters(d, season, episode):
    """Возвращает список персонажей эпизода, отсортированный в алфавитном порядке"""
    # Ключи d[season] - эпизоды, а ключи d[season][episode] - персонажи
    return sorted(d[season][episode])
```

### Какой сезон самый длинный?
```python
def get_the_longest_season(d):
    """Имя самого длинного сезона. Если их несколько, то может быть возвращен любой из них"""
    max_len = 0
    for k, v in d:  # k - название сезона, v - словарь с ключами-эпизодами
       if len(v) > max_len:
           max_len = len(v)
           season = k
    return season
```
> Почитайте [документацию встроенной функции `max`](https://docs.python.org/3/library/functions.html#max) и придумайте как применить её вместо всей этой функции.

### Кто произнес больше всего фраз в данном эпизоде?
```python
# Будем использовать collections.Counter и вспомогательную функцию, которая вернет несколько
# самых болтливых персонажей

import collections

def chatterboxes(d, season, episode, top_n):
    """Список пар персонаж-количество реплик, уопрядоченных по убыванию числа реклик,
    не более top_n штук
    """
    counter = collections.Counter()
    for k, v in d[season][episode].items():
        counter[k] = len(v)
    return counter.most_common(top_n)

def chatterbox(d, season, episode):
    """Самый болтливый персонаж эпизода, если таких несколько, будет возвращен только один из них"""
    return chatterboxes(d, season, episode, 1)[0][0]
```

### В каких эпизодах слово/словосочетание употреблялось не менее заданного числа раз
```python
def episodes_with_word(d, word, at_most):
    """Список эпизодов в формате 'сезон.эпизод', в которых word встретился по
    крайней мере at_most раз
    """
    episodes = []
    word = word.lower()
    for season, season_dict in d.items():
        for episode, episode_dict in season_dict.items():
            n = 0
            for char, quote_list in episode_dict.items():
                for quote in quote_list:
                    n += quote.lower().count(word)
            if n >= at_most:
                episodes.append(str(season)+'.'+str(episode))
    return episodes
```

# Домашнее задание

**???**
