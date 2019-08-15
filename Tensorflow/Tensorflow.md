# [TFRecordWriter](#TFRecord_Writer)
# [int64_feature,bytes_feature,float_list_feature,bytes_list_feature,int64_list_feature](#int64_feature)
# [TFRecord](#TF_Record)
# [图和会话](#图和会话)

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

<div id="图和会话"></div>

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
