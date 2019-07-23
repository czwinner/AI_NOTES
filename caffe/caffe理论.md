caffe.net.blobs是一个Orderdict,由每一层的名称和Blob组成。  

每一个blob里面又包含data和diff值。例如要取data层的data数据，则应该写成net.blobs['data'].data
