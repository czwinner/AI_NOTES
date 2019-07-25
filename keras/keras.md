[# keras.layers.Dense](#dense)
# keras Sequential模型


Sequential模型是层的线性堆栈
可以通过将层实例列表传递给构造函数来创建Sequential模型
```python
from keras.models import Sequential
from keras.layers import Dense, Activation

model = Sequential([
    Dense(32, input_shape=(784,)),
    Activation('relu'),
    Dense(10),
    Activation('softmax'),
])
```
也可以通过.add()方法简单地添加图层
```python
model = Sequential()
model.add(Dense(32, input_dim=784))
model.add(Activation('relu'))
```

<div id="dense"></div>

## keras.layers.Dense
```python
keras.layers.Dense(units, activation=None, use_bias=True, kernel_initializer='glorot_uniform', bias_initializer='zeros', kernel_regularizer=None, bias_regularizer=None, activity_regularizer=None, kernel_constraint=None, bias_constraint=None)
```
就是你常用的的全连接层。<br>
Dense 实现以下操作： output = activation(dot(input, kernel) + bias) 其中 activation 是按逐个元素计算的激活函数，kernel 是由网络层创建的权值矩阵，以及 bias 是其创建的偏置向量 (只在 use_bias 为 True 时才有用)。<br>
例子:<br>
```python
# 作为 Sequential 模型的第一层
model = Sequential()
model.add(Dense(32, input_shape=(16,)))
# 现在模型就会以尺寸为 (*, 16) 的数组作为输入，
# 其输出数组的尺寸为 (*, 32)

# 在第一层之后，你就不再需要指定输入的尺寸了：
model.add(Dense(32))
```
参数:<br>
* units:正整数，输出空间维度。<br>
* activation: 激活函数 (详见 activations)。 若不指定，则不使用激活函数 <br>
* use_bias: 布尔值，该层是否使用偏置向量。<br>
* kernel_initializer: kernel 权值矩阵的初始化器<br>
* bias_initializer: 偏置向量的初始化器<br>
* kernel_regularizer: 运用到 kernel 权值矩阵的正则化函数<br>
* bias_regularizer: 运用到偏置向的的正则化函数<br>
* activity_regularizer: 运用到层的输出的正则化函数<br>
* kernel_constraint: 运用到 kernel 权值矩阵的约束函数<br>
* bias_constraint: 运用到偏置向量的约束函数<br>
