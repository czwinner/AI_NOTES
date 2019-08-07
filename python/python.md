## [os.path.isfile(path)](#os.path.isfile)
## [os.path.sep.join](#os.path.sep.join)
## [encode](#encode)
## [python open](#python open)

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

<div id="python open"></div>

## Python open()函数
### 描述
Python open()函数用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数，如果该文件无法被打开，会抛出OSError
### 注意
使用open()函数一定要保证关闭文件队形，即调用close()函数
### 完整语法格式
```python
open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```
### 参数说明
* file:必需，文件路径（相对或绝对）
* mode:可选，文件打开模式
### mode参数
* w:打开一个文件只用于写入。如果该文件已经存在则打开文件，并从靠头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。
### 实例
测试文件test.txt,内容如下:
RUNOOB1<br>
RUNOOB2
```python
>>>f=open('test.txt')
>>>f.read()
'RUNOOB1\nRUNOOB2\n'
```
