## [PIL Image模块](#PIL_Image)

<div id="PIL_Image"></div>

## PIL Image模块
Image模块用于表示PIL图像。该模块包括从文件加载图像和创建新图像的功能。
### 函数
PIL.Image.open(fp, mode='r')<br>
打开并标识给定的图像文件。此函数标识文件，但文件保持打开状态，并且在尝试处理数据之前不会从文件中读取实际图像数据。
### 参数
* fp:文件名(字符串），文件对象
* mode:模式。如果给出，则该参数必须为'r'
### 返回
An Image Object
### 查看实例属性
* size:以二元tuple返回图片的尺寸(width,height)
