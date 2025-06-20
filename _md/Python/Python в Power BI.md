2024-09-11
#python #PowerBI

можно делать загрузку через скрипты питон. при этом подгружать библиотеки
в конце обязательно print(df)

пример
import pandas as pd
import numpy as np
df = pd.read_excel(r'\\mos.tmk.group\files\PUBLIC\014\DISPETCHERIZATSIYA\ВТЗ для заполнения\АО ВТЗ для заполнения.xlsm', sheet_name='рапорт ДГ', skiprows=2, skipfooter=5)
b = []
tmp = ''
for val in df.iloc[0]:
    if val is not np.nan:
        tmp = val
        b.append(val)
    else:
        b.append(tmp)
c = []
for i in list(zip(b, list(df.iloc[1]))):
    if i[1] is not np.nan:
        c.append(i[0].replace(r'\n', '') + ' ' + i[1])
    else:
        c.append(i[0])
df.columns = c
df = df.drop(index=[0, 1])
df['Цех'] = df['Цех'].fillna(method='ffill')
df.dropna(axis=0, how='all', inplace=True)
df.dropna(axis=1, how='all', inplace=True)
df['Цех - показатели'] = df['Цех'] + ' ' + df['Производственные показатели']
df.drop(columns=['Цех', 'Производственные показатели'], inplace=True)
print(df)