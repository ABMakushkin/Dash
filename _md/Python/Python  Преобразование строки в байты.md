2024-09-11
#python 
[Python | Преобразование строки в байты - GeeksforGeeks](https://www.geeksforgeeks.org/python-convert-string-to-bytes/?ysclid=m0qgvda0o0149733405)

[[Python StringIO]]


Inter-преобразования, как обычно, довольно популярны, но преобразование между строкой и байтами в наши дни более распространено из-за того, что для обработки файлов или машинного обучения (Pickle File) нам широко требуется, чтобы строки были преобразованы в байты. Давайте обсудим некоторые способы, которыми это можно сделать.

 **Метод № 1: использование bytes(str, enc)** Строка может быть преобразована в байты с помощью универсальной функции bytes. Эта функция внутренне указывает на библиотеку CPython, которая неявно вызывает функцию encode для преобразования строки в указанную кодировку. 

- Питон3

|   |
|---|
|`# Python code to demonstrate`<br><br>`# convert string to byte`<br><br>`# Using bytes(str, enc)`<br><br>`# initializing string`<br><br>`test_string` `=` `"GFG` `is` `best"`<br><br>`# printing original string`<br><br>`print``("The original string : "` `+` `str``(test_string))`<br><br>`# Using bytes(str, enc)`<br><br>`# convert string to byte`<br><br>`res` `=` `bytes(test_string,` `'utf-8'``)`<br><br>`# print result`<br><br>`print``("The byte converted string` `is`  `: "` `+` `str``(res)` `+` `",` `type` `: "` `+` `str``(``type``(res)))`|

**Выход :** 

Исходная строка: GFG лучше всего
Строка, преобразованная в байты, выглядит так: b'GFG is best', тип: <class 'bytes'>

  **Метод №2: использование encode(enc)** Наиболее рекомендуемый метод для выполнения этой конкретной задачи — использование функции encode для выполнения преобразования, поскольку это сокращает одно дополнительное связывание с определенной библиотекой, эта функция вызывает ее напрямую. 

- Питон3

|   |
|---|
|`# Python code to demonstrate`<br><br>`# convert string to byte`<br><br>`# Using encode(enc)`<br><br>`# initializing string`<br><br>`test_string` `=` `"GFG` `is` `best"`<br><br>`# printing original string`<br><br>`print``("The original string : "` `+` `str``(test_string))`<br><br>`# Using encode(enc)`<br><br>`# convert string to byte`<br><br>`res` `=` `test_string.encode(``'utf-8'``)`<br><br>`# print result`<br><br>`print``("The byte converted string` `is`  `: "` `+` `str``(res)` `+` `",` `type` `: "` `+` `str``(``type``(res)))`|

**Выход :** 

Исходная строка: GFG лучше всего
Строка, преобразованная в байты, выглядит так: b'GFG is best', тип: <class 'bytes'>

#### Метод №2: использование memoryview()

В этом примере мы вызываем метод encode() для переменной my_string, чтобы преобразовать ее в байты с использованием кодировки UTF-8. Затем мы передаем полученный объект bytes в функцию memoryview(), которая возвращает объект представления памяти, который предоставляет представление базовых байтов.

Наконец, мы вызываем метод tobytes() для объекта представления памяти, чтобы создать новый объект bytes, содержащий те же данные. Этот объект bytes сохраняется в переменной my_bytes и выводится на консоль.

#### ПРИМЕЧАНИЕ: функция memoryview() полезна в ситуациях, когда вам нужно получить доступ к базовым байтам объекта без их копирования. Однако это может быть не самым эффективным подходом для простого преобразования строк в байты, поскольку это влечет за собой дополнительные накладные расходы.

- Питон3

|   |
|---|
|`my_string` `=` `"Hello, world!"`  `#Define a string called my_string with the value "Hello, world!".`<br><br>`my_bytes` `=` `memoryview(my_string.encode(``'utf-8'``)).tobytes()`<br><br>`#Encode the string as bytes using the UTF-8 encoding by calling the encode() method on my_string and passing 'utf-8' as the argument. This will return a bytes object containing the encoded bytes.`<br><br>`#Convert the memoryview object of the bytes object to bytes using the tobytes() method. This creates a new bytes object that is a copy of the original bytes object.`<br><br>`#Print the resulting bytes object using the print() function.#`<br><br>`print``(my_bytes)`|

**Выход**

b'Привет, мир!'

временная сложность: O(n), 

Сложность пространства: O(n)

**Метод №3: Использование метода binascii.unhexlify():**

**Алгоритм:**

1. Импортируйте модуль binascii  
2. Инициализируйте строку, содержащую шестнадцатеричное представление байтов  
3. Используйте метод unhexlify() модуля binascii для преобразования шестнадцатеричной строки в байты  
4. Выведите преобразованные байты и их тип.

- Питон3

|   |
|---|
|`import` `binascii`<br><br>`# initializing string`<br><br>`test_string` `=` `"4766472069732062657374"`<br><br>`# printing original string`<br><br>`print``(``"The original string : "` `+` `str``(test_string))`<br><br>`# Using binascii.unhexlify()`<br><br>`# convert string to byte`<br><br>`res` `=` `binascii.unhexlify(test_string)`<br><br>`# print result`<br><br>`print``(``"The byte converted string is : "` `+` `str``(res)` `+` `", type : "` `+` `str``(``type``(res)))`<br><br>`#This code is contributed by  Jyothi pinjala`|

**Выход**

Исходная строка: 4766472069732062657374
Строка, преобразованная в байты, выглядит так: b'GfG is best', тип: <class 'bytes'>

**Сложность времени:**

Метод binascii.unhexlify() имеет временную сложность O(n), где n — длина входной строки.  
Все остальные операции в этом коде имеют временную сложность O(1).  
Таким образом, общая временная сложность этого кода составляет O(n).

**Сложность пространства:**

Пространственная сложность этого кода составляет O(1), поскольку он использует только постоянное количество дополнительного пространства, независимо от размера входных данных.

**Метод 5: использование метода struct.pack().**

**Пошаговый подход** 

Импортируйте модуль struct в свой код.  
Инициализируйте строку с именем «test_string» со значением «GFG is best».  
Распечатайте исходную строку с помощью оператора print.  
Используйте метод bytes() для преобразования строки в байты. Передайте методу кодировку «test_string» и «utf-8».  
Используйте метод struct.pack() для преобразования байтов в двоичные данные. Передайте методу строку формата «10s» и байты в качестве параметров. Строка формата «10s» указывает, что входные данные следует обрабатывать как строку длиной 10.  
Сохраните результат в переменной «res».  
Распечатайте преобразованную строку байтов вместе с ее типом с помощью оператора print.  
 

- Питон3

|   |
|---|
|`import` `struct`<br><br>`# initializing string`<br><br>`test_string` `=` `"GFG is best"`<br><br>`# printing original string`<br><br>`print``(``"The original string : "` `+` `str``(test_string))`<br><br>`# Using struct.pack()`<br><br>`# convert string to byte`<br><br>`res` `=` `struct.pack(``'10s'``, bytes(test_string,` `'utf-8'``))`<br><br>`# print result`<br><br>`print``(``"The byte converted string is  : "` `+` `str``(res)` `+` `", type : "` `+` `str``(``type``(res)))`|

**Выход**

Исходная строка: GFG лучше всего
Строка, преобразованная в байты, имеет вид: b'GFG is bes', тип: <class 'bytes'>

Временная сложность: Временная сложность методов bytes() и struct.pack() составляет O(n), где n — длина входной строки.

Вспомогательное пространство: сложность методов bytes() и struct.pack() составляет O(n), где n — длина входной строки.