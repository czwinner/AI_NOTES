[numpy.loadtxt](#numpy.loadtxt)  

<div id="numpy.loadtxt"></div>  
**numpy.loadtxt(fname,dtype=float,comments='#',delimiter=None,converters=None,skiprows=0,usecols=None,unpack=False,ndim=0,encoding='bytes',max_rows=None)**  

从文本文件中加载数据  
**参数:**  
fname:file,str,or pathlib.Path  
      要读取的文件，文件名或生成器  
dtype:data-type,oprional  
      要生成数据的数据类型，默认是浮点数  
delimiter:str,optional  
          用于分割值的字符串。默认空格  
**返回:** out:ndarray    
    从文本文件中读取的数据
