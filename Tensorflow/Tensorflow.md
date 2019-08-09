# [TFRecordWriter](#TFRecordWriter)
# [int64_feature,bytes_feature,float_list_feature,bytes_list_feature,int64_list_feature](#int64_feature)
# [TFRecord](#TFRecord)
<div id="TFRecordWriter"></div>

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

<div id="TFRecord"></div>

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
