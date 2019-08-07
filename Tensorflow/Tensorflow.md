# [TFRecordWriter](#TFRecordWriter)
# [int64_feature,bytes_feature,float_list_feature,bytes_list_feature,int64_list_feature](#int64_feature)
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
