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
