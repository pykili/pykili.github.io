---
title: 11 &mdash; Разбор диалогов «Рика и Морти»
---

# План этого занятия
* Изучение лексикона персонажей сериала «Рик и Морти»


# Задача на семинар

Мы будем работать с базой данных реплик, произнесённых в сериале «Рик и Морти».
Скачайте данные из репозитория [github.com/techcentaur/The-GrandFather](https://github.com/techcentaur/The-GrandFather), папку `dataset` положите в папку с программой.
Ваша задача — написать программу, которая будет считать сколько раз было произнесено какое-то слово данным персонажем.
Программа должна спросить слово и имя персонажа у пользователя, притом, если в качестве имени введено специальное слово «all», то считать реплики всех персонажей вместе.
Обязательно используйте функции!

# Пример решения

```python
#!/usr/bin/env python3


PUNCT_CHARS = """,.:;"'`()-"""
PUNCT_CHARS_WITH_SPACES = (' -', '- ',)
SEASONS = (
    tuple(range(1, 11)),
    (1, 2, 3, 4, 6, 8, 10,),
    (1, 2, 3, 7,),
)


def get_words(text):
    text = text.lower()
    for char in PUNCT_CHARS_WITH_SPACES:
        text = text.replace(char, '')
    words = text.split()
    for i, w in enumerate(words):
        words[i] = w.strip(PUNCT_CHARS)
    return words


def produce_file_list(prefix, sufix, episodes):
    files = []
    for i in episodes:
        file = prefix + str(i) + sufix
        files.append(file)
    return files


def get_episode_list(seasons):
    files = []
    for i, episodes in enumerate(seasons):
        season = i + 1
        prefix = './dataset/season' + str(season) + '/episode'
        files.extend(produce_file_list(prefix, '.txt', episodes))
    return files


def remove_remarks(text):
    while True:
        idx_left = text.find('(')
        idx_right = text.find(')')
        if idx_left == -1 or idx_right == -1:
            break
        text = text[:idx_left] + text[idx_right + 1:]
    return text


def parse_episode(episode, character=None):
    cues = []
    name = ''
    with open(episode, encoding='utf-8') as fh:
        for line in f:
            line = line.strip()
            # if not line:
            if line == '':
                continue
            if ((line.startswith('[') or line.startswith('('))
                    and (line.endswith(']') or line.endswith(')'))):
                continue
            splitted_line = line.split(': ', maxsplit=1)
            if len(splitted_line) == 2:
                name = splitted_line[0]
            text = splitted_line[-1]
            text = remove_remarks(text)
            if character is None or character in name:
                cues.append(text)
    return cues


def ask_user(what):
    while True:
        character = input(what)
        if character:
            return character
        print('Say something')


def count_word(word, text):
    words = get_words(text)
    x = words.count(word)
    return x


def print_result(name, word, count):
    print(
        'Character',
        name,
        'says word',
        word,
        count,
        'times',
    )


def main():
    episodes = get_episode_list(SEASONS)
    char = character = ask_user('Input character name, "all" for everyone: ')
    print('Working...')
    if character.lower() == 'all':
        char = None
    cues = []
    for file in episodes:
        cues.extend(parse_episode(file, char))
    text = '\n'.join(cues)
    word = ask_user('Input word to count: ')
    count = count_word(word, text)
    print_result(character, word, count)


if __name__ == '__main__':
    main()

```


# Домашнее задание

Вам нужно написать программу, порождающую случайные грамматически правильные, но бессмысленные тексты.  

Вместо тысячи объяснений прикладываем два образца программ: [poem_generator.py](https://github.com/pykili/pykili.github.io/tree/master/py/poem_generator.py) порождает стихотворение на русском языке, написанное трехстопным анапестом, [sentence_generator.py](https://github.com/pykili/pykili.github.io/blob/master/py/sentence_generator.py) порождает текст на английском языке. Посмотрев программы, вы поймёте, как такая программа должна быть устроена и какие в ней должны быть функции.  Функции, естественно, не обязательно делать ровно такими же, как в этой программе (и даже желательно сделать их как-нибудь по-своему), но каждая ваша функция должна делать какую-то одну определённую операцию — таким образом, функций у вас в программе должно быть не одна и не две. 

Для работы вам понадобится функция выбора случайного элемента из списка. Чтобы её использовать, в самом начале программы нужно написать

```python
import random
```

— это указание питону использовать модуль [random](https://docs.python.org/3/library/random.html), в котором хранится нужная функция. А сама функция называется `random.choice()`. Если у Вас есть список array, то после строчки

```python
x = random.choice(array)
```

`x` станет равен какому-то случайно выбранному элементу этого списка. В программе `sentence_generator.py` также используется функция `random.randint(x, y)`, которая выбирает случайное число от `x` до `y`.

Слова для текстов должны быть взяты из отдельных txt-файлов, открываемых программой. Эти файлы следует загрузить в репозиторий вместе с кодом выполненного домашнего задания. Также желательно в функции уложить вообще весь код.  Дополнительные ограничения по вариантам:  

1. Текст должен представлять собой правильное хайку, то есть стихотворение на русском языке из трёх строк без рифмы, при этом длина первой строки должна быть 5 слогов, второй строки - 7 слогов, третьей строки - 5 слогов.  Количество предложений в таком тексте может быть любым. 

2. Текст должен представлять собой стихотворение на русском языке из четырёх строк без рифмы, но написанное с соблюдением одной метрической схемы, кроме трёхстопного анапеста. 

3. Текст должен состоять из 5 предложений разных типов (утвердительные, вопросительные, отрицательные, условные, побудительные) на русском языке. Типы предложений должны выводиться в случайном порядке. 

4. Текст должен представлять собой правильное хайку, то есть стихотворение на одном из изучаемых вами языков (французский, итальянский, японский, немецкий, персидский) из трёх строк без рифмы, при этом длина первой строки должна быть 5 слогов, второй строки - 7 слогов, третьей строки - 5 слогов. Количество предложений в таком тексте может быть любым. 

5. Текст должен быть не короче, чем 5 предложений на русском языке. Предложения должны быть сложносочинёнными и сложноподчинёнными. 

6. Текст должен состоять из 5 предложений разных типов (утвердительные, вопросительные, отрицательные, условные, побудительные) на изучаемом вами языке (французский, испанский). Типы предложений должны выводиться в случайном порядке. 

7. Текст должен представлять собой танка, то есть стихотворение на русском языке из пяти строк без рифмы, при этом длина первой строки должна быть 5 слогов, второй строки - 7 слогов, третьей строки - 5 слогов, четвёртая и пятая строка - по 7 слогов. Количество предложений в таком тексте может быть любым. 


**Выполните задание пройдя по ссылке в GitHub Classroom:**

- 1 группа: дедлайн - 25 января 2019 23:55 <https://classroom.github.com/a/MQ_QKjjs>
- 2 группа: дедлайн - 20 января 2019 23:55 <https://classroom.github.com/a/ZzPT5P09>
- 3 группа: дедлайн - 20 января 2019 23:55 <https://classroom.github.com/a/x5WOG9za>
- 4 группа: дедлайн - 22 января 2019 23:55 <https://classroom.github.com/a/3TXLSuld>
