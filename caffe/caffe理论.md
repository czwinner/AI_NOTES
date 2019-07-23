caffe.net.blobs是一个Orderdict,由每一层的名称和Blob组成

![img](https://github.com/czwinner/AI_NOTES/blob/master/caffe/pictures/caffe_net_blobs.png) 

每一个blob里面又包含data和diff值。例如要取data层的data数据，则应该写成net.blobs['data'].data  

net.params是一个Ordereddict,键是各层的名称，值是Blob,Blob[0]是权重参数，Blob[1]是偏置值
