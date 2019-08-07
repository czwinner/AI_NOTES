## [os.path.isfile(path)](#os.path.isfile)
## [os.path.sep.join](#os.path.sep.join)
## [encode](#encode)
<div id="os.path.isfile"></div>

## os.path.isfile(path)  

参数:  
path:类路径。类路径对象可以是一个表示路径的 str 或者 bytes 对象，还可以是一个实现了 os.PathLike 协议的对象。  
如果path是现有常规文件，则返回True

<div id="os.path.sep.join"></div>

## os.path.sep.join

```python
ANNOT_PATH = os.path.sep.join(["lisa","allAnnotations.csv"])
```
返回连接的字符串<br>
ANNOT_PATH='lisa/allAnnotations.csv'<br>

<div id="encode"></div>

## python encode方法
### 描述
encode()方法以指定的编码格式编码字符串。errors参数可以指定不同的错误处理方案。
### 语法
```python
str.encode(encoding='UTF-8',errors='strict')
```
### 参数
* encoding:要使用的编码，如:UTF-8
* errors:设置不同错误的处理方案。默认为'strict',意为编码错误引起一个UnicodeError
### 返回值
该方法返回编码后的字符串，它是一个bytes对象
### 实例
```python
str="菜鸟教程"
str_utf8=str.encode("UTF-8")
str_gbk=str.encode("GBK")
print(str)
print("UTF-8编码: ",str_utf8)
print("GBK编码: ",str_gbk)
print("UTF-8解码: ",str_utf8.decode('UTF-8','strict'))
print("GBK解码: ",str_gbk.decode('GBK','strict'))
```
以上实例输出结果
```python
UTF-8编码: b'\xe8\x8f\x9c\xe9\xb8\x9f\xe6\x95\x99\xe7\xa8\x8b'
GBK编码: b'\xb2\xcb\xc4\xf1\xbd\xcc\xb3\xcc'
UTF-8解码：菜鸟教程
GBK解码：菜鸟教程
```
