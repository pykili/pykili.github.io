---
title: 8 &mdash; Тестовый вариант контрольной №1
---

# Тестовый вариант контрольной №1

Для работы нужно использовать <http://web-corpora.net/Test1_2016/quotes.txt>. В файле на каждой строчке записана цитата, затем тире "—", а затем источник цитаты.

Перед тем, как начать формулировать задачи и разрабатывать решения, определим 2 функции. Эти функции будут использоваться во всех задачах.

```python
def clear_text(text):
    '''Функция очищает текст от 'мусорных' символов слева и справа от text
    '''
    trash_symbols = '!"#$%&\'-()*+,./:;<=>?@[\\]^_`{|}~«»—'

    return text.strip(trash_symbols)
```

```python
def get_words(string_of_text):
    '''Преобразует текст в набор токенов.
        Токеном является слово, очищенное от любых символов, кроме букв.
    '''

    good_words = []

    for word in string_of_text.split():
        candidate = clear_text(word)
        if candidate != '':
            candidate = candidate.lower()
            good_words.append(candidate)

    return good_words
```

# Задача №1

Программа должна открыть файл с цитатами в кодировке UTF-8 и корректно вывести на экран цитаты, в которых менее 10 слов.

**Возможное решение**

```python
FILENAME = 'quotes.txt'
DASH = '—'

with open(FILENAME, encoding='utf-8') as fid:
    for line in fid:
        quote, author = line.split(DASH, maxsplit=1)
        quote_words = get_words(quote)

        if len(quote_words) < 10:
            print(quote)
```

# Задача №2

Программа должна сказать, в скольких цитатах есть слово "разум", а затем распечатать источники этих цитат через запятую.

**Возможное решение**

```python
FILENAME = 'quotes.txt'
DASH = '—'
MIND_WORD = 'разум'

mind_authors = []

with open(FILENAME, encoding='utf-8') as fid:
    for line in fid:
        quote, author = line.split(DASH, maxsplit=1)
        quote_words = get_words(quote)

        if MIND_WORD in quote_words:
            mind_authors.append(author.strip())

print('Слово разум притствует в', len(mind_authors), 'цитате(ах)')
print(', '.join(mind_authors))
```

# Задача №3

Программа должна спрашивать у пользователя слова, пока тот не введёт пустое слово. После этого она должна для каждого слова распечатать

- само это слово
- все цитаты, в которых оно встречается.

Если слово ни разу не встретилось, то вместо цитат нужно вывести сообщение о том, что цитат не нашлось.

**Возможное решение**

```python
FILENAME = 'quotes.txt'
DASH = '—'

def find_quotes(input_word, quotes, quotes_words):

    results = []

    for i, quote_words in enumerate(quotes_words):
        if input_word in quote_words:
            results.append(quotes[i])

    return results

quotes = [] # список цитат из файла
quotes_words = [] # список списков слов из каждой цитаты

# Формируем "поисковый индекс" по которому будем искать
#
with open(FILENAME, encoding='utf-8') as fid:
    for line in fid:
        line = line.strip()
        quote, author = line.split(DASH, maxsplit=1)
        quote_words = get_words(quote)

        quotes.append(line) # добавим всю цитату с автором
        quotes_words.append(quote_words)

# Запрашиваем у пользователя "поисковый запрос"
#
while True:
    word = input('Введите слово: ')
    if word == '':
        break

    word = word.strip().lower()
    found_quotes = find_quotes(word, quotes, quotes_words)

    if len(found_quotes) == 0:
        print('Цитат не нашлось')
    else:
        print('Вы ввели:', word)
        for quote in found_quotes:
            print(quote)
```
