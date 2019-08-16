# [numpy.loadtxt](#numpy.loadtxt)  
# [numpy.random.uniform](#numpy.random.uniform)
# [numpy.expand_dims](#numpy.expand_dims)
<div id="numpy.loadtxt"></div>

## numpy.loadtxt(fname,dtype=float,comments='#',delimiter=None,converters=None,skiprows=0,usecols=None,unpack=False,ndim=0,encoding='bytes',max_rows=None)**  

从文本文件中加载数据  
**参数:**  
fname:file,str,or pathlib.Path  
      要读取的文件，文件名或生成器  
dtype:data-type,oprional  
      要生成数据的数据类型，默认是浮点数  
delimiter:str,optional  
          用于分割值的字符串。默认空格  
**返回:** out:ndarray    
    从文本文件中读取的数据

<div id="numpy.random.uniform"></div>

## numpy.random.uniform
numpy.random.uniform(low=0.0,high=1.0,size=None)<br>
从均匀分布中抽取样本。在low到high 区间内取样本，包括low,不包括high<br>
### 参数
low:float or array_like of floats,optional<br>
    输出间隔的下限<br>
high: float or tuple of ints,oprional<br>
      输出间隔的上限<br>
size: int or tuple of ints,optional<br>
      输出形状，如果给定形状(m,n,k)则绘制MxNxK个样本。如果为None,则返回单个值。<br>
### 返回
ndarray or scalar<br>
### 例子:
```python
import numpy as np
np.random.uniform(low=0.0,high=10.0,size=(2,3))
输出：
array([[2.19447438,6.93675292,6.94288899],
       [0.32986697,6.71316174,3.60521754]])
```

<div id="numpy.expand_dims"></div>

## numpy.expand_dims(a,axis)
插入新轴
### 参数
* a:array_like
输入数组
* axis:int
新轴放置的位置
### 返回
扩展后的新轴
### 例子
```python
>>>x=np.array([1,2])
>>>x.shape
(2,)
>>>y=np.expand_dims(x,axis=0)
>>>y
array([[1,2]])
>>>y.shape
(1,2)
>>>y=np.expand_dims(x,axis=1)
>>>y
array([[1],
       [2]])
>>>y.shape
(2,1)
```
