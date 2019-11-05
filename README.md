# SSD-Tensorflow
Single Shot MultiBox Detector in TensorFlow
该方法是用train.py进行命令行训练，因为其本身是单阶段分类模型，所以train.py的框架就是用的tensorflow.slim中的分类训练框架，那么其
调用就和分类一样，notebook的形式在notebook文件夹下


输入的数据选择voc格式，用脚本转化为TFRecord格式即可进行训练,根据类名称不同，要修改pascal_common中的
字典
修改完之后，可以按照命令脚本中的方式调用函数进行训练






在调用训练脚本时会出现

! InvalidArgumentError (see above for traceback): Default MaxPoolingOp only supports NHWC on device type CPU
修改如下

    at train_ssd_network.py line 27:
    DATA_FORMAT = 'NCHW'
    modify to:
    DATA_FORMAT = 'NHWC'



网络结构在nets.ssd_vgg_300.SSDNet()中的net定义，要修改网络结构直接修改这里即可，用其他类型的网络可对其结构进行重写

对于损失的定义，则是在nets.ssd_vgg_300.SSDNet()中的ssd_losses，因为它是对每层特征图结构进行reshape后计算损失，所以可以针对最后的
给定的损失结构修改





网络的输出是针对每个图输出张量形式的结果，不是像其他类型的统一，针对每个特征图生成预测的2个张量形式
当计算损失的时候是将张量展开成每列一个anchor的形式，用mask来计算每个anchor的损失在进行加和求平均

算法本身在anchor的生成和匹配部分不够高效，生成的anchors形式是[[y,x,w,h],[y,x,w,h],[y,x,w,h],…………]y,x表示特定特征图上每个anchor的坐标值，是标准化后的矩阵形式，h,w则是每个位置anchor的大小，是个列表形式。每个特征图一组。而在匹配的时候是用所有的GT和该层特征图上的anchor进行匹配，而不是和所有的anchor进行一次完整的匹配。匹配完成后就用结构构造相应特征图的输出张量
