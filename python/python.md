## [os.path.isfile(path)](#os.path.isfile)
## [os.path.sep.join](#os.path.sep.join)
<div id="os.path.isfile"></div>
os.path.isfile(path)  

参数:  
path:类路径。类路径对象可以是一个表示路径的 str 或者 bytes 对象，还可以是一个实现了 os.PathLike 协议的对象。  
如果path是现有常规文件，则返回True

<div id="os.path.sep.join"></div>
os.path.sep.join<br>
```python
ANNOT_PATH = os.path.sep.join(["lisa","allAnnotations.csv"])
```

返回连接的字符串<br>
ANNOT_PATH='lisa/allAnnotations.csv'<br>
