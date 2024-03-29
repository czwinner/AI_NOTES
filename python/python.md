## [os.path.isfile(path)](#os.path.isfile)
## [os.path.sep.join](#os.path.sep.join)
## [encode](#encode)
## [python open](#python_open)
## [python File write](#python_File_write)
## [python read()](#python_read)
## [python strip()](#python_strip)
## [python split()](#python_split)
## [python get()](#python_get)
## [python argparse](#argparse)
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

<div id="python_get"></div>

## Python字典get()方法
### 描述
Python字典get()函数返回指定键的值，如果只不在字典中返回默认值
### 语法
dict.get(key,default=None)
### 参数
* key:字典中要查找的键
* default:如果指定键的值不存在时，返回该默认值
### 返回值
返回指定键的值，如果值不在字典中返回默认值None
### 例子
```python
dict={'Name':'Ruboob','Age':27}
print("Age值为: %s" % dict.get('Age'))
print("Sex值为: %s" % dict.get('Sex','NA'))
以上实例输出结果为:
Age值为:27
Sex值为:NA
```

<div id="argparse"></div>

## python argparse()
### 创建一个解析器
使用argparse的第一步是创建一个ArgumentParser对象:<br>
```python
parser=argparse.ArgumentParser(description='Process some integers.')
```
AggumentParser对象包含将命令行解析成Python数据类型所需的全部信息。<br>
### 添加参数
给一个ArgumentParser添加程序参数信息是通过调用add_argument()方法完成的。通常，这些调用指定ArgumentParser如何获取命令行字符串并将其转换为对象。这些信息在parse_args()调用时被存储和使用。例如:<br>
```python
>>> parser.add_argument('integers', metavar='N', type=int, nargs='+',
...                     help='an integer for the accumulator')
>>> parser.add_argument('--sum', dest='accumulate', action='store_const',
...                     const=sum, default=max,
...                     help='sum the integers (default: find the max)')
```
稍后调用parse_args()将返回一个具有integers和accumulate两个属性的对象。integers属性将是一个包含一个或多个整数的列表，而accumulate属性当命令行中指定了--sum参数时将是sum()函数，否则是max()函数。<br>
### 解析参数
ArgumentParser通过parse_args()方法解析参数。它将检查命令行，把每个参数转换为适当的类型然后调用相应的操作。在大多数情况下，这意味着一个简单的Namespace对象从命令行参数中解析出的属性构建:<br>
```python
>>> parser.parse_args(['--sum', '7', '-1', '42'])
Namespace(accumulate=<built-in function sum>, integers=[7, -1, 42])
```
在脚本中，通常 parse_args() 会被不带参数调用，而 ArgumentParser 将自动从 sys.argv 中确定命令行参数。<br>
### add_argument()方法
```python
ArgumentParser.add_argument(name or flags,action,nargs,const,default,type,choices,required,help,metavar,dest)
```
* name or flags:一个命名或者一个选项字符串列表，例如foo或-f,--foo
* action:当参数在命令行中出现时使用的动作基本类型。
* nargs:命令行参数应当消耗的数目
* const:被一些action和nargs选择所需求的常数
* default:当参数未在命令行中出现时使用的值
* type:命令行参数应当被转换成的类型
* required:此命令行选项是否可省略
* help:一个选项作用的简单描述
* metavar:在使用方法消息中使用的参数值示例
* dest:被添加到parse_args()所返回对象上的属性名
