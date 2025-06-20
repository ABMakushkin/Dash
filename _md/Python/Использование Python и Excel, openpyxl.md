2024-09-24
#python 
[Использование Python и Excel для обработки и анализа данных. Часть 2: библиотеки для работы с данными / Хабр (habr.com)](https://habr.com/ru/companies/otus/articles/331998/)


**Как читать и редактировать Excel файлы при помощи openpyxl  
**  
  
ПЕРЕВОД  
Оригинал статьи — [www.datacamp.com/community/tutorials/python-excel-tutorial](https://www.datacamp.com/community/tutorials/python-excel-tutorial)  
Автор — Karlijn Willems  
  
Эта библиотека пригодится, если вы хотите читать и редактировать файлы .xlsx, xlsm, xltx и xltm.  
  
Установите openpyxl using pip. Общие рекомендации по установке этой библиотеки — сделать это в виртуальной среде Python без системных библиотек. Вы можете использовать виртуальную среду для создания изолированных сред Python: она создает папку, содержащую все необходимые файлы, для использования библиотек, которые потребуются для Python.  
  
Перейдите в директорию, в которой находится ваш проект, и повторно активируйте виртуальную среду venv. Затем перейдите к установке openpyxl с помощью pip, чтобы убедиться, что вы можете читать и записывать с ним файлы:  
  

```
# Activate virtualenv
$ source activate venv

# Install `openpyxl` in `venv`
$ pip install openpyxl
```

  
Теперь, когда вы установили openpyxl, вы можете начать загрузку данных. Но что именно это за данные? Например, в книге с данными, которые вы пытаетесь получить на Python, есть следующие листы:  
  
![](https://habrastorage.org/r/w1560/web/db6/738/152/db6738152e3444c091b29ef030703bad.png)  
  
Функция load_workbook () принимает имя файла в качестве аргумента и возвращает объект рабочей книги, который представляет файл. Это можно проверить запуском type (wb). Не забудьте убедиться, что вы находитесь в правильной директории, где расположена электронная таблица. В противном случае вы получите сообщение об ошибке при импорте.  
  

```
# Import `load_workbook` module from `openpyxl`
from openpyxl import load_workbook

# Load in the workbook
wb = load_workbook('./test.xlsx')

# Get sheet names
print(wb.get_sheet_names())
```

  
Помните, вы можете изменить рабочий каталог с помощью os.chdir (). Фрагмент кода выше возвращает имена листов книги, загруженной в Python. Вы можете использовать эту информацию для получения отдельных листов книги. Также вы можете проверить, какой лист активен в настоящий момент с помощью wb.active. В приведенном ниже коде, вы также можете использовать его для загрузки данных на другом листе книги:  
  

```
# Get a sheet by name 
sheet = wb.get_sheet_by_name('Sheet3')

# Print the sheet title 
sheet.title

# Get currently active sheet
anotherSheet = wb.active

# Check `anotherSheet` 
anotherSheet
```

  
На первый взгляд, с этими объектами Worksheet мало что можно сделать. Однако, можно извлекать значения из определенных ячеек на листе книги, используя квадратные скобки [], к которым нужно передавать точную ячейку, из которой вы хотите получить значение.  
  
Обратите внимание, это похоже на выбор, получение и индексирование массивов NumPy и Pandas DataFrames, но это еще не все, что нужно сделать, чтобы получить значение. Нужно еще добавить значение атрибута:  
  

```
# Retrieve the value of a certain cell
sheet['A1'].value

# Select element 'B2' of your sheet 
c = sheet['B2']

# Retrieve the row number of your element
c.row

# Retrieve the column letter of your element
c.column

# Retrieve the coordinates of the cell 
c.coordinate
```

  
Помимо value, есть и другие атрибуты, которые можно использовать для проверки ячейки, а именно row, column и coordinate:  
  
Атрибут row вернет 2;  
Добавление атрибута column к “С” даст вам «B»;  
coordinate вернет «B2».  
  
Вы также можете получить значения ячеек с помощью функции cell (). Передайте аргументы row и column, добавьте значения к этим аргументам, которые соответствуют значениям ячейки, которые вы хотите получить, и, конечно же, не забудьте добавить атрибут value:  
  

```
# Retrieve cell value 
sheet.cell(row=1, column=2).value

# Print out values in column 2 
for i in range(1, 4):
     print(i, sheet.cell(row=i, column=2).value)
```

  
Обратите внимание: если вы не укажете значение атрибута value, вы получите <Cell Sheet3.B1>, который ничего не говорит о значении, которое содержится в этой конкретной ячейке.  
  
Вы используете цикл с помощью функции range (), чтобы помочь вам вывести значения строк, которые имеют значения в столбце 2. Если эти конкретные ячейки пусты, вы получите None.  
Более того, существуют специальные функции, которые вы можете вызвать, чтобы получить другие значения, например get_column_letter () и column_index_from_string.  
  
В двух функциях уже более или менее указано, что вы можете получить, используя их. Но лучше всего сделать их явными: пока вы можете получить букву прежнего столбца, можно сделать обратное или получить индекс столбца, перебирая букву за буквой. Как это работает:  
  

```
# Import relevant modules from `openpyxl.utils`
from openpyxl.utils import get_column_letter, column_index_from_string

# Return 'A'
get_column_letter(1)

# Return '1'
column_index_from_string('A')
```

  
Вы уже получили значения для строк, которые имеют значения в определенном столбце, но что нужно сделать, если нужно вывести строки файла, не сосредотачиваясь только на одном столбце?  
  
Конечно, использовать другой цикл.  
  
Например, вы хотите сосредоточиться на области, находящейся между «A1» и «C3», где первый указывает левый верхний угол, а второй — правый нижний угол области, на которой вы хотите сфокусироваться. Эта область будет так называемой cellObj, которую вы видите в первой строке кода ниже. Затем вы указываете, что для каждой ячейки, которая находится в этой области, вы хотите вывести координату и значение, которое содержится в этой ячейке. После окончания каждой строки вы хотите выводить сообщение-сигнал о том, что строка этой области cellObj была выведена.  
  

```
# Print row per row
for cellObj in sheet['A1':'C3']:
      for cell in cellObj:
              print(cells.coordinate, cells.value)
      print('--- END ---')
```

  
Обратите внимание, что выбор области очень похож на выбор, получение и индексирование списка и элементы NumPy, где вы также используете квадратные скобки и двоеточие чтобы указать область, из которой вы хотите получить значения. Кроме того, вышеприведенный цикл также хорошо использует атрибуты ячейки!  
  
Чтобы визуализировать описанное выше, возможно, вы захотите проверить результат, который вернет вам завершенный цикл:  
  

```
('A1', u'M')
('B1', u'N')
('C1', u'O')
--- END ---
('A2', 10L)
('B2', 11L)
('C2', 12L)
--- END ---
('A3', 14L)
('B3', 15L)
('C3', 16L)
--- END ---
```

  
Наконец, есть некоторые атрибуты, которые вы можете использовать для проверки результата импорта, а именно max_row и max_column. Эти атрибуты, конечно, являются общими способами обеспечения правильной загрузки данных, но тем не менее в данном случае они могут и будут полезны.  
  

```
# Retrieve the maximum amount of rows 
sheet.max_row

# Retrieve the maximum amount of columns
sheet.max_column
```

  
Это все очень классно, но мы почти слышим, что вы сейчас думаете, что это ужасно трудный способ работать с файлами, особенно если нужно еще и управлять данными.  
Должно быть что-то проще, не так ли? Всё так!  
  
Openpyxl имеет поддержку Pandas DataFrames. И можно использовать функцию DataFrame () из пакета Pandas, чтобы поместить значения листа в DataFrame:  
  

```
# Import `pandas` 
import pandas as pd

# Convert Sheet to DataFrame
df = pd.DataFrame(sheet.values)
Если вы хотите указать заголовки и индексы, вам нужно добавить немного больше кода:
# Put the sheet values in `data`
data = sheet.values

# Indicate the columns in the sheet values
cols = next(data)[1:]

# Convert your data to a list
data = list(data)

# Read in the data at index 0 for the indices
idx = [r[0] for r in data]

# Slice the data at index 1 
data = (islice(r, 1, None) for r in data)

# Make your DataFrame
df = pd.DataFrame(data, index=idx, columns=cols)
```

  
Затем вы можете начать управлять данными при помощи всех функций, которые есть в Pandas. Но помните, что вы находитесь в виртуальной среде, поэтому, если библиотека еще не подключена, вам нужно будет установить ее снова через pip.  
  
Чтобы записать Pandas DataFrames обратно в файл Excel, можно использовать функцию dataframe_to_rows () из модуля utils:  
  

```
# Import `dataframe_to_rows`
from openpyxl.utils.dataframe import dataframe_to_rows

# Initialize a workbook 
wb = Workbook()

# Get the worksheet in the active workbook
ws = wb.active

# Append the rows of the DataFrame to your worksheet
for r in dataframe_to_rows(df, index=True, header=True):
    ws.append(r)
```

  
Но это определенно не все! Библиотека openpyxl предлагает вам высокую гибкость в отношении того, как вы записываете свои данные в файлы Excel, изменяете стили ячеек или используете режим только для записи. Это делает ее одной из тех библиотек, которую вам точно необходимо знать, если вы часто работаете с электронными таблицами.  
  
И не забудьте деактивировать виртуальную среду, когда закончите работу с данными!  
  
Теперь давайте рассмотрим некоторые другие библиотеки, которые вы можете использовать для получения данных в электронной таблице на Python.  
  
Готовы узнать больше?  
  
**Чтение и форматирование Excel файлов xlrd**  
Эта библиотека идеальна, если вы хотите читать данные и форматировать данные в файлах с расширением .xls или .xlsx.  
  

```
# Import `xlrd`
import xlrd

# Open a workbook 
workbook = xlrd.open_workbook('example.xls')

# Loads only current sheets to memory
workbook = xlrd.open_workbook('example.xls', on_demand = True)
```

  
Если вы не хотите рассматривать всю книгу, можно использовать такие функции, как sheet_by_name () или sheet_by_index (), чтобы извлекать листы, которые необходимо использовать в анализе.  
  

```
# Load a specific sheet by name
worksheet = workbook.sheet_by_name('Sheet1')

# Load a specific sheet by index 
worksheet = workbook.sheet_by_index(0)

# Retrieve the value from cell at indices (0,0) 
sheet.cell(0, 0).value
```

  
Наконец, можно получить значения по определенным координатам, обозначенным индексами.  
О том, как xlwt и xlutils, соотносятся с xlrd расскажем дальше.  
  
**Запись данных в Excel файл при помощи xlrd**  
  
Если нужно создать электронные таблицы, в которых есть данные, кроме библиотеки XlsxWriter можно использовать библиотеки xlwt. Xlwt идеально подходит для записи и форматирования данных в файлы с расширением .xls.  
  
Когда вы вручную хотите записать в файл, это будет выглядеть так:  
  

```
# Import `xlwt` 
import xlwt

# Initialize a workbook 
book = xlwt.Workbook(encoding="utf-8")

# Add a sheet to the workbook 
sheet1 = book.add_sheet("Python Sheet 1") 

# Write to the sheet of the workbook 
sheet1.write(0, 0, "This is the First Cell of the First Sheet") 

# Save the workbook 
book.save("spreadsheet.xls")
```

  
Если нужно записать данные в файл, то для минимизации ручного труда можно прибегнуть к циклу for. Это позволит немного автоматизировать процесс. Делаем скрипт, в котором создается книга, в которую добавляется лист. Далее указываем список со столбцами и со значениями, которые будут перенесены на рабочий лист.  
  
Цикл for будет следить за тем, чтобы все значения попадали в файл: задаем, что с каждым элементом в диапазоне от 0 до 4 (5 не включено) мы собираемся производить действия. Будем заполнять значения строка за строкой. Для этого указываем row элемент, который будет “прыгать” в каждом цикле. А далее у нас следующий for цикл, который пройдется по столбцам листа. Задаем условие, что для каждой строки на листе смотрим на столбец и заполняем значение для каждого столбца в строке. Когда заполнили все столбцы строки значениями, переходим к следующей строке, пока не заполним все имеющиеся строки.  
  

```
# Initialize a workbook
book = xlwt.Workbook()

# Add a sheet to the workbook
sheet1 = book.add_sheet("Sheet1")

# The data
cols = ["A", "B", "C", "D", "E"]
txt = [0,1,2,3,4]

# Loop over the rows and columns and fill in the values
for num in range(5):
      row = sheet1.row(num)
      for index, col in enumerate(cols):
          value = txt[index] + num
          row.write(index, value)

# Save the result
book.save("test.xls")
```

  
В качестве примера скриншот результирующего файла:  
  
![](https://habrastorage.org/r/w1560/web/902/afb/461/902afb461a1240d38455b643c403b5c0.png)  
  
Теперь, когда вы видели, как xlrd и xlwt взаимодействуют вместе, пришло время посмотреть на библиотеку, которая тесно связана с этими двумя: xlutils.  
  
**Коллекция утилит xlutils**  
  
Эта библиотека в основном представляет собой набор утилит, для которых требуются как xlrd, так и xlwt. Включает в себя возможность копировать и изменять/фильтровать существующие файлы. Вообще говоря, оба этих случая подпадают теперь под openpyxl.  
  
**Использование pyexcel для чтения файлов .xls или .xlsx**  
  
Еще одна библиотека, которую можно использовать для чтения данных таблиц в Python — pyexcel. Это Python Wrapper, который предоставляет один API для чтения, обработки и записи данных в файлах .csv, .ods, .xls, .xlsx и .xlsm.  
  
Чтобы получить данные в массиве, можно использовать функцию get_array (), которая содержится в пакете pyexcel:  
  

```
# Import `pyexcel`
import pyexcel

# Get an array from the data
my_array = pyexcel.get_array(file_name="test.xls")
 
Также можно получить данные в упорядоченном словаре списков, используя функцию get_dict ():
# Import `OrderedDict` module 
from pyexcel._compact import OrderedDict

# Get your data in an ordered dictionary of lists
my_dict = pyexcel.get_dict(file_name="test.xls", name_columns_by_row=0)

# Get your data in a dictionary of 2D arrays
book_dict = pyexcel.get_book_dict(file_name="test.xls")
```

  
Однако, если вы хотите вернуть в словарь двумерные массивы или, иными словами, получить все листы книги в одном словаре, стоит использовать функцию get_book_dict ().  
  
Имейте в виду, что обе упомянутые структуры данных, массивы и словари вашей электронной таблицы, позволяют создавать DataFrames ваших данных с помощью pd.DataFrame (). Это упростит обработку ваших данных!  
  
Наконец, вы можете просто получить записи с pyexcel благодаря функции get_records (). Просто передайте аргумент file_name функции и обратно получите список словарей:  
  

```
# Retrieve the records of the file
records = pyexcel.get_records(file_name="test.xls")
```

  
**Записи файлов при помощи pyexcel**  
  
Так же, как загрузить данные в массивы с помощью этого пакета, можно также легко экспортировать массивы обратно в электронную таблицу. Для этого используется функция save_as () с передачей массива и имени целевого файла в аргумент dest_file_name:  
  

```
# Get the data
data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# Save the array to a file
pyexcel.save_as(array=data, dest_file_name="array_data.xls")
```

  
Обратите внимание: если указать разделитель, то можно добавить аргумент dest_delimiter и передать символ, который хотите использовать, в качестве разделителя между “”.  
  
Однако, если у вас есть словарь, нужно будет использовать функцию save_book_as (). Передайте двумерный словарь в bookdict и укажите имя файла, и все ОК:  
  

```
# The data
2d_array_dictionary = {'Sheet 1': [
                                   ['ID', 'AGE', 'SCORE']
                                   [1, 22, 5],
                                   [2, 15, 6],
                                   [3, 28, 9]
                                  ],
                       'Sheet 2': [
                                    ['X', 'Y', 'Z'],
                                    [1, 2, 3],
                                    [4, 5, 6]
                                    [7, 8, 9]
                                  ],
                       'Sheet 3': [
                                    ['M', 'N', 'O', 'P'],
                                    [10, 11, 12, 13],
                                    [14, 15, 16, 17]
                                    [18, 19, 20, 21]
                                   ]}

# Save the data to a file                        
pyexcel.save_book_as(bookdict=2d_array_dictionary, dest_file_name="2d_array_data.xls")
```

  
Помните, что когда используете код, который напечатан в фрагменте кода выше, порядок данных в словаре не будет сохранен!  
  
**Чтение и запись .csv файлов**  
  
Если вы все еще ищете библиотеки, которые позволяют загружать и записывать данные в CSV-файлы, кроме Pandas, рекомендуем библиотеку csv:  
  

```
# import `csv`
import csv

# Read in csv file 
for row in csv.reader(open('data.csv'), delimiter=','):
      print(row)
      
# Write csv file
data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
outfile = open('data.csv', 'w')
writer = csv.writer(outfile, delimiter=';', quotechar='"')
writer.writerows(data)
outfile.close()
```

  
Обратите внимание, что NumPy имеет функцию genfromtxt (), которая позволяет загружать данные, содержащиеся в CSV-файлах в массивах, которые затем можно помещать в DataFrames.  
  
**Финальная проверка данных**  
  
Когда данные подготовлены, не забудьте последний шаг: проверьте правильность загрузки данных. Если вы поместили свои данные в DataFrame, вы можете легко и быстро проверить, был ли импорт успешным, выполнив следующие команды:  
  

```
# Check the first entries of the DataFrame
df1.head()

# Check the last entries of the DataFrame
df1.tail()
```

  
Note: Используйте DataCamp Pandas Cheat Sheet, когда вы планируете загружать файлы в виде Pandas DataFrames.  
  
Если данные в массиве, вы можете проверить его, используя следующие атрибуты массива: shape, ndim, dtype и т.д.:  
  

```
# Inspect the shape 
data.shape

# Inspect the number of dimensions
data.ndim

# Inspect the data type
data.dtype
```

  
**Что дальше?**  
  
Поздравляем, теперь вы знаете, как читать файлы Excel в Python :) Но импорт данных — это только начало рабочего процесса в области данных. Когда у вас есть данные из электронных таблиц в вашей среде, вы можете сосредоточиться на том, что действительно важно: на анализе данных.  
  
Если вы хотите глубже погрузиться в тему — знакомьтесь с PyXll, которая позволяет записывать функции в Python и вызывать их в Excel.