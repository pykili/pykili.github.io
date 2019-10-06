---
title: 20 &mdash; Тестовый вариант контрольной №2
---

# Тестовый вариант контрольной №2
Для работы программы вам понадобится <a href="/content/test2_mystem.xml">вывод программы для морфологического анализа MyStem в формате XML</a>. Формат разметки описан на <a href="https://tech.yandex.ru/mystem/doc/grammemes-values-docpage/">странице анализатора</a>.

# Задача №1

Открыть XML-файл и посчитать число строк внутри тега `se`, то есть между строкам `<se>` и `</se>`, открыть другой файл и записать туда число подсчитанных строк.

**Возможное решение**

```python
import re


def count_lines_in_se(xml):
    match = re.search(r'^<se>$\n(.*)^</se>$', xml, flags=re.MULTILINE | re.DOTALL)
    lines_inside_se = match.group(1).splitlines()
    return len(lines_inside_se)


def problem1():
    with open('mystem.xml', encoding='utf-8') as fh:
        xml = fh.read()
    answer = count_lines_in_se(xml)
    with open('number_of_lines_inside_se.txt', 'w') as fh:
        fh.write(str(answer))


problem1()
```

# Задача №2

Создать словарь, в котором ключами являются строка с результатом морфологического разбора слова (то есть значения атрибута `gr` для строк, в которых имеется `<w>`), а значениями — количество их вхождений в файле. Распечатать пары ключ-значение из словаря в открытый для записи файл таким образом, чтобы каждая пара располагалась на одной строке и была разделена символом табуляции.

**Возможное решение**


```python
import re
from collections import Counter


def gr_counter(xml):
    gr = re.findall(r'gr="([^"]+)"', xml)
    return Counter(gr)

def problem2():
    with open('mystem.xml', 'r', encoding='utf-8') as fh:
        xml = fh.read()
    counter = gr_counter(xml)
    with open('morphology_counter.tsv', 'w') as fh:
        for gr, count in counter.most_common():
            fh.write(gr)
            fh.write('\t')
            fh.write(str(count))
            fh.write('\n')
```

# Задача №3

С помощью регулярных выражений найти все вхождения прилагательных женского года (то есть таких, для которых в `gr=...` входит запись «A», а также указан род «жен»). Соответствующие этим характеристикам словоформы распечатать в новый файл в одну строку через запятую.

Преобразуйте содержимое корпуса в формат csv. Возьмите строки внутри тега <body> и с помощью re.sub очистите их от тегов. Запишите результат в новый файл следующим образом: на одной строке должны находиться лемма, разбор, словоформа, разделённые точкой с запятой. Пунктуацию и служебную информацию можно удалить.

**Возможное решение**

```python
import re


def feminine_adj(xml):
    words = re.findall(
        r'^.+gr="A=[^"]*жен[^"]*" />(\w+)</w>$',
        xml,
        flags=re.MULTILINE
    )
    return words


def problem3_part1(xml):
    words = feminine_adj(xml)
    with open('feminine_adj.txt', 'w') as fh:
        fh.write(','.join(words))


def parse_xml(xml):
    match = re.search(r'<body>(.+)</body>', xml, flags=re.DOTALL)
    body = match.group(1)
    rows = []
    for line in body.splitlines():
        match = re.search(
            r'^<w><ana lex="([^"]+)" gr="([^"]+)" />(\w+)</w>$',
            line
        )
        if match:
            rows.append(match.groups())
    return rows


def problem3_part2(xml):
    rows = parse_xml(xml)
    with open('mystem.csv', 'w') as fh:
        for row in rows:
            fh.write(';'.join(row))
            fh.write('\n')


def problem3()
    with open('mystem.xml', 'r', encoding='utf-8') as fh:
        xml = fh.read()
    problem3_part1(xml)
    problem3_part2(xml)
```
