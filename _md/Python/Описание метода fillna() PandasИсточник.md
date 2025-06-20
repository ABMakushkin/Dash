2024-09-24
#python 
[Описание метода fillna() Pandas для заполнения нулевых значений (pythonpip.ru)](https://pythonpip.ru/pandas/pandas-fillna?ysclid=m1gj8ovfxl144985138)

Мы можем использовать функцию Pandas.fillna() для заполнения нулевых значений в наборе данных.


Описание метода fillna() Pandas:

Синтаксис
DataFrame.fillna(value=None, method=None, axis=None, inplace=False, limit=None, downcast=None, **kwargs) 
Параметры
value: это значение, которое используется для заполнения нулевых значений, поочередно Series/dict/DataFrame.
method: метод, который используется для заполнения пустых значений в переиндексированном ряду.
axis: принимает целочисленное или строковое значение для строк/столбцов. Ось, по которой нам нужно заполнить пропущенные значения.
inplace: если это True, он заполняет значения на пустом месте.
limit: это целочисленное значение, указывающее максимальное количество последовательных заполнений значения NaN вперед/назад.
downcast: требуется dict, указывающий, что следует преобразовывать, например Float64, в int64.
Returns
Он возвращает объект, в котором заполняются пропущенные значения.

Пример 1
import pandas as pd 
# Create a dataframe 
info = pd.DataFrame(data={'x':[10,20,30,40,50,None]}) 
print(info) 
# Fill null value to dataframe using 'inplace' 
info.fillna(value=0, inplace=True) 
print(info) 
Вывод:

       x 
0     10.0 
1     20.0 
2     30.0 
3     40.0 
4     50.0 
5     NaN 
       x 
0     10.0 
1     20.0 
2     30.0 
3     40.0 
4     50.0 
5      0.0 
Пример 2
Приведенный ниже код отвечает за заполнение DataFrame, состоящего из некоторых значений NaN.

import pandas as pd 
# Create a dataframe 
info = pd.DataFrame([[np.nan,np.nan, 20, 0], 
[1, np.nan, 4, 1], 
[np.nan, np.nan, np.nan, 5], 
[np.nan, 20, np.nan, 2]], 
columns=list('ABCD')) 
info 
Вывод:


РЕКЛАМА
    A    B     C    D 
0  NaN  NaN   20.0  0 
1  1.0  NaN   4.0   1 
2  NaN  NaN   NaN   5 
3  NaN  20.0  NaN   2 
Пример 3
В приведенном ниже коде мы использовали функцию fillna() для заполнения только некоторых значений NaN.

info = pd.DataFrame([[np.nan,np.nan, 20, 0], 
[1, np.nan, 4, 1], 
[np.nan, np.nan, np.nan, 5], 
[np.nan, 20, np.nan, 2]], 
columns=list('ABCD')) 
info 
info.fillna(0) 
info.fillna(method='ffill') 
values = {'A': 0, 'B': 1, 'C': 2, 'D': 3} 
info.fillna(value=values) 
info.fillna(value=values, limit=1) 
Вывод:

    A    B     C    D 
0  0.0  1.0   20.0  0 
1  1.0  NaN   4.0   1 
2  NaN  NaN   2.0   5 
3  NaN  20.0  NaN   2 

