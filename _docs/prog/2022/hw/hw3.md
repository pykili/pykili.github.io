---
title: HW3
---

# Homework 3

Дедлайн 23:59 10 октября

Для сдачи домашек по питону вам нужно будет создать репозиторий на гитлабе со следующей структурой:

1. Репозиторий должен называться  HSE_python_intro, сделайте его приватным и добавьте в коллабораторы преподавателя и ассистента вашей группы (тут надо погуглить)
2. Внутри него под каждое дз нужно создавать папку hw1, hw2, etc
3. Внутрь папки нужно положить файл с расширением .py, в котором лежит решение домашнего задания

Помните, что мы не загружаем файлики руками, а клонируем репозиторий и работаем с гитом, а также к каждому коммиту пишем понятные комментарии.

Каждую задачу нужно добавлять через отдельный merge request. 

Ссылку на дз нужно вставить в эту гугл форму: [https://forms.gle/JQixZLi6f5kVgcAq7](https://forms.gle/JQixZLi6f5kVgcAq7)

Вам нужно решить 5 задач и положить каждую в отдельный файл.

В каждой задаче пользователя должны спрашивать на вход необходимые данные (тут надо погуглить как в питоне запрашивать ввод с консоли) и на выходе должны появляться понятные ответы (не просто число/строка). Каждая задача оценивается в 2 балла.

Для решения нельзя использовать списки, циклы, срезы и условные операторы.

**Задача 1**

Напишите программу, которая по введенным оценкам за каждую из форм оценивания считает итоговую оценку за курс по нашей формуле оценки. Напомним, что формула оценки есть на нашем сайте и что оценку нужно округлить.

**Задача 2**

Напишите программу, которая запрашивает имя пользователя и желает ему хорошего дня. Убедитесь, что имя, которое вы печатаете, начинается с заглавной буквы и строка, введенная пользователем не содержит висячих пробелов, для этого используйте методы строк. Еще тут нужно погуглить про то как встраивать переменную внутрь строки.

**Задача 3**

Спросите у пользователя его рост в сантиметрах и скажите на сколько целых дюймов он выше, чем дюймовочка и на сколько целых вершков он ниже, чем Петр I.

**Задача 4**

Сделайте калькулятор сдачи. На вход он должна получать 2 числа – цену товара в рублях и сумму, которую дал покупатель. На выход программа должна выдавать целое число рублей и число копеек, которые нужно вернуть покупателю. Помните, что цена товара и сумма данная покупателем могут быть дробными.

**Задача 5**

Напишите программу для прогастролога. На вход она должна получать имя и дату рождения. Нужно возвести длину имени в степень месяца рождения, умножить на сумму чисел в годе рождения и найти остаток от деления на день рождения. Вывести получившееся целое число с загадочным комментарием о том, как это число влияет на способности к программированию.

### Критерии:

Есть часть критериев, которые будут действовать для всех последующих домашек

1. Дз сдано не через GitLab – 0 баллов за все дз
2. Файл .py не запускается (падает с ошибкой) – 0 баллов за задачу/задачи в этом файле
3. Не соблюден pep-8 – минус балл
4. Нет комментариев в коде – минус балл