# SSD-Tensorflow
Single Shot MultiBox Detector in TensorFlow
该方法是用train.py进行命令行训练，因为其本身是单阶段分类模型，所以train.py的框架就是用的tensorflow.slim中的分类训练框架，那么其
调用就和分类一样，notebook的形式在notebook文件夹下
输入的数据选择voc格式，用脚本转化为TFRecord格式即可进行训练,根据类名称不同，要修改pascal_common中的
字典
修改完之后，可以按照命令脚本中的方式调用函数进行训练








在调用训练脚本时会出现InvalidArgumentError (see above for traceback): Default MaxPoolingOp only supports NHWC on device type CPU
修改如下
at train_ssd_network.py line 27:
DATA_FORMAT = 'NCHW'
modify to:
DATA_FORMAT = 'NHWC'
