# [cv.VideoCapture](#videocapture)
# [cv.imwrite](#imwrite)
# [cv.resize](#resize)
<div id="videocapture"></div>

## cv.VideoCapture(src)
读取视频，返回一个VideoCapture对象。<br>
方法：videocapture.get(propId)<br>
作用:一个视频有很多属性，比如帧率，总帧数，尺寸，格式等。VideoCapture的get方法可以获取这些属性。<br>
参数:<br>
属性的ID。属性的ID可以是下面之一：<br>
<img src="https://github.com/czwinner/AI_NOTES/blob/master/opencv/pictures/VideoCapture_get_ID%E5%B1%9E%E6%80%A7.png" width=600px height=600px><br>
返回：所属属性的值<br>
方法:cv.VideoCapture.read()<br>
获取解码并返回下一个视频帧。该方法在一次调用中组合了VideoCapture.grab()和VideoCapture.retrive()。这是读取视频文件或从解码中捕获数据并返回刚才抓取的帧的最方便的方法。如果视频文件中没有更多帧，方法返回False和空图像。即返回一个bool值和一个ndarray<br>
方法:cv.VideoCapture.release()<br>
关闭视频文件或捕获设备<br>
<div id="imwrite"></div>

## cv.imwrite(filename,img)
将图像保存到指定的文件中。函数imwrite将图像保存到指定的文件中。<br>
参数：<br>
* filename:要保存的文件名<br>
* img:要保存的图像<br>

<div id="resize"></div>

## cv.resize(img,dsize)
调整图像<br>
参数：<br>
* img:输入图像<br>
* dsize:输出图像尺寸<br>

返回：
调整后的图像
