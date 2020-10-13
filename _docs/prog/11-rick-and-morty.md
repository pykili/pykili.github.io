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


<!-- ====================================================================== -->

{% include_relative hw/hw_11_base/README.md %}

