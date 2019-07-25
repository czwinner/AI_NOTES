[plt.rcParams](#rcParams)  
[plt.subplot](#subplot)
<div id="rcParams"></div>

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
