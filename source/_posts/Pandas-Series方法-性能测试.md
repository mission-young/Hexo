title: Pandas Series方法 性能测试
author: 远方
date: 2020-08-14 14:08:56
tags:
---
对于pandas的Series数据，进行整体的数据变换是常见的事，包括类型转换，字符串切割，数据计算等等，如何高性能地完成这些数据变换，对于大规模的数据处理至关重要。

``` python3
import numpy as np
import pandas as pd
import timeit

a = pd.Series(range(1000))

# %timeit a.apply(lambda x:x+2)  385us

def func1(x):
	return x + 2

# %timeit func1(a) 88us

# %timeit a.astype('str') 524us

def func2(x):
	return str(x)

# %timeit func2(a) 383us

b = pd.Series(['abcdefg' for i in range(1000)])

# %timeit b.str[3:5]  287us

def func3(x):
	return x[3:5]
    
# func3(b) mistake

# 3    abcdefg
# 4    abcdefg
# dtype: object
```
### series作为函数参数传入
series作为函数参数传入，由于未指定函数参数类型，因而该函数可以传入标量，也可以传入向量，传入向量则为向量运算。但需要注意的是，对于`[]`切片运算，如果传入的是向量，并不会对内部标量进行切割，而是对向量本身进行切割，这也导致了执行`func3(b)`的时候，没能达成我们想要的目标。由于是向量运算，这种方式执行的速度比较快。
### series作为主体，调用函数
series作为主题，通过`.func`的形式调用函数，这种方式的速度较慢，耗时较长。尤其是通常使用的`.astype`和`.apply`，会降低运行效率。