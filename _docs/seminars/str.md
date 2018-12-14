---
title: Строки
permalink: /str/
---

# `str`

В нашем курсе мы постоянно работаем с типом `str` — строками.
На этой странице представлен обзор всего того, что мы уже знаем о них.


### Создание строк

#### Одинарные и двойные кавычки
—

#### Тройные кавычки
—

#### «Сырые» строки
—

#### `str()`
—


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
—


### Регистр

#### Проверка регистра: `str.islower()`, `str.isupper()`, `str.istitle()`

Эти методы возвращают `True` или `False` в зависимости от того, как написаны буквы в строке: строчными или заглавными.
`.islower()` сообщает, что все буквы в строке строчные, `.isupper()` — заглавные, `.istitle()` сообщает, что первая встретившаяся буква в каждом «слове» — заглавная, а остальные — строчные.

```python
>>> strings = ['h', 'H', 'hello', 'Hello', 'hello world', 'HELLO WORLD',
...            'Hello world', 'Hello World']
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

#### Приведение к другому регистру: `str.lower()`, `str.upper()`, `str.capitalize()`, `str.title()`
—


### Разбиение и объединение

#### Разбиение: `str.split()`
—

#### Операторы `+` и `+=`

Строки можно конкатенировать (объединять) с помощью операторов `+` и `+=`:

```python
>>> s1 = 'abc'
>>> s2 = 'XYZ'
>>> s1 + s2
'abcXYZ'
>>> s2 += s1
>>> s2
'XYZabc'
```

#### Объединение: `str.join()`

Если нужно конкатенировать (объединить) сразу много строк, которые являются элементами списка (или другой коллекции), то удобный и быстрый способ — это `.join()`. Строка, к которой применяется метод, используется как разделитель, а список строк, передаваемые как аргумент — как части новой строки.

```python
>>> strings = ['milk', 'eggs', 'honey', 'flour']
>>> ''.join(strings)
'milkeggshoneyflour'
>>> ', '.join(strings)
'milk, eggs, honey, flour'
>>> print('\n'.join(strings))
milk
eggs
honey
flour

```

Обратите внимание, что `.join()` — это обратный метод по отношению к `.split()`, вызванному с каким-то аргументом:

```python
>>> long_string = 'milk, eggs, honey, flour'
>>> delimiter = ', '
>>> strings = long_string.split(delimiter)
>>> long_string == delimiter.join(strings)
True
```

### Модификация

#### Замена: `str.replace()`
—

#### Обрезание хвостов: `str.strip()`
Метод `.strip()` удаляет «лишние» символы в начале и конце строки. По умолчанию, без аргументов, удаляются все пробельные символы (пробелы, переносы строк и т.п.) от начала и до первого непробельного символа, а также от последнего непробельного символа до конца строки.

```python
>>> s = '\t   \t hello world\n\n   \n'
>>> print(s)
	   	 hello

    

>>> s.strip()
'hello world'
```

Если в качестве аргумента передать строку, то вместо пробельных символов удаляться символы, указанные в переданной строке.

```python
>>> s = '--;....;.;-.;-...-;..;-...-;.--;help.-;...;-...-;-.--;---;..-;-.;--.;.;.-.;'
>>> s.strip('-.;')
'help'
```

