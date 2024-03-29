# [plt.grid](#grid)
# [plt.imshow](#imshow)
# [plt.rcParams](#rcParams)  
# [plt.subplot](#subplot)
# [plt.xlabel](#xlabel)
# [plt.xticks()](#xticks)

<div id="grid"></div> 

## plt.grid <br>
```python
plt.grid(True) # 显示背景的网格线<br>
plt.grid(False) # 关闭背景的网格线<br> 
ax.grid(color='r', linestyle='-', linewidth=2) # 设置背景网格线的样式<br>
```
<div id="imshow"></div>

## plt.imshow

plt.imshow(X,cmap=None)<br>
**参数:**<br>
X:要绘制的图像或数组<br>
camp:颜色图谱(colormap),默认绘制为RGB颜色空间<br>
用的比较多的有gray,jet等，如<br>
```python
plt.imshow(image,plt.cm.gray)
plt.imshow(image,cmap=plt.cm.jet)
```
**返回**:
一个AxesImage对象<br>
```diff
- 注意: plt.imshow()函数负责对图像进行处理，但是不能显示，其后跟着plt.show()才能显示出来
```
<div id="rcParams"></div> 

## plt.rcParams 

```python
plt.rcParams['figure.figsize'] = (8.0, 4.0) # 设置figure_size尺寸  
plt.rcParams['image.interpolation'] = 'nearest' # 设置 interpolation style  
plt.rcParams['image.cmap'] = 'gray' # 设置 颜色 style
```


<div id="subplot"></div> 

## plt.subplot(*args,**args):

```
plt.subplot(*args,**args):
```  

返回给定网格位置的子图<br>
例子:<br> 
```python
for i in range(25):  
	plt.subplot(5,5,i+1)
``` 

表示一共5行5列，分别在第0个位置到第24个位置画坐标轴
<div id="xlabel"></div>

## plt.xlabel<br>
```python
plt.xlabel(xlabel,fontdict=None,labelpad=None,**kwargs)
```
设置x轴的标签<br>
**参数:**<br>
xlabel:str<br>
标签文字<br>

<div id="xticks"></div>

## plt.xticks()<br/>
获取或设置当前刻度线位置和x轴标签<br/>
**参数**<br/>
locs:array_like<br/>
应该放置刻度的位置列表。通过空列表禁用xticks<br/>
labels:array_like,optional<br/>
要放置在给定locs的标签列表<br/>
**返回:**<br/>
locs:一系列标签列表<br/>
labels:一个'.Text'对象列表<br/>
**例子：**<br/>
```python
import matplotlib.pyplot as plt
import numpy as np
plt.rcParams['font.sans-serif']=['SimHei'] #设置中文显示
plt.xticks(np.arange(5),('Tom','Dick','Harry','Sally','Sue'))
plt.yticks(np.arange(5),('汽车','飞机','大炮','轮船','高铁'))
```
<img width="600px" height="400px"  src="https://github.com/czwinner/AI_NOTES/blob/master/matplotlib/pictures/plt.xticks.jpg"/>
