2025-05-26
#python 

[Документация Python по f-строкам](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals)

Начиная с версии 3.6 в Python появился новый тип строк — `f-строки`, которые буквально означают «formatted string». Эти строки улучшают читаемость кода, а также работают быстрее, чем другие способы форматирования.

Чтобы использовать `f-строки`, перед строкой ставится буква `"f"` или `"F"`. Выражения для вставки помещаются в фигурные скобки `{}`:

Пример:

```python
force = "Темная Сторона" message = f"Да пребудет с тобой {force}!" print(message)  
# Выведет: Да пребудет с тобой Темная Сторона!
```

`Важно!` В фигурных скобочках можно указывать переменные, которые доступны в текущей области видимости (об этом позднее) и Python сам преобразует их в строку и вставляет в нужное место.

Пример:

```python
age = 28 message = f"Мне {age} лет" print(message)  
# Выведет: Мне 28 лет
```


Более того, в фигурных скобках можно использовать **выражения с кучей переменных**.

Пример:

```python
birth_year = 1985 current_year = 2024 message = f"Мне {current_year - birth_year} лет" print(message)  
# Выведет: Мне 39 лет
```

Под капотом это все конвертируется в вызов функции `format()`, но новый подход действительно удобнее.

Пользуйтесь в свое удовольствие.

#### Встраивание выражений

Внутри фигурных скобок можно использовать любые выражения, включая арифметические операции, вызовы функций и доступ к атрибутам объектов:

pythonCopy code

```python
age = 30 message = f"{name} is {age} years old." print(message)  
# Вывод: Alice is 30 years old.  
# Пример с выражением 
area = 5 message = f"The area of a rectangle is {area * area}." print(message)  
# Вывод: The area of a rectangle is 25.
```

#### Форматирование чисел

F-строки поддерживают спецификаторы форматирования, которые позволяют красиво выводить числа:

pythonCopy code

```python
value = 123.456789 formatted_value = f"{value:.2f}"  
# Округление до 2 знаков после запятой print(formatted_value)  
# Вывод: 123.46
```

#### Выравнивание и ширина

Можно использовать выравнивание и задавать ширину полей:

pythonCopy code

```python
name = "Bob" age = 25 message = f"|{'Name':<10}|{'Age':^5}|{'Occupation':>15}|" print(message)  
# Вывод: |Name      | Age |    Occupation|
```

#### Экранирование фигурных скобок

Если нужно использовать фигурные скобки в самой строке, их можно экранировать:

pythonCopy code

```python
message = f"{{Hello}} {name}"  
# Вывод: {Hello} Alice print(message)
```

#### Примеры использования f-строк

1. **Вывод информации о пользователе:**

pythonCopy code

```python
username = "john_doe" score = 95 print(f"User {username} has a score of {score}.")
```

2. **Форматирование даты:**

pythonCopy code

```python
from datetime import datetime current_date = datetime.now() print(f"Today's date is {current_date:%Y-%m-%d}.")
```

3. **Использование в циклах:**

pythonCopy code

```python
for i in range(3):     
	print(f"Item {i + 1}: {i * 10}")
```

