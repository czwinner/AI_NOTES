[caffe.io.load_image](#caffe.io.load_image)

[caffe.io.Transformer](#caffe.io.Transformer)  

[caffe.Net](#caffe.Net)  

[caffe.set_mode_gpu()](#caffe.set_mode_gpu()) 

<div id="caffe.io.load_image"></div>

**caffe.io.load_image**

返回ndarray
caffe.io.load_image()读进来是RGB格式和0~1(float),所以在进行特征提取之前要在transformer中设置transformer.set_raw_scale('data',255)  
以及transformer.set_channel_swap('data',(2,1,0))(将RGB变换到BGR)

<div id="caffe.io.Transformer"></div>  

**caffe.io.Transformer:**  

转换输入再传入网络  

例子:  

```  
transformer=caffe.io.Transformer({'data':net.blobs['data'].data.shape})  
transformer.set_transpose('data',(2,0,1)) #python读取的图片格式HxWxC,需要转化为CxHxW  
transformer.set_mean('data',mu) # 每个通道减去均值  
transformer.set_raw_scale('data',255) #python图像[0,1],像caffeNet和AlexNet代表[0,255]所以需要输入*比例  
transformer.set_channel_swap('data',(2,1,0)) #Caffe图片是BGR格式，而原始格式是RGB，所以要转化 
```

<div id="caffe.set_mode_gpu()"></div>

**caffe.set_mode_cpu()**    #使用caffe的GPU模式

<div id="caffe.Net"></div>

**caffe.Net**  

加载模型数据  

```
net = caffe.Net(
        deploy_prototxt_path,   # 用于分类的网络定义文件路径
        caffe_model_path,       # 训练好模型路径
        caffe.TEST              # 设置为测试阶段
        )
```
