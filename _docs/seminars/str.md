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

Эти методы возвращают `True` или `False` в зависимости от того, как написаны буквы в строке: строчными или заглавными.
`.islower()` сообщает, что все буквы в строке строчные, `.isupper()` — заглавные, `.istitle()` — первая встретившаяся буква заглавная, а остальные — строчные.

```python
>>> strings = ['h', 'H', 'hello', 'Hello', 'hello world', 'HELLO WORLD', 'Hello world', 'Hello World']
>>> for s in strings:
...     print(s, end=': ')
...     if s.islower():
...         print('lower', end=', ')
...     if s.isupper():
...         print('upper', end=', ')
...     if s.istitle():
...         print('title', end=', ')
...     print()
h: lower,
H: upper, title,
hello: lower,
Hello: title,
hello world: lower,
HELLO WORLD: upper,
Hello world:
Hello World: title,

```

Все эти методы для строки без букв (в том числе для пустой) возвращают `False`:
```python
>>> ''.islower() or ''.isupper() or ''.ititle()
False
>>> '123'.islower() or '123'.isupper() or '123'.istitle()
False
```

#### Приведение к другому регистру: `str.lower()`, `str.upper()`, `str.title()`


### Разбиение и объединение

#### Разбиение: `str.split()`

#### Операторы `+` и `+=`

#### Объединение: `str.join()`


### Модификация

#### Замена: `str.replace()`

#### Обрезание хвоста: `str.strip()`
