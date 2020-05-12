---
title: JSON
---

## Формат json

**JSON** &mdash; простой, основанный на использовании текста, способ хранить и передавать структурированные данные. 

JSON значит *JavaScript Object Notation.*

Его придумали для того, чтобы упростить обмен данными. 

Его предложения легко читаются и составляются как человеком, так и компьютером.

Его легко преобразовать в структуру данных для большинства языков программирования (числа, строки, логические переменные, массивы и так далее).

Многие языки программирования имеют функции и библиотеки для чтения и создания структур JSON. 

## Правила json

Строка json может содержать __объект__, и тогда она начинается с `{` и заканчивается на `}`. 
Такой объект очень похож на питоновский *словарь*: 
у него есть ключи - строки, которые пишутся в кавычках, а через двоеточие пишется значение, 
пары ключ-значение разделяются запятыми. Например:
```json
{"first_name": "Guido", "last_name":"Rossum"}
```

Строка json может содержать __массив__, и тогда она начинается с `[` и заканчивается на `]`. 
Такой массив очень похож на питоновский массив: в нем значения перечисляются через запятую. Например:

```json
{"some_people": ["Guido van Rossum", "Diana Clarke", "Naomi Ceder", "Van Lindberg", "Ewa Jodlowska"]}
```

Значение в массиве или объекте может быть:
* Числом (целым или с плавающей точкой)
* Строкой (в двойных кавычках)
* Логическим значением (true или false)
* Другим массивом (заключенным в квадратные скобки)
* Другим объектом (заключенным в фигурные скобки)
* Значением null

Чтобы включить в строку специальные символы (например, кавычку), их нужно экранировать с помощью \, например, `\"` или `\r\n`. Наглядные правила построения json-строки можно посмотреть на официальном сайте http://www.json.org/, если захочется.

Может показаться, что это вообще-то все и так очень похоже на обычный питон. Но это не так. Во-первых, json &mdash; это не исполняемый код, а просто текст. Во-вторых, очень часто запись валидного питоновского словаря или массива не будет являться валидной записью в формате json. Например, это не json, но при этом словарь: `{(1, 'a'): u'12345'}`. (Попробуйте придумать еще примеры.)

Вот еще пример строки json, посложнее:
```json
{"organisation": "Python Software Foundation",
 "officers": [
            {"first_name": "Guido", "last_name":"Rossum", "position":"president"},
            {"first_name": "Diana", "last_name":"Clarke", "position":"chair"},
            {"first_name": "Naomi", "last_name":"Ceder", "position":"vice chair"},
            {"first_name": "Van", "last_name":"Lindberg", "position":"vice chair"},
            {"first_name": "Ewa", "last_name":"Jodlowska", "position":"director of operations"}
            ],
"type": "non-profit",
"country": "USA",
"founded": 2001,
"members": 244,
"budget": 750000,
"url": "www.python.org/psf/"}
```

## Модуль json

В питоне есть стандартный модуль `json`. В основном из этого модуля используют такие функции:

* `loads`  - превратить строку в формате JSON в объект питона - словарь или массив. У этой функции один обязательный аргумент - строка.
* `dumps`  - превратить питоновский словарь или массив в строку JSON. У этой функции один обязательный аргумент - словарь или массив.
* `load` - прочитать файл и превратить JSON, который в нем находится, в объект питона. У этой функции два обязательных аргумента - файл и объект питона.
* `dump` - превратить питоновский словарь или массив в строку JSON и записать ее в файл. У этой функции два обязательных аргумента - файл и объект питона.

Под словом "файл" в данном случае имеется в виду любой файло-подобный объект &mdash; собственно файл, или стандартный ввод-вывод, или даже запросы, которые мы отправляем через `urllib.request`, то есть такие объекты, к которым можно применить метод `.read()`.


## Пример
Наша json-ка записана в файл (файлы, содержащие данные в формате `json` традиционно именуют с расширением `.json`). Попробуем прочитать этот файл в объекты питона.

ниже &mdash; код чтобы в jupyter-тетрадке записать нашу json-ку в файл `my_cool_data.json`, **код на питоне ещё чуть ниже**.

```
%%writefile my_cool_data.json
{"organisation": "Python Software Foundation",
 "officers": [
            {"first_name": "Guido", "last_name":"Rossum", "position":"president"},
            {"first_name": "Diana", "last_name":"Clarke", "position":"chair"},
            {"first_name": "Naomi", "last_name":"Ceder", "position":"vice chair"},
            {"first_name": "Van", "last_name":"Lindberg", "position":"vice chair"},
            {"first_name": "Ewa", "last_name":"Jodlowska", "position":"director of operations"}
            ],
"type": "non-profit",
"country": "USA",
"founded": 2001,
"members": 244,
"budget": 750000,
"url": "www.python.org/psf/"}
```
```
Writing my_cool_data.json
```


Ниже приведён код, способный прочитать наш json-файл в питон.
```python
import json

my_cool_f = open("my_cool_data.json")
data = json.load(my_cool_f)
my_cool_f.close()

print(type(data))  # распечатаем тип объекта и убедимся, что теперь это не строка, а словарь
```

Результат работы кода:
```
{'budget': 750000,
 'country': 'USA',
 'founded': 2001,
 'members': 244,
 'officers': [{'first_name': 'Guido',
               'last_name': 'Rossum',
               'position': 'president'},
              {'first_name': 'Diana',
               'last_name': 'Clarke',
               'position': 'chair'},
              {'first_name': 'Naomi',
               'last_name': 'Ceder',
               'position': 'vice chair'},
              {'first_name': 'Van',
               'last_name': 'Lindberg',
               'position': 'vice chair'},
              {'first_name': 'Ewa',
               'last_name': 'Jodlowska',
               'position': 'director of operations'}],
 'organisation': 'Python Software Foundation',
 'type': 'non-profit',
 'url': 'www.python.org/psf/'}
```



## Пример

Попробуем превратить нашу строку в объекты питона:

```python
json_string = """{"organisation": "Python Software Foundation",
                 "officers": [
                            {"first_name": "Guido", "last_name":"Rossum", "position":"president"},
                            {"first_name": "Diana", "last_name":"Clarke", "position":"chair"},
                            {"first_name": "Naomi", "last_name":"Ceder", "position":"vice chair"},
                            {"first_name": "Van", "last_name":"Lindberg", "position":"vice chair"},
                            {"first_name": "Ewa", "last_name":"Jodlowska", "position":"director of operations"}
                            ],
                "type": "non-profit",
                "country": "USA",
                "founded": 2001,
                "members": 244,
                "budget": 750000,
                "url": "www.python.org/psf/"}"""

import json

data = json.loads(json_string)
print(type(data))  # распечатаем тип объекта и убедимся, что теперь это не строка, а словарь

from pprint import pprint

pprint(data) # посмотрим на сам этот словарь
```

Результат работы кода:
```
<class 'dict'>
{'budget': 750000,
 'country': 'USA',
 'founded': 2001,
 'members': 244,
 'officers': [{'first_name': 'Guido',
               'last_name': 'Rossum',
               'position': 'president'},
              {'first_name': 'Diana',
               'last_name': 'Clarke',
               'position': 'chair'},
              {'first_name': 'Naomi',
               'last_name': 'Ceder',
               'position': 'vice chair'},
              {'first_name': 'Van',
               'last_name': 'Lindberg',
               'position': 'vice chair'},
              {'first_name': 'Ewa',
               'last_name': 'Jodlowska',
               'position': 'director of operations'}],
 'organisation': 'Python Software Foundation',
 'type': 'non-profit',
 'url': 'www.python.org/psf/'}
```

Попробуем поработать с этим словарем. например, распечатаем его ключи.
```python
for key in data: 
    print(key, end=' ')
```
Результат работы кода:
```
organisation officers type country founded members budget url 
```

Теперь предположим, что у нас есть питоновский словарь или массив, который мы хотим сохранить в виде строки json
```python
d = {"John": 51, "Kate": 12, "Bill": 27}
json_string = json.dumps(d)
print(type(json_string))  # убедимся, что теперь наши данные превратились в строку
print(json_string)  # распечатаем эту строку
```
Результат работы кода:
```
<class 'str'>
{"John": 51, "Kate": 12, "Bill": 27}
```

То же самое можно делать с массивами
```python
arr = ['hello', 'world']
json_string = json.dumps(arr)
print(type(json_string)) 
print(json_string)
```
Результат работы кода:


```
<class 'str'>
["hello", "world"]
```


Убедимся, что **не все** питоновские правильные объекты хорошо вписываются в json
```python
d = {("A", 21): "John"}
json_string = json.dumps(d)
print(json_string)
```
Результат работы кода:

```
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-8-9472bb4c66d1> in <module>
      1 # убедимся, что не все питоновские правильные объекты хорошо вписываются в json
      2 d = {("A", 21): "John"}
----> 3 json_string = json.dumps(d)
      4 print(json_string)

2 frames
/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/json/encoder.py in iterencode(self, o, _one_shot)
    255                 self.key_separator, self.item_separator, self.sort_keys,
    256                 self.skipkeys, _one_shot)
--> 257         return _iterencode(o, 0)
    258 
    259 def _make_iterencode(markers, _default, _encoder, _indent, _floatstr,

TypeError: keys must be str, int, float, bool or None, not tuple
```


## Важные хитрости метода `dump`

Записывая json в файл, можно вставить ещё два необязательных параметра, которые могут быть полезны для последующей работы.

Во-первых, это параметр `indent`, он позволяет сделать так, чтобы данные записывались в файл с человекопонятным форматированием. Тогда файл можно будет открыть текстовым редактором и посмотреть глазами, что там внутри.

Во-вторых, это параметр `ensure_ascii`, он служит в целом для того же. Дело в том, что если в ваших данных есть не-ascii символы, то модуль json по умолчанию кодирует их специальным образом, используя при этом только символы из ограниченного набора, читающиеся одинаково почти во всех кодировках. Это хорошо при переносе данных из одной программы в другую: ничего не собьётся и не потерятся. Но это плохо для человека: понять, что в таком файле, станет невозможно.


Вот такой код просто сбросит словарь в файл:
```python

d = {'абв': 1, 'где': 2, 'ёжз': 3}

with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(d, f)
```


Если заглянуть в файл, то результат будет таким:
```
{"\u0433\u0434\u0435": 2, "\u0430\u0431\u0432": 1, "\u0451\u0436\u0437": 3}
```


Добавим параметр ensure_ascii:
```python
with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(d, f, ensure_ascii = False)
```

Результат работы кода:
```
{"где": 2, "абв": 1, "ёжз": 3}
```

Добавим indent (числовое значение &mdash; это число пробелов в отступах):

```python
with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(d, f, ensure_ascii = False, indent = 4)
```

Результат работы кода:
```
{
    "абв": 1,
    "где": 2,
    "ёжз": 3
}
```
