2024-09-24
#python 
[python - How can I replicate rows of a Pandas DataFrame? - Stack Overflow](https://stackoverflow.com/questions/50788508/how-can-i-replicate-rows-of-a-pandas-dataframe)
0
df.iloc[np.arange(len(df)).repeat(3)]
1
```python
newdf = pd.DataFrame(np.repeat(df.values, 3, axis=0))
newdf.columns = df.columns
```

2
```python
newdf = pd.DataFrame(np.repeat(df.values, 3, axis=0), columns=df.columns)

```

3
```python
newdf = df.loc[np.repeat(df.index, 3)].reset_index(drop=True)
```

4
```python
newdf = df.loc[df.index.repeat(3)].reset_index(drop=True)
```

5
```python
newdf = pd.concat([df] * 3).sort_index().reset_index(drop=True)
```

6
```python
newdf = df.loc[sorted([*df.index] * 3)].reset_index(drop=True)
```












