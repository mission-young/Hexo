title: Python 学习笔记
author: 远方
date: 2020-08-14 13:08:48
tags:
---
- `str`是`python`的内建数据类型

## Pandas Series 字符串操作

`pandas`的series内元素为`str`类型时，如果需要对整列的字符串进行变换，是无法直接调用字符串函数的，如`[]`截取操作，`count`计数，`split`分割操作等。
```python3
s = pd.Series(['A','b','C','bbhello','123',np.nan,'hj'])
```
```
0          A
1          b
2          C
3    bbhello
4        123
5        NaN
6         hj
dtype: object
```
```
type(s)
<class 'pandas.core.series.Series'>
```
如`s[0:1]`实际上截取的是`series`的第一个元素，即`A`，而不是所有数据的第一个字母组成的`series`. 使用`s.count('b')`会提示非法调用。

为了实现序列元素的批操作，可以将`series`转化为`strings.StringMethods`.
```python3
type(s.str)
<class 'pandas.core.strings.StringMethods'>
```
再对`s.str`进行字符串操作就可以成功了。如
```
s.str.count('b')
0    0.0
1    1.0
2    0.0
3    2.0
4    0.0
5    NaN
6    0.0
dtype: float64
```

如何对`Series`进行内部元素数据类型转换，可以使用`s.astype('')`来实现。


