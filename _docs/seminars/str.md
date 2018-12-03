---
title: `str`
permalink: /str/
---

# `str`

В нашем курсе мы постоянно работаем с типом `str` — строками.
На этой странице представлен обзор всего того, что мы уже знаем о них.


### Создание строк

#### Одинарные и двойные кавычки

#### Тройные кавычки

#### «Сырые» строки

#### `str()`


### Поиск в строке

#### Оператор `in`

`in` позволяет узнать является ли одна строка частью другой.

```python
>>> long = 'Hello, dear Mary'
>>> short = 'dear'
>>> if short in long:
...     print(short, 'is a part of', long)
...
dear is a part of Hello, dear Mary
>>> short in long
True
>>> long in short
False
```

#### Поиск положения: `str.find()`, `str.rfind()`


### Регистр

#### Проверка регистра: `str.islower()`, `str.isupper()`, `str.istitle()`

#### Приведение к другому регистру: `str.lower()`, `str.upper()`, `str.title()`


### Разбиение и объединение

#### Разбиение: `str.split()`

#### Операторы `+` и `+=`

#### Объединение: `str.join()`


### Модификация

#### Замена: `str.replace()`

#### Обрезание хвоста: `str.strip()`
