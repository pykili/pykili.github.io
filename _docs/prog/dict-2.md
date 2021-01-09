---
title: 13 &mdash; Вложенные словари
---

# План этого занятия
* вложенные словари
* `dict.setdefault`
* `defaultdict` и lists comprehension
* `DictReader`

# Вложенные словари

В качестве значения ключа словаря может выступать любой объект: число, строка, список или словарь.
Вложенные коллекции часто используются для структурирования данных.
Рассмотрим пример: словарь из списков, описывающий содержание пеналов учеников класса 1б:

```python
>>> dict_of_lists = {
... 'Ксюша': ['ручка', 'ручка', 'карандаш', 'ластик',],
... 'Миша': ['ручка', 'мелок', 'линейка',],
... }
```

Используя такую структуру очень просто отвечать на вопросы типа: «Что в пенале у Миши?», «У кого больше всего предметов в пенале?»:

```python
>>> print('В пенале у Миши:', ', '.join(dict_of_lists['Миша']))
В пенале у Миши: ручка, мелок, линейка
>>> max_count = -1
>>> rich_student = None
>>> for student, case in dict_of_lists.items():
...     count = len(case)
...     if count > max_count:
...         rich_student = student
...         max_count = count
>>> print('Самый набитый пенал у ученика по имени', rich_student)
Самый набитый пенал у ученика по имени Ксюша
```

Если нам не важен порядок предметов, уложенных в пинал, но важно уметь быстро и удобно получить число предметов данного типа, то удобнее представить пенал в виде словаря, где ключом будет название предмета, а значением — его количество.
В качестве такого словаря будем использовать специальную разновидность словарей из стандартной библиотеки — `collections.Counter` ([см. предыдущий семинар](/13/)).

```python
>>> import collections
>>> dict_of_counters = {}
>>> for student, case in dict_of_lists.items():
...     dict_of_counters[student] = collections.Counter(case)
```

Теперь мы можем очень просто выяснить ответы на другие вопросы, например: «Какого предмета больше всего у Ксюши?» или «Сколько ластиков у Миши?».

```python
>>> print('Самый многочисленный предмет в пенале Ксюши — это',
...       dict_of_counters['Ксюша'].most_common(1)[0][0])
Самый многочисленный предмет в пенале Ксюши — это ручка
>>> print('Количество ластиков у Миши:', dict_of_counters['Миша']['ластик'])
Количество ластиков у Миши: 0
```

Обратите внимание, что для несуществующих в `Counter` ключей значение равно нулю.

Предположим, что мы хотим добавить ещё какой-то предмет в чей-то пенал, указав имя ученика и название предмета.

```python
>>> dict_of_counters['Ксюша']['ластик'] += 1
>>> dict_of_counters['Ксюша']['фломастер'] += 1
```

Обратите внимание, что для несуществующих в `Counter` ключей оператор `+=` тоже работает.

Усложним задачу, пусть теперь, если используется имя ученика, которого ещё нет в словаре, то его нужно добавить.
Ниже приводится несколько реализаций функции, которая делает такое обновление словаря, в порядке улучшения качества кода.

```python
def add_item1(d, student, item):
    if student in d:
        d[student][item] += 1
    else:
        d[student][item] = collections.Counter([item])
        # или, аналогично:
        # d[student][item] = collections.Counter({item: 1})


def add_item2(d, student, item):
    if student not in d:
        d[student] = collections.Counter()
    d[student][item] += 1


def add_item3(d, student, item):
    case = d.setdefault(student, collections.Counter())
    case[item] += 1
```

В последнем примере используется метод словаря `.setdefault(key, default)`.
Этот метод возвращает значение словаря по ключу `key`, а если такого ключа ещё нет, то добавляет в словарь пару `key: default` и возвращает `default`.

## collections.defaultdict

### Интуиция
`defaultdict` &mdash; это такой словарь, который старается сам правильно обработать отсутствие ключа в словаре.

Обычно мы сообщаем ему, какого типа должно по умолчанию быть значение, соответствующее ключу.

Зная поумолчальный тип значений, `defaultdict`, будет заводить тривиальное значение этого типа каждый раз, когда мы будем обращаться к ключу, которого на момент обращения нет в словаре.

В таком случае мы не получим ошибку `KeyError (такого ключа в словаре нет)`, а сразу увидим значение, о котором подсуетился `defaultdict`.

> **тривиальное значение** какого-то типа тут -- что-то максимально простое для этого типа.
Для целых чисел -- 0, для нецелых -- 0.0, для строк -- пустая строка, для списков -- пустой список.

### Пример использования
`defaultdict` обычно нужен там, где для обычного словаря вам пришлось бы писать

```python
my_dict = {}
...
if cat not in my_dict.keys():
  my_dict[cat] = ''  # или 0.0 или 0 или []
else:
  my_dict[cat] += mlem  # или .append(mlem)
```

вместо этого можно написать

```python
from collections import defaultdict as dd
# погуглите import as если забыли в чём суть as

my_dict = dd(str)
...
my_dict[cat] += mlem  # обработка отсутствия кота в ключах произошла автоматически

```


## lists comprehension (демо-версия)
это удобный способ сокращать создание или заполнение списков.
Он иногда будет появляться в примерах для экономии места, поэтому честно о нём рассказать.


### О дебаггинге lists comprehension
В одной строке оказываются несколько разных действий. Поэтому
информация о том, что ошибка произошла в строке с list comprehension, более неоднозначна, чем то же про обычную строку.

Если что-то не работает в строке с list comprehension, сразу переписывайте её без list comprehension и чините логику в "развёрнутом" виде, а потом сворачивайте обратно когда убедитесь, что с логикой всё ок.

### Основная идея

это
```python
li = []
for elem in [1,2,3]:
  li.append(elem)
```

можно записать так:
```python
li = [elem for elem in [1,2,3]]
```

---
вложенность обрабатывается слева направо:
```python
def cond(smth):
  res = isinstance(smth, int) and smth % 2 == 0
  return res

li = []
for i in [0,1,2]:
  for j in [3,4]:
    if cond(i*j):
      li.append(i*j)

li2 = [i*j for i in [0,1,2] for j in [3,4] if cond(i*j)]

# имхо более читаемо как в li3
li3 = [i*j
       for i in [0,1,2]
       for j in [3,4]
       if cond(i*j)]
```

## csv.DictReader
Отличный способ читать csv.
Авторский конспект на подходе, временно [ссылка на подходящий](https://courses.cs.washington.edu/courses/cse140/13wi/csv-parsing.html).




# Домашнее задание
**больше, чем обычно, и на подольше**

Вам нужно написать программу, которая загадывает персонажей "Звёздных войн". Загадав персонажа, программа показывает подсказку в виде частотного биграммного словосочетания из реплик этого персонажа, и ждёт ответа пользователя, после чего сообщает, угадал он или нет. Например, если загадан персонаж «THREEPIO», можно показать подсказку «Master Luke».
Реплики персонажей нужно брать из сценариев ЗВ, [ссылка на страницу датасета сценариев](https://www.kaggle.com/xvivancos/star-wars-movie-scripts).


Пользователь может попросить подсказку. Тогда нужно выдать в ответ какую-то (если есть) информацию о загаданном персонаже из датасета2 [ссылка на страницу датасета базы знаний ЗВ](https://www.kaggle.com/jsphyg/star-wars).


В задании обязательно использовать словарь. Когда читаете csv, используйте DictReader.


Дополнительные свойства программы по вариантам:

1. После каждой попытки угадать персонажа вероятность того, что пользователю разрешена ещё одна попытка, обратно пропорциональна частоте реплик персонажа -- можно поиграть с конкретной математикой, но отгадывая редких персонажей должно быть проще получить доп. попытки [ссылка на SO](https://stackoverflow.com/questions/5886987/true-or-false-output-based-on-a-probability);
2. Пользователь может выбрать подмножество персонажей, одного из которых загадает программа -- нужно спросить список интересных персонажей в начале работы программы;
3. Пользователю за раз показывается столько реплик персонажа, сколько в его имени букв по модулю 4;
4. Вместе с репликой подсказывается, из какого сезона взята реплика;
5. Вместе с репликой подсказывается, чьи реплики были в сценарии до и после показанной;
6. Программа показывает не биграммы, а триграммы;
7. В время ([см. либу datetime](https://pythonworld.ru/moduli/modul-datetime.html)) с полуночи до часу ночи программа сообщает пользователю о том, что он выиграл или проиграл, напечатав имя персонажа ASCII-артом (генерировать его не надо, можно захардкодить).