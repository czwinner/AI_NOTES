## [os.path.isfile(path)](#os.path.isfile)
## [os.path.sep.join](#os.path.sep.join)
## [encode](#encode)
## [python open](#python_open)
## [python File write](#python_File_write)
## [python read()](#python_read)
## [python strip()](#python_strip)
## [python split()](#python_split)
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

<div id="python_open"></div>

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

<div id="python_File_write"></div>

## Python File write()方法
### 概述
write()方法用于向文件红写入指定字符串。<br>
在文件关闭前或缓冲区刷新前，字符串内容存储在缓冲区宗，这时在文件中看不到写入的内容。<br>
如果文件打开模式带b,那写入内容时，str(参数)要用encode方法转为bytes形式<br>
### 语法
fileObject.write(str)
### 参数
* str:要写入文件的字符串
### 返回值
写入的字符长度
### 例子：
```python
f=open("test.txt",'w')
f.write('Hello Python\nHello Java!')
f.close()
```
打开test.txt<br>
Hello Python<br>
Hello Java!<br>

<div id="python_read"</div>

## Python read()
read()每次读取整个文件，通常用于将文件内容放到一个字符串变量中。<br>
### 例子
test.txt文件内容如下:<br>
这是第一行<br>
这是第二行<br>
这是第三行<br>
```python
f=open(file="test.txt")
line=f.read()
type(line) #输出str
line #输出'这是第一行\n这是第二行\n这是第三行\n'
```

<div id="python_strip"></div>

## Python strip()
### 描述
Python strip()方法用于移除字符串头尾指定的字符（默认为空格）或字符序列。<br>
### 注意
该方法只能删除开头或是结尾的字符，不能删除中间部分的字符。<br>
### 语法
str.strip(chars)
### 参数
* chars:移除字符串头尾指定的字符序列
### 返回值
返回移除字符串头尾指定的字符序列生成的新字符串
### 例子
```python
str="**czwinner**"
str.strip('*')
输出如下:
'czwinner'
```

<div id="python_split"></div>

## Python split()方法
### 描述
split()通过指定分隔符对字符串进行切片，如果第二个参数maxsplit有指定值，则分割为maxsplit+1个子字符串
### 语法
```python
str.split(sep=None, maxsplit=-1)
```
### 参数
* sep:分隔符，默认为所有的空字符串，包括空格、换行(\n)、制表符(\t)等
* maxsplit:分割次数。默认为-1,即分隔所有
### 返回值
分割后的字符串列表
### 例子
```python
b="this is a very pretty girl!"
b.split() #返回['this','is','a','very','pretty','girl!']
b.split(maxsplit=1) # 返回['this','is a very pretty girl!']
```
