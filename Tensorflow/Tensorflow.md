# [TFRecordWriter](#TFRecordWriter)
<div id="TFRecordWriter"></div>

## class TFRecordWriter
将记录写入TFRecords文件的类<br>
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
方法 write<br>
write(record)<br>
将字符串记录写入文件<br>
参数：<br>
* record:str<br>
