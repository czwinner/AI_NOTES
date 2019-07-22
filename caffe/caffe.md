[caffe.Net](#caffe.Net)  
[caffe.set_mode_gpu()](#caffe.set_mode_gpu())  
<div id="caffe.set_mode_gpu()"></div>
caffe.set_mode_cpu()    #使用caffe的GPU模式

<div id="caffe.Net"></div>  
caffe.Net  

加载模型数据  

```
net = caffe.Net(
        deploy_prototxt_path,   # 用于分类的网络定义文件路径
        caffe_model_path,       # 训练好模型路径
        caffe.TEST              # 设置为测试阶段
        )
```
