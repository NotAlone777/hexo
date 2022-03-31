---
title: pandas烹饪指南
date: 2021-11-23 15:49:20
tags:
  - Python
categories:
  - Python
summary: pandas常用基础语法总结
---

### 1. pandas支持文件类型表格
格式类型 | 数据类型 | 读取 | 写入
:-: | :-: | :-: | :-: 
text | CSV | read_csv | to_csv
text | JSON | read_json | to_json
text | HTML | read_html | to_html
text | Local clipboard | read_clipboard | to_clipboard
binary | MS Excel | read_excel | to_excel
binary | OpenDocument | read_excel | 
binary | HDF5 Format | read_hdf | to_hdf
binary | Feather Format | read_feather | to_feather
binary | Parquet Format | read_parquet | to_parquet
binary | Msgpack | read_msgpack | to_msgpack
binary | Stata | read_stata | to_stata
binary | SAS | read_sas | 
binary | Python Pickle Format | read_pickle | to_pickle
SQL | SQL | read_sql | to_sql
SQL | Google Big Query | read_gbq | to_gbq

### 2. 惯用语
#### 2.2. 加载数据
```python
In [1]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ...:                    'BBB': [10, 20, 30, 40],
   ...:                    'CCC': [100, 50, -30, -50]})
   ...: 

In [2]: df

Out[2]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50
```
#### 2.3. if-then...
##### 在一列上执行 if-then 操作：
```python
In [3]: df.loc[df.AAA >= 5, 'BBB'] = -1

In [4]: df

Out[4]: 
   AAA  BBB  CCC
0    4   10  100
1    5   -1   50
2    6   -1  -30
3    7   -1  -50
```
##### 在两列上执行 if-then 操作：
```python
In [5]: df.loc[df.AAA >= 5, ['BBB', 'CCC']] = 555

In [6]: df

Out[6]: 
   AAA  BBB  CCC
0    4   10  100
1    5  555  555
2    6  555  555
3    7  555  555
```
##### 再添加一行代码，执行 -else 操作：
```python
In [7]: df.loc[df.AAA < 5, ['BBB', 'CCC']] = 2000

In [8]: df
Out[8]: 
   AAA   BBB   CCC
0    4  2000  2000
1    5   555   555
2    6   555   555
3    7   555   555
```
##### 或用 Pandas 的 where 设置掩码（mask）：
```python
In [9]: df_mask = pd.DataFrame({'AAA': [True] * 4,
   ...:                         'BBB': [False] * 4,
   ...:                         'CCC': [True, False] * 2})
   ...: 

In [10]: df.where(df_mask, -1000)
Out[10]: 
   AAA   BBB   CCC
0    4 -1000  2000
1    5 -1000 -1000
2    6 -1000   555
3    7 -1000 -1000
```
##### 用 NumPy where() 函数实现 if-then-else
```python
In [11]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ....:                    'BBB': [10, 20, 30, 40],
   ....:                    'CCC': [100, 50, -30, -50]})
   ....: 

In [12]: df
Out[12]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

In [13]: df['logic'] = np.where(df['AAA'] > 5, 'high', 'low')

In [14]: df
Out[14]: 
   AAA  BBB  CCC logic
0    4   10  100   low
1    5   20   50   low
2    6   30  -30  high
3    7   40  -50  high
```
#### 2.4. 切割
##### 用布尔条件切割 DataFrame
```python
In [15]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ....:                    'BBB': [10, 20, 30, 40],
   ....:                    'CCC': [100, 50, -30, -50]})
   ....: 

In [16]: df
Out[16]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

In [17]: df[df.AAA <= 5]
Out[17]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50

In [18]: df[df.AAA > 5]
Out[18]: 
   AAA  BBB  CCC
2    6   30  -30
3    7   40  -50
```
#### 2.5. 设置条件
##### 多条件选择
```python
In [15]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ....:                    'BBB': [10, 20, 30, 40],
   ....:                    'CCC': [100, 50, -30, -50]})
   ....: 

In [16]: df
Out[16]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

In [17]: df[df.AAA <= 5]
Out[17]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50

In [18]: df[df.AAA > 5]
Out[18]: 
   AAA  BBB  CCC
2    6   30  -30
3    7   40  -50
```
##### 和（&），不赋值，直接返回 Series：
```python
In [21]: df.loc[(df['BBB'] < 25) & (df['CCC'] >= -40), 'AAA']
Out[21]: 
0    4
1    5
Name: AAA, dtype: int64
```
##### 或（|），不赋值，直接返回 Series：
```python
In [22]: df.loc[(df['BBB'] > 25) | (df['CCC'] >= -40), 'AAA']
Out[22]: 
0    4
1    5
2    6
3    7
Name: AAA, dtype: int64
```
##### 或（|），赋值，修改 DataFrame：
```python
In [23]: df.loc[(df['BBB'] > 25) | (df['CCC'] >= 75), 'AAA'] = 0.1

In [24]: df
Out[24]: 
   AAA  BBB  CCC
0  0.1   10  100
1  5.0   20   50
2  0.1   30  -30
3  0.1   40  -50
```
#### 2.6. 用 argsort 选择最接近指定值的行
```python
In [25]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ....:                    'BBB': [10, 20, 30, 40],
   ....:                    'CCC': [100, 50, -30, -50]})
   ....: 

In [26]: df
Out[26]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

In [27]: aValue = 43.0

In [28]: df.loc[(df.CCC - aValue).abs().argsort()]
Out[28]: 
   AAA  BBB  CCC
1    5   20   50
0    4   10  100
2    6   30  -30
3    7   40  -50
```
#### 2.7. 用二进制运算符动态减少条件列表
```python
In [29]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ....:                    'BBB': [10, 20, 30, 40],
   ....:                    'CCC': [100, 50, -30, -50]})
   ....: 

In [30]: df
Out[30]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

In [31]: Crit1 = df.AAA <= 5.5

In [32]: Crit2 = df.BBB == 10.0

In [33]: Crit3 = df.CCC > -40.0
```
##### 硬编码方式为：
```python
In [34]: AllCrit = Crit1 & Crit2 & Crit3
```
##### 生成动态条件列表：
```python
In [35]: import functools

In [36]: CritList = [Crit1, Crit2, Crit3]

In [37]: AllCrit = functools.reduce(lambda x, y: x & y, CritList)

In [38]: df[AllCrit]
Out[38]: 
   AAA  BBB  CCC
0    4   10  100
```
### 3. 选择
#### 3.1. 行标签与值作为条件
```python
In [39]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ....:                    'BBB': [10, 20, 30, 40],
   ....:                    'CCC': [100, 50, -30, -50]})
   ....: 

In [40]: df
Out[40]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

In [41]: df[(df.AAA <= 6) & (df.index.isin([0, 2, 4]))]
Out[41]: 
   AAA  BBB  CCC
0    4   10  100
2    6   30  -30
```
#### 3.2. 标签切片用 loc，位置切片用 iloc
```python
In [42]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ....:                    'BBB': [10, 20, 30, 40],
   ....:                    'CCC': [100, 50, -30, -50]},
   ....:                   index=['foo', 'bar', 'boo', 'kar'])
   ....: 
```
前 2 个是显式切片方法，第 3 个是通用方法：
1. 位置切片，Python 切片风格，不包括结尾数据；
2. 标签切片，非 Python 切片风格，包括结尾数据；
3. 通用切片，支持两种切片风格，取决于切片用的是标签还是位置。

```python
In [43]: df.loc['bar':'kar']  # Label
Out[43]: 
     AAA  BBB  CCC
bar    5   20   50
boo    6   30  -30
kar    7   40  -50

# Generic
In [44]: df.iloc[0:3]
Out[44]: 
     AAA  BBB  CCC
foo    4   10  100
bar    5   20   50
boo    6   30  -30

In [45]: df.loc['bar':'kar']
Out[45]: 
     AAA  BBB  CCC
bar    5   20   50
boo    6   30  -30
kar    7   40  -50
```
##### 包含整数，且不从 0 开始的索引，或不是逐步递增的索引会引发歧义。
```python
In [46]: data = {'AAA': [4, 5, 6, 7],
   ....:         'BBB': [10, 20, 30, 40],
   ....:         'CCC': [100, 50, -30, -50]}
   ....: 

In [47]: df2 = pd.DataFrame(data=data, index=[1, 2, 3, 4])  # Note index starts at 1.

In [48]: df2.iloc[1:3]  # Position-oriented
Out[48]: 
   AAA  BBB  CCC
2    5   20   50
3    6   30  -30

In [49]: df2.loc[1:3]  # Label-oriented
Out[49]: 
   AAA  BBB  CCC
1    4   10  100
2    5   20   50
3    6   30  -30
```
#### 3.3. 用逆运算符 (~)提取掩码的反向内容
```python
In [50]: df = pd.DataFrame({'AAA': [4, 5, 6, 7],
   ....:                    'BBB': [10, 20, 30, 40],
   ....:                    'CCC': [100, 50, -30, -50]})
   ....: 

In [51]: df
Out[51]: 
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

In [52]: df[~((df.AAA <= 6) & (df.index.isin([0, 2, 4])))]
Out[52]: 
   AAA  BBB  CCC
1    5   20   50
3    7   40  -50
```
#### 3.4. 用 applymap 高效动态生成新列
```python
In [53]: df = pd.DataFrame({'AAA': [1, 2, 1, 3],
   ....:                    'BBB': [1, 1, 2, 2],
   ....:                    'CCC': [2, 1, 3, 1]})
   ....: 

In [54]: df
Out[54]: 
   AAA  BBB  CCC
0    1    1    2
1    2    1    1
2    1    2    3
3    3    2    1

In [55]: source_cols = df.columns   # Or some subset would work too

In [56]: new_cols = [str(x) + "_cat" for x in source_cols]

In [57]: categories = {1: 'Alpha', 2: 'Beta', 3: 'Charlie'}

In [58]: df[new_cols] = df[source_cols].applymap(categories.get)

In [59]: df
Out[59]: 
   AAA  BBB  CCC  AAA_cat BBB_cat  CCC_cat
0    1    1    2    Alpha   Alpha     Beta
1    2    1    1     Beta   Alpha    Alpha
2    1    2    3    Alpha    Beta  Charlie
3    3    2    1  Charlie    Beta    Alpha
```
#### 3.5. 分组时用 min()
```python
In [60]: df = pd.DataFrame({'AAA': [1, 1, 1, 2, 2, 2, 3, 3],
   ....:                    'BBB': [2, 1, 3, 4, 5, 1, 2, 3]})
   ....: 

In [61]: df
Out[61]: 
   AAA  BBB
0    1    2
1    1    1
2    1    3
3    2    4
4    2    5
5    2    1
6    3    2
7    3    3
```
##### 方法1：用 idxmin() 提取每组最小值的索引
```python
In [62]: df.loc[df.groupby("AAA")["BBB"].idxmin()]
Out[62]: 
   AAA  BBB
1    1    1
5    2    1
6    3    2
```
##### 方法 2：先排序，再提取每组的第一个值
```python
In [63]: df.sort_values(by="BBB").groupby("AAA", as_index=False).first()
Out[63]: 
   AAA  BBB
0    1    1
1    2    1
2    3    2
```
> 注意，提取的数据一样，但索引不一样。