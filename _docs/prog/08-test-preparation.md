---
title: 9 &mdash; Задачи для подготовки к контрольной работе №1
---

# Задачи для подготовки к контрольной работе №1


## Задача №1

Напишите функцию, которая принимает на вход список чисел и возвращает
среднее этих чисел.

**Рассуждения:**

1. Среднее в математике - сумма чисел делить на количество
2. Если список пустой, то количество чисел - 0, а на ноль делить нельзя.
   Нужно это обработать.

**Решение:**

```python
def avg(numbers):
    if not numbers:
        return 0
    return sum(numbers) / len(numbers)
```


## Задача №2

Найдите среди всех слов из частотного словаря слово,
которое имеет самую близкую к средней частоте частоту.

Оформите решение в виде нескольких функций.

Частотный словарь можно скачать по
[ссылке](https://gist.githubusercontent.com/Sapunov/22d060fe952eca5a347e8f105ebe7a42/raw/9595d0cf0276d972775ce6ce899c2299c492468a/freq_dict.txt)

**Рассуждения**

1. Сперва нам нужно прочитать файл с частотным словарем. Так как в задаче просят работать только со словами и их частотами, преобразуем содержимое файла в 2 списка - список слов и список частот.
2. Напишем функцию, которая по списку частот вычислит среднюю частоту.
3. Напишем функцию, которая вычислит индекс слова, которое имеет наиболее близкую частоту к средней.

**Решение:**

```python
def read_dict(filename):
    words = []
    freqs = []
    with open(filename, encoding='utf-8') as file:
        for line in file:
            word, _, freq = line.split('|')
            freq = float(freq)
            words.append(word)
            freqs.append(freq)
    return words, freqs


def avg(numbers):
    if not numbers:
        return 0
    return sum(numbers) / len(numbers)


def find_closest(freqs, avg_freq):
    index = -1
    min_dist = 1000000 # это больше, чем любое значение из списка freqs
    for i in range(len(freqs)):
        dist = abs(freqs[i] - avg_freq)
        if dist < min_dist:
            index = i
            min_dist = dist
    return index
```

А вот так можно воспользоваться написанными функциями:

```python
>>> words, freqs = read_dict('freq_dict.txt')
>>> closest = find_closest(freqs, avg(freqs))
>>> print('Word found:', words[closest], freqs[closest])
Word found: велосипед 28.34
```

## Задача №3


Вам дан частотный словарь слов русского языка.
Частотный словарь можно скачать по
[ссылке](https://gist.githubusercontent.com/Sapunov/22d060fe952eca5a347e8f105ebe7a42/raw/9595d0cf0276d972775ce6ce899c2299c492468a/freq_dict.txt)

Реализуйте функцию `retrieve(filename, lte=None, gte=None, gender=None)`.

Функция должна проходить по всем строкам файла `filename` и отбирать те слова вместе с частотой и морфологической информацией, которые подпадают под критерии.

- `lte` - частота слова должна быть меньше или равна значению `lte`. Если `lte` равен `None`, то этот параметр фильтрации учитывать не нужно.
- `gte` - частота слова должна быть больше или равна значения `gte`. Если `gte` не указан (то есть равен `None`), то этот параметр фильтрации учитывать ну нужно.
- `gender` - слово должно принадлежать указанному в `gender` роду. Если `gender` не указано (то есть равно `None`), то учитывать этот параметр при фильтрации не нужно.

**Примеры**

Если в файл содержит следующие строки:

```
абитуриент|сущ одуш ед муж им|1.47
абонемент|сущ неод ед муж им|1.1
абонент|сущ одуш ед муж им|3.43
абориген|сущ одуш ед муж им|10.53
аборт|сущ неод ед муж им|9.79
абразив|сущ неод ед муж им|4.35
абракадабра|сущ неод ед жен им|1.22
абрек|сущ одуш ед муж им|3.86
абрикос|сущ неод ед муж им|4.84
абрикосовый|прл ед муж им|1.53
абсолют|сущ неод ед муж им|1.35
```

То функция должна работать следующим образом:

```python
# Без аргументов

retrieve(filename)
[('абитуриент', 'сущ одуш ед муж им', 1.47),
 ('абонемент', 'сущ неод ед муж им', 1.1),
 ('абонент', 'сущ одуш ед муж им', 3.43),
 ('абориген', 'сущ одуш ед муж им', 10.53),
 ('аборт', 'сущ неод ед муж им', 9.79),
 ('абразив', 'сущ неод ед муж им', 4.35),
 ('абракадабра', 'сущ неод ед жен им', 1.22),
 ('абрек', 'сущ одуш ед муж им', 3.86),
 ('абрикос', 'сущ неод ед муж им', 4.84),
 ('абрикосовый', 'прл ед муж им', 1.53),
 ('абсолют', 'сущ неод ед муж им', 1.35)]
```

```python
# Только женский род

retrieve(filename, gender='жен')
[('абракадабра', 'сущ неод ед жен им', 1.22)]
```

```python
# Мужской рож с частотой больше 5

retrieve(s, gender='муж', gte=5)
[('абориген', 'сущ одуш ед муж им', 10.53),
 ('аборт', 'сущ неод ед муж им', 9.79)]
```

Обратите внимание на то, что возвращает функция - это список кортежей, состоящих каждый из 3 элементов.

**Решение:**

```python
def retrieve(filename, lte=None, gte=None, gender=None):
    output = []
    with open(filename, encoding='utf-8') as file:
        for line in file:
            word, morph, freq = line.split('|')
            freq = float(freq)
            if lte is not None and freq > lte:
                continue
            if gte is not None and freq < gte:
                continue
            if gender is not None and gender not in morph:
                continue
            output.append((word, morph, freq))
    return output
```

## Задача №4

Напишите функцию, которая принимает два аргумента:
первый -- произвольная строка,
второй -- опциональный (по умолчанию равен пустой строке)

Второй аргумент может принимать значения `"longest"`, `"alphanumerically_last"`, `"same_vowels"`.

Если значение второго аргумента -- `"longest"`, нужно найти в строке -- первом аргументе -- максимальную последовательность из одинаковых букв и вернуть её.

Если значение второго аргумента -- `"alphanumerically_last"`, нужно найти в строке -- первом аргументе -- символ, порядковый номер которого в алфавите больше, чем у всех остальных, и вернуть его.

Если значение второго аргумента -- `"same_vowels"`, нужно проверить, есть ли в строке -- первом аргументе -- последовательность символов, среди которых несколько гласных, причём все гласные одинаковы. Если такая последовательность есть, нужно вернуть `True`, иначе -- `False`.

Если значение второго аргумента -- пустая строка, то нужно выдать в ответ тройку -- результат для аргумента `"longest"`, результат для аргумента `"alphanumerically_last"`, результат для аргумента `"same_vowels"`.

```python
def longest(my_str):
    if not my_str:
        return ''

    maximal_subseq_found = my_str[0]

    for letter_pos, letter in enumerate(my_str[:-1]):
        subseq = ''
        for subseq_letter in my_str[letter_pos:]:
            if subseq_letter == letter:
                subseq += letter
            else:
                break
        if len(subseq) > len(maximal_subseq_found):
            maximal_subseq_found = subseq

    return maximal_subseq_found


def alphanumerically_last(my_str):
    if not my_str:
        return ''

    alphabet = "абвгдеёжзийклмнопрстуфхцчшщъыьэюя"

    for letter in my_str:
        if letter.isalpha() and letter.lower() not in alphabet:
            print(f"ERROR! passed string {my_str} with unknown letter {letter}")
            return ''

    alnum_last_letter = my_str[0]
    for letter in my_str:
        if not letter.isalpha():
            continue
        if alphabet.index(letter.lower()) > alphabet.index(alnum_last_letter.lower()):
            alnum_last_letter = letter

    return alnum_last_letter


def contains_same_vowels(my_str):
    if not my_str:
        return False

    vowels = "аеёиоуыэюя"

    my_str_vowels = ''
    for letter in my_str:
        if letter.lower() in vowels:
            my_str_vowels += letter.lower()

    harmonic_subsequence_present = False
    for idx in range(1, len(my_str_vowels[1:])):
        vowel = my_str_vowels[idx]
        if my_str_vowels[idx - 1] == vowel:
            harmonic_subsequence_present = True
            break

    return harmonic_subsequence_present



def foo(my_str, my_param=''):

    if my_param == "longest":
        return longest(my_str)
    elif my_param == "alphanumerically_last":
        return alphanumerically_last(my_str)
    elif my_param == "same_vowels":
        return contains_same_vowels(my_str)
    elif my_param == "":
        return longest(my_str), alphanumerically_last(my_str), contains_same_vowels(my_str)


print(foo("парам пам пурум"))
print(foo("парам пам пурум", "longest"))
print(foo("парам пам пурум", "alphanumerically_last"))
print(foo("парам пам пурум", "same_vowels"))

print(foo("кот ехал ехал и уехал"))
print(foo("ПЫТАЙТЕСБ ЧТОТ0 N3МЕНИТЬ!"))
print(foo(""))
```

## Задача №5
Реализовать функции `my_append`, `my_extend`, `my_remove`.

`my_append` принимает на вход список и элемент произволной природы, возвращает список, равный исходному, продолженному переданным элементом. (Короче, то, что бы сделал аппенд этого элемента к списку) Использовать метод `append` и `extend` нельзя

`my_extend` принимает на вход два списка и возвращает список, полученный конкатенацией переданных. Использовать `extend` нельзя, за использование `append` штраф 1 балл.

`my_remove` принимает на вход список и число, *i* число может быть позицией любого элемента в списке. Возвращает копию исходного списка, из которой удалён *i*-тый элемент. Использовать стандартные методы удаления элементов из списка нельзя.

```python
def my_append(my_li, elem):
    result_li = [elem] * (len(my_li) + 1)

    for my_elem_idx, my_elem in enumerate(my_li):
        result_li[my_elem_idx] = my_elem

    return result_li

def my_extend(my_li1, my_li2):
    result_li = my_li1

    for elem in my_li2:
        result_li = my_append(result_li, elem)

    return result_li

def my_remove(my_li, my_idx):
    result_li = my_extend(my_li[:my_idx], my_li[my_idx+1:])
    return result_li

print(my_append([2,3], True))
print(my_append([2], True))
print(my_append([], True))
print(my_append([], []))

print(my_extend([2,3], [4,5]))
print(my_extend([2], [3, 4,5]))
print(my_extend([], [2,3,4,5]))
print(my_extend([2,3,4,5], []))
print(my_extend([[]], [[4,5]]))
print(my_extend([], []))

print(my_remove([1,2,3,4], 0))
print(my_remove([1,2,3,4], 1))
print(my_remove([1,2,3,4], 3))
print(my_remove([1,2,3,4], 6))
print(my_remove([1,2,3,4], -8))
print(my_remove([], 1))
```

## Задача №6

Напишите функцию, принимающую на вход имя файла (это будет имя файла с субтитрами к Рику и МОрти, скачать для тренировки можно [тут](https://raw.githubusercontent.com/oserikov/python_13112019/master/data/series/lol_just_another_folder/rick_and_morty/all_subtitles.txt) ).
Функция должна записать два файла -- один с репликами рика,  другой -- с репликами морти. имена созданных файлов с объяснением, какой про что, нужно вывести на экран.
За удаление одного-двух ругательств из реплик добавляется дополнительный балл.

```python
def foo(src_filename, blacklist = ["fuck", "morning"]):
    rick_fn = "rick.txt"
    morty_fn = "morty.txt"
    with open(src_filename, 'r', encoding="utf-8") as src_f:
        rick_f = open(rick_fn, 'w', encoding="utf-8")
        morty_f = open(morty_fn, 'w', encoding="utf-8")

        for line in src_f:

            line_masked = line
            for blacklisted_word in blacklist:
                line_masked = line_masked.replace(blacklisted_word,
                                                  "*" * len(blacklisted_word))

            if line.startswith("rick:"):
                rick_f.write(line_masked)
            elif line.startswith("morty:"):
                morty_f.write(line_masked)

        rick_f.close()
        morty_f.close()

    print(f"morty replics written to {morty_fn}")
    print(f"rick replics written to {rick_fn}")

# change the path to the one that fits for you
foo("all_subtitles.txt")

```


