# [TFRecordWriter](#TFRecord_Writer)
# [int64_feature,bytes_feature,float_list_feature,bytes_list_feature,int64_list_feature](#int64_feature)
# [TFRecord](#TF_Record)
# [图和会话](#graph_session)
# [tf.gfile.Gfile](#tf.gfile.Gfile)
# [加载pb模型](#load_pb)
# [label_map_util.convert_label_map_to_categories](#label_map_util.convert_label_map_to_categories)
<div id="TFRecord_Writer"></div>

## class TFRecordWriter
将记录写入TFRecords文件的类<br>
**方法__init__**<br>
```python
__init__(
  path,
  options=None
  )
 ```
打开文件路径并创建一个写入它的TFRecordWriter<br>
参数:<br>
* path:TFRecords文件的路径<br>
* options:(可选）指定压缩类型<br>

**方法 write**<br>
write(record)<br>
将字符串记录写入文件<br>
参数：<br>
* record:str<br>

<div id="int64_feature"></div>

## int64_feature,bytes_feature,float_list_feature,bytes_list_feature,int64_list_feature
```python
In[6]: def int64_feature(value):
       		return tf.train.Feature(int64_list=tf.train.Int64List(value=[value]))
	int64_feature(32)
Out[6]: int64_list {
			value:32
		   }
In[11]: def bytes_feature(value):
			return tf.train.Feature(bytes_list=tf.train.BytesList(value=[value]))
	bytes_feature(b'32')
Out[11]: bytes_list{
			value:"32"
		   }
In[16]: def float_list_feature(value):
			return tf.train.Feature(float_list=tf.train.FloatList(value=value))
	float_list_feature([16,32])
Out[16]:float_list{
			value:16.0
			value:32.0
		  }
In[20]: def bytes_list_feature(value):
			return tf.train.Feature(bytes_list=tf.train.BytesList(value=value))
	bytes_list_feature([b'32',b'16'])
Out[20]:bytes_list{
			value:"32"
			value:"16"
		  }
In[24]: def int64_list_feature(value):
			return tf.train.Feature(int64_list=tf.train.Int64List(value=value))
	int64_list_feature([16,32])
Out[24]:int64_list{
			value:16
			value:32
		  }
```

<div id="TF_Record"</div>

## TFRecord
TFRecord文件格式是Tensorflow自己的二进制存储格式。<br>
当处理大型数据集时使用二进制文件格式存储数据会对导入性能产生重大影响，从而影响训练时间。二进制数据在磁盘上占用的空间更少，复制时间更短，可以从磁盘更有效地读取。<br>
### 构建TFRecords
TFRecord文件将数据存储为二进制字符串序列。意味着需要在将数据写入文件之前指定数据的结构。Tensorflow为此提供了两个组件:tf.train.Example和tf.train.SequenceExample。必须将每个数据样本存储在其中一个结构中，然后对其进行序列化并使用tf.python_io.TFRecordWriter将其写入磁盘。<br>
tf.train.Example不是普通的Python类，而是protocol buffer。protocol buffer是Google开发的一种方法，用于有效的方式序列化结构化数据。<br>
### 使用tf.train.Example的电影推荐
如果数据集由features组成，其中每个feature都是相同类型的值列表，则tf.train.Example是要使用的正确组件。<br>
让我们使用Tensorflow文档中的电影推荐应用程序作为示例<br>
![](https://miro.medium.com/max/1400/1*At3Y8UvzwAK1bfl5EjRQiA.jpeg)

我们有许多feature,每个feature都是一个列表，其中每个条目都具有相同的数据类型。为了将这些feature都存储在TFRecord中，我们需要创建构成feature的列表。<br>
tf.train.BytesList,tf.train.FloatList和tf.train.Int64List是tf.train.Feature的核心。这三个都有一个参数值，需要相应字节、float和int的列表。<br>
```python
movie_name_list=tf.train.BytesList(value=[b'The Shawshank Redemption',b'Fight Club'])
movie_rating_list=tf.train.FloatList(value=[9.0,9.7])
```
Python字符串需要转换为字节(例如my_string.encode('utf-8')),然后能存储在tf.train.BytesList中。<br>
tf.train.Feature包装特定类型的数据列表，以便Tensorflow可以理解它。它有一个参数，可以是bytes_list/float_list/int64_list之一。存储列表可以是tf.train.BytesList(参数名称bytes_list),tf.train.FloatList(参数名称float_list)或tf.train.Int64List(参数名称int64_list)类型。<br>
```python
movie_names=tf.train.Feature(bytes_list=movie_name_list)
movie_ratings=tf.train.Feature(float_list=movie_rating_list)
```
tf.train.Features是feature的集合，它有一个参数feature，feature需要一个字典，键是feature的名称，值是tf.train.Feature。<br>
```python
movie_dict={
	'Movie Names':movie_names,
	'Movie Ratings':movie_ratings
	}
movies=tf.train.Features(feature=movie_dict)
```
tf.train.Example是构造TFRecord的主要组件之一。它的单个参数是features,即tf.train.Features。<br>
```python
example=tf.train.Example(features=movies)
```
tf.python_io.TFRecordWriter实际上是一个Python类。它的参数path接受文件路径，并创建一个与任何其他文件对象一样工作的writer对象。TFRecordWriter类提供write,flush和close方法。方法write接受一个字符串作为参数并将其写入磁盘，这意味着必须首先序列化结构化数据。为此,tf.train.Example和tf.train.SequenceExample提供了SerializeToString方法。<br>
```python
with tf.python_io.TFRecordWriter('movie_rating.tfrecord') as writer:
	writer.write(example.SerialzeToString())
```
这是一个完整的示例，它将features写入TFRecord文件<br>
```python
In [1]:import tensorflow as tf
In [2]:data={
		'Age':29,
		'Movie':['The Shawshank Redemption','Fight club'],
		'Movie Ratings':[9.0,9.7],
		'Suggestion':'Inception',
		'Suggestion Purchased':1.0,
		'Purchase Price':9.99
		}
In [3]:data
Out[3]:{'Age':29,
	'Movie':['The Shawshank Redemption','Fight club'],
	'Movie Ratings':[9.0,9.7],
	'Suggestion':'Inception',
	'Suggestion Purchased':1.0,
	'Purchase Price':9.99}
In [4]: Age_feature=tf.train.Feature(int64_list=tf.train.Int64List(value=[data['Age']]))
        Movie_feature=tf.train.Feature(bytes_list=tf.train.BytesList(value=[i.encode('utf-8') for i in data['Movie']]))
	Movie_Rating_feature=tf.train.Feature(float_list=tf.train.FloatList(value=data['Movie Ratings']))
        Suggestion_feature=tf.train.Feature(bytes_list=tf.train.BytesList(value=[data['Suggestion'].encode('utf-8')]))
        Suggestion_Purchased_feature=tf.train.Feature(float_list=tf.train.FloatList(value=[data['Suggestion Purchased']]))
        Purchase_Price=tf.train.Feature(float_list=tf.train.FloatList(value=[data['Purchase Price']]))
In [5]: feature=tf.train.Features(feature={
                                          'Age':Age_feature,
    					  'Movie':Movie_feature,
    					  'Movie Rating':Movie_Rating_feature,
    					  'Suggestion':Suggestion_feature,
    					  'Suggestion Purchased':Suggestion_Purchased_feature,
    					  'Purchase Price':Purchase_Price
					  })
In [6]: feature
Out [6]: feature {
  		  key: "Age"
  		  value {
    			 int64_list {
      			 		value: 29
    				    }
  			}
		 }
	 feature {
  		  key: "Movie"
 		  value {
    			bytes_list {
      					value: "The Shawshank Redemption"
      					value: "Fight club"
    				   }
  			}
		}
	feature {
	  	 key: "Movie Rating"
	  	 value {
	    		float_list {
		      			value: 9.0
	      				value: 9.699999809265137
	    			   }
	  		}
		}
	feature {
	  	key: "Purchase Price"
	  	value {
	    		float_list {
	      				value: 9.989999771118164
	    			   }
	  	      }
		}
	feature {
	  	key: "Suggestion"
	  	value {
	    		bytes_list {
	      				value: "Inception"
	    			   }
	  	      }
		}
	feature {
	  	key: "Suggestion Purchased"
	  	value {
	    		float_list {
	      				value: 1.0
	    			   }
	  	      }
		}
In [7]:  example=tf.train.Example(features=feature)
In [8]:  example
Out [8]: features {
		  feature {
		    	  key: "Age"
		    	  value {
		      		 int64_list {
					     value: 29
		      			    }
		    		}
		  	  }
		  feature {
		    	  key: "Movie"
		    	  value {
		      		bytes_list {
					    value: "The Shawshank Redemption"
					    value: "Fight club"
		      			   }
		    		}
		  	  }
		  feature {
		    	  key: "Movie Rating"
		    	  value {
		      		float_list {
					    value: 9.0
					    value: 9.699999809265137
		      			   }
		    	      }
		  	  }
		  feature {
		    	key: "Purchase Price"
		        value {
		      		float_list {
					    value: 9.989999771118164
		      			   }
		    		}
		  	  }
		  feature {
		    	key: "Suggestion"
		    	value {
		      		bytes_list {
					    value: "Inception"
		      	                   }
		    	      }
		  	  }
		  feature {
		    	key: "Suggestion Purchased"
		    	value {
		      		float_list {
					    value: 1.0
		      	      		   }
		    	      }
		  	  }
		     }
In [9]:  writer=tf.python_io.TFRecordWriter('test.tfrecord')
In [10]: writer.write(example.SerializeToString())
In [11]: writer.close()
```


<div id="graph_session"></div>

## 图和会话
Tensorflow使用数据流图将计算表示为独立的指令之间的依赖关系。首先定义数据流图，然后创建Tensorflow会话，以便在一组本地和远程设备上运行图的各个部分。<br>
### 为什么使用数据流图？
数据流是一种用于并行计算的常用编程模型。在数据流图中，节点表示计算单元，边缘表示计算使用或产生的数据。例如，在Tensorflow图中，tf.matmul操作对应于单个节点，该节点具有两个传入边（要相乘的矩阵)和一个传出边（乘法结果)。<br>
### 什么事tf.Graph?
tf.Graph包含两类相关信息:<br>
* 图结构。图的节点和边缘，表示各个操作组合在一起的方式，但不规定它们的使用方式。图结构不包含源代码传达的上下文信息。<br>
* 图集合
### 构建tf.Graph
Tensorflow程序都以数据流图构建阶段开始。在此阶段，会调用Tensorflow API函数，这些函数可构建新的tf.Operation(节点)和tf.Tensor(边)对象并将它们添加到tf.Graph实例中。Tensorflow提供了一个默认图，此图是同一上下文的所有API函数的明确参数。例如:<br>
* 调用tf.constant(42.0)可创建单个tf.operation,该操作可以生成值42.0,将该值添加到默认图中，并返回表示常量值的tf.Tensor.
* 调用tf.matmul(x,y)可创建单个tf.Operation,该操作会将tf.Tensor对象x和y的值相乘，将其添加到默认图中，并返回表示乘法运算结果的tf.Tensor
* 执行v=tf.Variable(0)可向图添加一个tf.Operation。该操作可以存储一个可写入的张量值，该值在多个tf.Session.run调用之间保持恒定。tf.Variable对象会封装此操作，并可以像张量一样使用，即读取已存储值的当前值。
* 调用tf.train.Optimizer.minimize可将操作和张量添加到计算梯度的默认图中，并返回一个tf.Operation,该操作在运行时会将这些梯度应用到一组变量中。
**注意**:调用Tensorflow API中的大多数函数只会将操作和张量添加到默认图中，而不会执行实际计算。编写这些函数，直到拥有表示整个计算的tf.Tensor或tf.Operation,然后将该对象传递给tf.Session以执行计算。<br>
### 命名空间
tf.Graph对象会定义一个命名空间（为其包含的tf.Operation对象)。Tensorflow会自动为图中的每个指令选择一个唯一名称，但也可以指定描述性名称。Tensorflow API提供两种方法来覆盖操作名称:<br>
* 如果API函数会创建新的tf.Operation或返回新的tf.Tensor,则会接受name参数。例如,tf.constant(42.0,name="answer")会创建一个新的tf.Operation(名为"answer")并返回一个tf.Tensor(名为"answer:0")，如果默认图已包含名为"answer"的操作，则Tensorflow会在名称上附加"_1","_2"等字符，以便让名称具有唯一性。
* 借助tf.name_scope函数，可以向在特定上下文中创建的所有操作添加名称作用域前缀。当前名称作用域前缀是一个用"/"分隔的名称列表，其中包含所有活跃tf.name_scope上下文管理器的名称。如果某个名称作用域已在当前上下文中被占用，Tensorflow将在该作用域上附加"_1","_2"等字符。例如:
```python
c_0 = tf.constant(0, name="c") # => operation named "c_1"
# 已经使用的名称
c_1 = tf.constant(2, name="c") # => operation "c_1"
# name scopes为在同一上下文中创建的所有操作添加前缀
with tf.name_scope("outer"):
	c_2 = tf.constant(2, name="c") # => operation named "outer/c"
	# name scopes 嵌套
	with tf.name_scope("inner"):
		c_3 = tf.constant(3, name="c") # => operation named "outer/inner/c"
	# 退出 name_scope将返回上一个前缀
	c_4 = tf.constant(4, name="c") # => operation named "outer/c_1"
	# 已经使用过inner的作用域
	with tf.name_scope("inner"):
		c_5 = tf.constant(5, name="c") # => operation named "outer/inner_1/c"
```
### 在会话中执行图:tf.Session
tf.Session 对象使我们能够访问本地机器中的设备和使用分布式 TensorFlow 运行时的远程设备。它还可缓存关于 tf.Graph 的信息，使您能够多次高效地运行同一计算。
### tf.Session
可以为当前默认图创建一个 tf.Session
```python
# Create a default in-process session.
with tf.Session() as sess:
	###
```
由于 tf.Session 拥有物理资源（例如 GPU 和网络连接），因此通常（在 with 代码块中）用作上下文管理器，并在您退出代码块时自动关闭会话。您也可以在不使用 with 代码块的情况下创建会话，但应在完成会话时明确调用 tf.Session.close 以便释放资源。
### 使用 tf.Session.run 执行操作
tf.Session.run 方法是运行 tf.Operation 或评估 tf.Tensor 的主要机制。您可以将一个或多个 tf.Operation 或 tf.Tensor 对象传递到 tf.Session.run，TensorFlow 将执行计算结果所需的操作。

tf.Session.run 要求您指定一组 fetch，这些 fetch 可确定返回值，并且可能是 tf.Operation、tf.Tensor 或类张量类型，例如 tf.Variable。这些 fetch 决定了必须执行哪些子图（属于整体 tf.Graph）以生成结果：该子图包含 fetch 列表中指定的所有操作，以及其输出用于计算 fetch 值的所有操作。例如，以下代码段说明了 tf.Session.run 的不同参数如何导致执行不同的子图：
```python
x = tf.constant([[37.0, -23.0], [1.0, 4.0]])
w = tf.Variable(tf.random_uniform([2, 2]))
y = tf.matmul(x, w)
output = tf.nn.softmax(y)
init_op = w.initializer

with tf.Session() as sess:
  # Run the initializer on `w`.
  sess.run(init_op)

  # Evaluate `output`. `sess.run(output)` will return a NumPy array containing
  # the result of the computation.
  print(sess.run(output))

  # Evaluate `y` and `output`. Note that `y` will only be computed once, and its
  # result used both to return `y_val` and as an input to the `tf.nn.softmax()`
  # op. Both `y_val` and `output_val` will be NumPy arrays.
  y_val, output_val = sess.run([y, output])
```
tf.Session.run 也可以选择接受 feed 字典，该字典是从 tf.Tensor 对象（通常是 tf.placeholder 张量）到在执行时会替换这些张量的值（通常是 Python 标量、列表或 NumPy 数组）的映射。例如:<br>
```python
# Define a placeholder that expects a vector of three floating-point values,
# and a computation that depends on it.
x = tf.placeholder(tf.float32, shape=[3])
y = tf.square(x)

with tf.Session() as sess:
  # Feeding a value changes the result that is returned when you evaluate `y`.
  print(sess.run(y, {x: [1.0, 2.0, 3.0]}))  # => "[1.0, 4.0, 9.0]"
  print(sess.run(y, {x: [0.0, 0.0, 5.0]}))  # => "[0.0, 0.0, 25.0]"

  # Raises <a href="../api_docs/python/tf/errors/InvalidArgumentError"><code>tf.errors.InvalidArgumentError</code></a>, because you must feed a value for
  # a `tf.placeholder()` when evaluating a tensor that depends on it.
  sess.run(y)

  # Raises `ValueError`, because the shape of `37.0` does not match the shape
  # of placeholder `x`.
  sess.run(y, {x: 37.0})
```

<div id="tf.gfile.GFile"</div>

## tf.gfile.Gfile(filename,mode)
获取文本操作句柄，类似于python提供的文本操作open()函数，filename是要打开的文件名，mode是以何种方式去读写，将会返回一个文本操作句柄。<br>
### 方法read(n=-1)
以字符串形式返回文件的内容。从文件中的当前位置开始读取。
### 参数
* n:如果n != -1,则读取n个字节。如果n = -1则读取到文件末尾。
### 返回:
字节模式下文件的n个字节。

<div id="load_pb"></div>

## 加载pb模型
1.通过tf.gfile.Gfile打开模型。<br>
2.通过tf.GraphDef.ParseFromString得到模型中的图和变量数据。<br>
3.通过tf.import_graph_def加载目前的图<br>
4.拿到输入节点和输出节点tensor并进行预测。<br>
```python
import tensorflow as tf
def load_graph(model_dir):
with tf.gfile.Gfile(model_dir,"rb") as f: #获取模型数据
	graph_def=tf.GraphDef()
	graph_def.ParseFromString(f.read()) #得到模型中的计算图和数据
	with tf.Graph().as_default() as graph: #这里Graph()要有括号，不然会报错
		tf.import_graph_def(graph_def,name="michael") #导入模型中的图到现在这个新的计算图中
		return graph
if __name__=="__main__":
	graph=load_graph("model/pb/frozen_model.pb") #传入的是完整的路径包括pb的名字
	for op in graph.get_operations(): #打印出图中的节点信息
		print(op.name,op.values())
	x=graph.get_tensor_by_name('michael/input_holder:0') #得到输入节点tensor的名字，记得跟上导入图时指定的name
	y=graph.get_tensor_by_name('michael/prediction:0') #得到输出节点tensor的名字
	with tf.Session(graph=graph) as sess: #创建会话
		y_out=sess.run(y,feed_dict={x:[10.0]})
		print(y_out)
```
	
<div id="label_map_util.convert_label_map_to_categories"></div>

## label_map_util.convert_label_map_to_categories
### label_map_util.convert_label_map_to_categories(label_map,max_num_classes,use_display_name=True)
此函数转换标签映射proto并返回一个多字典列表，每个字典都有以下键:<br>
'id':(必需) 唯一标识此类别的整数ID<br>
'name':(必需)表示类别名称的字符串，例如'猫','狗','披萨'<br>
### 参数
* label_map:StringIntLabelMapProto或None。如果为None,则使用max_num_classes类别创建默认类别列表。
* max_num_classes:要包括的最大标签索引数
* use_display_name:(boolean)选择是否将"display_name"字段作为类别名称加载。如果为False或者display_name字段不存在。则使用"name"字段作为类别名称。
### 返回
categories:表示所有可能类别的字典列表
### 例子
```python
from object_detection.utils import label_map_util
categories=label_map_utils.convert_label_map_to_categories(labelMap,max_num_classes=3,use_display_name=True)
结果:
categories=[{'id':1,'name':'pedestrianCrossing'},
	    {'id':2,'name':'signalAhead'},
	    {'id':3,'name':'stop'}]
```
