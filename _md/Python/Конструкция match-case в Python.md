2024-09-03
#python 
[Конструкция match-case в Python (pythonist.ru)](https://pythonist.ru/konstrukcziya-match-case-v-python-polnoe-rukovodstvo/?ysclid=lz9z0p50c7200513993)


Начиная с версии 3.10 в языке Python наконец-то появилась конструкция switch-case, которая называется match-case.

С помощью выражения match-case можно избавиться от довольно громоздких цепочек if-elif-else, например:

http_status = 400

if http_status == 400:

print("Bad Request")

elif http_status == 403:

print("Forbidden")

elif http_status == 404:

print("Not Found")

else:

print("Other")

Вместо этого можно использовать компактное выражение match-case:

http_status = 400

match http_status:

case 400:

print("Bad Request")

case 403:

print("Forbidden")

case 404:

print("Not Found")

case _:

print("Other")

Во многих случаях последний вариант гораздо лучше. Он делает код более читаемым и менее повторяемым.

В данной статье мы полностью опишем конструкцию match-case в Python. Также мы рассмотрим распространенные проблемы с операторами if-else и варианты их решения при помощи конструкции match-case. И, наконец, разберем 5 ситуаций, в которых можно использовать операторы match-case.

**От редакции Pythonist:** вам также может быть интересна статья [«Используем словари как альтернативу конструкции if-else»](https://pythonist.ru/ispolzuem-slovari-kak-alternativu-konstrukczii-if-else/).

Итак, приступим к делу!

## Проблема операторов if-else

В принципе, операторы if-else не всегда являются наиболее интуитивным способом сравнения, и речь не только о Python. Это особенно верно, когда блоки if-else повторяются и формируют длинные цепочки.

Рассмотрим пример на псевдокоде, где при помощи операторов if-else проверяется день недели:

day = "Monday"

if day == "Sunday" {

print("Take it easy")

}

else if day == "Monday" {

print("Go to work")

}

else if day == "Tuesday" {

print("Work + Hobbies")

}

else if day == "Wednesday" {

print("Meetings")

}

else if day == "Thursday" {

print("Presentations")

}

else if day == "Friday" {

print("Interviews and party")

}

else if day == "Saturday" {

print("Time to do sports")

Как видите, в этом фрагменте кода много повторений.

Проверка `day == "something"` повторяется многократно. И совершенно ясно, что при каждой проверке мы обращаемся к переменной `day`. Если бы можно было не повторять `day == "something"`, код стал бы чище и короче.

Избавиться от подобных повторов позволяет switch-case. Это решение есть в большинстве популярных языков программирования.

## Решение проблемы — конструкция switch-case

Конструкция switch-case делает структуру сравнения более плавной.

Используя switch-case, вы указываете интересующее вас значение и задаете шаблоны (case) для каждого возможного результата. Затем код пытается сопоставить значение с шаблонами.

Использование оператора switch-case помогает избежать повторений и делает код чище.

Для примера заменим if-else на switch-case в нашем примере псевдокода :

day = "Monday"

switch day {

case "Sunday" : print("Take it easy")

case "Monday" : print("Go to work")

case "Tuesday" : print("Work + Hobbies")

case "Wednesday": print("Meetings")

case "Thursday" : print("Presentations")

case "Friday" : print("Interviews and party")

case "Saturday" : print("Time to do sports")

Это выглядит намного чище и короче, чем куча if-else в примере в предыдущем разделе.

Switch-case часто встречается в популярных языках программирования, например в C++.

Python 3.10 также поддерживает конструкцию switch-case. Но она носит другое название — match-case.

Давайте теперь подробно разберем эти операторы именно в языке Python.

## match-case — Python-версия switch-case

Итак, начиная с версии 3.10 язык Python начинает поддерживать конструкцию switch-case. Напоминаем, что в Python она называется match-case.

Оператор match-case также известен как оператор структурного сопоставления с заданным шаблоном.

Задача, решаемая этим оператором, описана в предыдущем разделе. Короче говоря, он заменяет повторяющиеся операторы if-else компактной структурой сравнения с шаблоном.

### match-case в Python

Общая структура match-case в Python имеет следующий синтаксис:

match element:

case pattern1:

# statements

case pattern2:

# statements

case pattern3:

# statements

В этой конструкции кода:

- **match element** означает «сопоставьте элемент со следующими шаблонами»
- затем каждое выражение **case pattern** сравнивает элемент с указанным шаблоном (это может быть, например, строка или число)
- если шаблон соответствует элементу, выполняется соответствующий блок кода, после этого оператор match-case заканчивает свою работу.

В общем, все то же самое, что и с применением оператора if-elif-else, но с меньшим количеством повторений.

### Пример

Давайте теперь реализуем на Python пример с днями недели, который до этого был написан на псевдокоде. Обычный код с if-else будет выглядеть так:

day = "Monday"

if day == "Sunday":

print("Take it easy")

elif day == "Monday":

print("Go to work")

elif day == "Tuesday":

print("Work + Hobbies")

elif day == "Wednesday":

print("Meetings")

elif day == "Thursday":

print("Presentations")

elif day == "Friday":

print("Interviews and party")

elif day == "Saturday":

print("Time to do sports")

Этот фрагмент кода работает, но выглядит так себе. С оператором match-case в Python 3.10 можно написать красивее:

day = "Monday"

match day:

case "Sunday" : print("Take it easy")

case "Monday" : print("Go to work")

case "Tuesday" : print("Work + Hobbies")

case "Wednesday" : print("Meetings")

case "Thursday" : print("Presentations")

case "Friday" : print("Interviews and party")

case "Saturday" : print("Time to do sports")

Не правда ли, смотрится лучше? Как только вы адаптируетесь к чтению такой структуры, данный код будет казаться вам куда читабельнее предыдущего.

Кстати, вы можете разбить выражения `case` на несколько строк, если это более удобно.

Вот наш код, где каждое выражение находится в отдельной строке:

day = "Monday"

match day:

case "Sunday":

print("Take it easy")

case "Monday":

print("Go to work")

case "Tuesday":

print("Work + Hobbies")

case "Wednesday":

print("Meetings")

case "Thursday":

print("Presentations")

case "Friday":

print("Interviews and party")

case "Saturday":

print("Time to do sports")

Теперь у вас есть общее представление о том, как код с выражениями match-case может упростить код с if-else.

Теперь давайте рассмотрим типы шаблонов для сопоставления в выражениях match-case.

## 5 распространенных типов шаблонов для match-case

В выражениях match-case можно использовать различные типы шаблонов. Наиболее примечательные варианты:

1. Шаблон литерал (literal).
2. Шаблон захвата (capture).
3. Шаблон подстановки (wildcard).
4. Шаблон постоянных значений (constant value).
5. Шаблон последовательностей (sequence)

Рассмотрим теперь каждый вариант отдельно.

### Шаблон литерал (literal)

Самый простой вариант использования match-case — это сопоставление с литеральными шаблонами. Литерал, с которым можно сравнивать, может быть:

- числом
- строкой
- None
- True
- False

Хорошим примером данного шаблона является уже рассмотренный нами код с днями недели. В нем мы сравниваем строковые литералы с переменной `day`:

day = "Monday"

match day:

case "Sunday" : print("Take it easy")

case "Monday" : print("Go to work")

case "Tuesday" : print("Work + Hobbies")

case "Wednesday" : print("Meetings")

case "Thursday" : print("Presentations")

case "Friday" : print("Interviews and party")

case "Saturday" : print("Time to do sports")

Этот код проверяет, какое значение имеет переменная `day`. На основании этого он выполняет действие, которое в данном случае представляет собой простой вывод в консоль.

### Шаблон захвата (capture)

Выражение match-case можно использовать для сохранения (захвата) совпавшего значения в переменную.

Лучше всего это продемонстрировать на примере.

Давайте создадим функцию `greet()`, которая приветствует человека, если указано его имя.

def greet(name=None):

match name:

# Check if name == None

case None:

print("Hello there")

# Store name into some_name if it is not None

case some_name:

print(f"Hello {some_name}")

greet() # Prints "Hello there"

greet("Jack") # Prints "Hello Jack"

Выражение match-case делает две вещи (результат указан в комментариях):

- Проверяет, имеет ли переменная `name` значение `None`. Если это так, в консоли отображается приветственное сообщение по умолчанию.
- Проверяет, имеет ли переменная `name` какое-либо другое значение. Если да, это имя сохраняется в переменную `some_name`. И дальше в консоль выводится приветствие именно с этим именем.

### Шаблон подстановки (wildcard)

При использовании выражения match-case вы можете применять знак подстановки для сопоставления без привязки к конкретному значению. При этом подстановка соответствует всему, что не включено в выражения `case`. В некотором смысле подстановка — это блок else выражения match-case.

Для знака подстановки используется символ подчеркивания `_`.

#### Первый пример использования знака подстановки

Например, давайте проверять результат бросания монеты:

coinflip = 4

match coinflip:

case 1:

print("Heads")

exit()

case 0:

print("Tails")

case _:

print("Must be 0 or 1.")

Результат:

Must be 0 or 1.

Здесь знак подстановки соответствует всему кроме 0 или 1. В нашем коде результат подбрасывания монеты в переменной `coinflip` по какой-то причине равен 4. Этот результат совпадает с подстановкой и поэтому запускается соответствующее действие.

Это один из способов использования шаблона подстановки в выражениях match-case.

#### Второй пример использования знака подстановки

Допустим, вам безразличны значения элементов некоторой коллекции. Вам просто нужно убедиться, что эти элементы существуют, иными словами важно их количество.

Например, давайте проверим размер кортежа, не рассматривая значения его элементов:

location = (0, 0)

match location:

case(_,):

print("1D location found")

case(_, _):

print("2D location found")

case(_, _, _):

print(("3D location found"))

Результат:

2D location found

Как можно заметить, в этом случае проверяется только количество найденных элементов. Здесь мы использовали подстановку, чтобы убедиться, что в кортеже есть два элемента, не заботясь об отдельных значениях элементов.

То же самое можно сделать, используя оператор if-else и функцию `len()`. Но если операторы if-else становятся слишком длинными, лучше использовать оператор match-case для повышения качества кода.

### Шаблон постоянных значений и перечисления

В качестве шаблонов выражения match-case можно использовать элементы перечислений (enumerations).

Чтобы это продемонстрировать, давайте создадим класс перечисления `Direction` (унаследованный от класса `Enum`), представляющий четыре основных направления на компасе.

Далее создадим функцию `handle_directions()`, которая принимает элемент класса `Direction` в качестве входного аргумента. Эта функция сопоставляет этот аргумент с одним из направлений из перечисления и реагирует соответствующим образом.

Вот сам код:

from enum import Enum

class Direction(Enum):

NORTH = 1

EAST = 2

SOUTH = 3

WEST = 4

def handle_directions(direction):

match direction:

case Direction.NORTH: print("Heading North")

case Direction.EAST: print("Heading East")

case Direction.SOUTH: print("Heading South")

case Direction.WEST: print("Heading West")

Теперь можно вызвать функцию handle_directions() следующим образом:

handle_directions(Direction.NORTH)

Конечно, приведенный выше код можно заменить чем-то более коротким или простым. Но этот пример наглядно демонстрирует, как можно использовать перечисления вместе с выражениями match-case.

### Шаблон последовательностей

Для сопоставления с заданным шаблоном можно использовать распакованные элементы различных последовательностей (коллекций).

Если вы вдруг не знаете, что такое распаковка, то это означает вытягивание значений последовательности в отдельные переменные.

Вот пример распаковки списка в четыре переменные:

directions = ["North", "East", "South", "West"]

# Grab the values from a list and store them into variables

n, e, s, w = directions

print(n) # prints North

print(e) # prints East

print(s) # prints South

print(w) # prints West

Таким образом, распаковку можно использовать вместе с выражением match-case.

Например, давайте проверим, является ли местоположение (переменная `location`) точкой 1D, 2D или 3D. Кроме того, давайте будем сохранять значения координат в отдельных переменных в зависимости от количества измерений.

location = (1, 3)

match location:

case x, :

print(f"1D location found: ({x})")

case x, y:

print(f"2D location found: ({x}, {y})")

case x, y, z:

print((f"3D location found: ({x}, {y}, {z})"))

Результат:

2D location found: (1, 3)

Данный код сопоставляет переменную location, в которой сохранен некий кортеж с координатами, с кортежами из одного, двух или трех элементов. А затем значения координат распаковываются в различные переменные.

Если в кортеже больше значений, но важны только первые три, можно использовать оператор *.

Допустим, есть кортеж с координатами, сохраненный в переменную `location`. И вдобавок в нем еще могут храниться посторонние элементы. Чтобы отловить «лишние» элементы, воспользуемся оператором `*`:

location = (1, 3, 2, "a", "b", "c")

match location:

case x, :

print(f"1D location found: ({x})")

case x, y:

print(f"2D location found: ({x}, {y})")

case x, y, z, *names:

print((f"3D location found: ({x}, {y}, {z})"))

print(f"Also, there was some extra data: {names}")

Результат:

3D location found: (1, 3, 2)

Also, there was some extra data: ['a', 'b', 'c']

Теперь выражение `*names` захватывает все остальные элементы, вне зависимости от того, сколько их там есть.

Теперь вы знаете основные типы шаблонов, с которыми можно производить сравнения. И последнее, но не менее важное: давайте посмотрим, как комбинировать шаблоны в выражении match-case.

## Комбинирование шаблонов в `match-case`

В конструкции match-case можно сравнивать сразу несколько шаблонов.

Для этого используется логический оператор `|`(или). Таким образом проверяется, соответствует ли хотя бы один шаблон заданному значению.

Например, давайте проверим, выходным или рабочим днем является день недели:

day = "Monday"

match day:

case "Saturday" | "Sunday":

print("Weekend")

case "Monday" | "Tuesday" | "Wednesday" | "Thursday" | "Friday":

print("Work")

Результат:

Work

## Заключение

Во многих языках программирования длинные цепочки операторов if-else можно заменять более аккуратными операторами switch-case.

И наконец-то в версии Python 3.10 была реализована аналогичная функция — match-case.

Перевод статьи Arturri Jalli [Python ‘switch case’ Statement: A Complete Guide (match case)](https://www.codingem.com/python-switch-case-statement/).

