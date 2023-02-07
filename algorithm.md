# 环境搭建

## 本地环境

###Anaconda

**Anaconda用于创建不同项目的独立python环境**

安装Anaconda时，勾选将路径添加到环境变量（便于终端调用寻径）

安装目录：`D:/ANCONDA`

```
conda create -n torch_1_12 python=3.7
```

```
# To activate this environment, use
#
#     $ conda activate torch_1_12
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```

### pyTorch

**PyTorch是一个基于Torch的开源Python机器学习库**

> https://pytorch.org/
>
> 需安装torch torchvision
>
> <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220820170459683.png" alt="image-20220820170459683" style="zoom:67%;" />

torch-vision文件较小，启动虚拟环境后直接在外网安装

```
pip install torch-vision
```

外网下载文件速度较慢 → 本地安装torch

引航计划 自动驾驶软件 torch-1.12.0+cpu-cp37-cp37m-win_amd64.whl

cmd → 启动虚拟环境 → pip install 文件名（文件名的输入可以通过拖拽文件解决）

安装后在虚拟环境中调用python解释器测试：

```
(torch_1_12) C:\Users\liufangfei>python
Python 3.7.13 (default, Mar 28 2022, 08:03:21) [MSC v.1916 64 bit (AMD64)] :: Anaconda, Inc. on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.__version__\
...
'1.12.0+cpu'
```

> 安装中pip版本未更新警告：
>
> ```
> WARNING: There was an error checking the latest version of pip.
> 
> (torch_1_12) C:\Users\liufangfei>pip install --upgrade pip
> Requirement already satisfied: pip in d:\anaconda\envs\torch_1_12\lib\site-packages (22.1.2)
> Collecting pip
>   Downloading pip-22.2.2-py3-none-any.whl (2.0 MB)
>      ---------------------------------------- 2.0/2.0 MB 1.9 MB/s eta 0:00:00
> ERROR: To modify pip, please run the following command:
> D:\ANACONDA\envs\torch_1_12\python.exe -m pip install --upgrade pip
> ```

> 其他需用库也在虚拟环境中使用pip安装
>
> ```
> pip install tqdm torchsummary
> pip install scikit-image
> pip install opencv-python==3.4.2.17
> ```

### VSCode

**VSCode是一款代码编辑器**

在VScode中可以使用/编辑多种汇编语言，但必须为语言安装对应的扩展

① 安装python对应扩展

② 设置python解释器

左下角⚙→设置 → 搜索python interpreter → 更改default python interpreter path

```
// 查找python解释器路径
(torch_1_12) C:\Users\liufangfei>where python
D:\ANACONDA\envs\torch_1_12\python.exe
D:\ANACONDA\python.exe
```

![image-20220820190131856](C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220820190131856.png)

![image-20220820190343115](C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220820190343115.png)

> 用户：给用户所有目录设置解释器
>
> 工作区：针对当前工作目录设置解释器
>
> > 解释器是虚拟环境中的解释器
>
> <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220820191114865.png" alt="image-20220820191114865" style="zoom:67%;" />

> 安装 Markdown 扩展插件 → 阅读 readme

> 附：PyCharm中配置解释器
>
> File → Settings → Project → Python Interpreter → ⚙（Add：可添加conda中的虚拟环境等）

------------------------

小结：

① 硬件：下载Anconda/VScode

② Anconda创建虚拟环境，在虚拟环境中下载项目所需库

③ VScode中搭建python开发平台：

​	安装python扩展 → 扩展中配置python解释器 → 新建test.py运行测试

-----------------------

## 超算环境

### sugon

① 申请算力资源

② 进入中科曙光用户页面配置超算环境

> **1 申请算力：**选择异构计算
>
> <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220822114109826.png" alt="image-20220822114109826" style="zoom:67%;" />
>
> **2 登录中科曙光用户页面：**https://ac.sugon.com
>
> **3 安装并配置Anaconda：**shell终端/图形文件操作界面 → 查看文件目录 → 安装Linux端Anaconda
>
> > 文件夹：home → 用户名 → （新建）soft → （新建）Anaconda3 → 将安装包上传到此文件夹中
> >
> > ```
> > // 切换到安装目录 授予执行权限
> > [acavbgmcz3@login02 ~]$ cd soft
> > [acavbgmcz3@login02 soft]$ cd Anaconda3/
> > [acavbgmcz3@login02 Anaconda3]$ chmod +x Anaconda3-2022.05-Linux-x86_64.sh
> > ```
> >
> > ```
> > // 运行安装文件
> > [acavbgmcz3@login02 Anaconda3]$ ./Anaconda3-2022.05-Linux-x86_64.sh
> > ```
> >
> > Anocanda环境配置：环境变量
> >
> > > sugon官方帮助文档：https://ac.sugon.com/doc/1.0.6/11250/general-handbook/compile/Anaconda.html
> >
> > ```
> > // 打开bashrc文件并编辑
> > [acavbgmcz3@login08 ~]$ nano ~/.bashrc
> > // 编辑界面加入语句，路径改为自己的 Anoconda bin 目录
> > // export PATH=/public/home/username/soft/Anaconda3/bin:$PATH
> > ```
> >
> > 初始化并激活配置：
> >
> > ```
> > ~/anaconda3/bin/conda init
> > source ~/.bashrc
> > ```
> >
> > 检验安装是否成功：
> >
> > ```
> > [acavbgmcz3@login08 ~]$ which conda
> > ~/anaconda3/bin/conda
> > [acavbgmcz3@login08 ~]$ conda
> > usage: conda [-h] [-V] command ...
> > 
> > conda is a tool for managing and deploying applications, environments and packages.
> > 
> > Options:
> > 
> > positional arguments:
> >   command
> >     clean        Remove unused packages and caches.
> >     compare      Compare packages between conda environments.
> >     config       Modify configuration values in .condarc. This is modeled after the git config command. Writes to the user .condarc file
> >                  (/public/home/acavbgmcz3/.condarc) by default.
> >     create       Create a new conda environment from a list of specified packages.
> >     help         Displays a list of available conda commands and their help strings.
> >     info         Display information about current conda install.
> >     init         Initialize conda for shell interaction. [Experimental]
> >     install      Installs a list of packages into a specified conda environment.
> >     list         List linked packages in a conda environment.
> >     package      Low-level conda package utility. (EXPERIMENTAL)
> >     remove       Remove a list of packages from a specified conda environment.
> >     uninstall    Alias for conda remove.
> >     run          Run an executable in a conda environment.
> >     search       Search for packages and display associated information. The input is a MatchSpec, a query language for conda packages. See examples
> >                  below.
> >     update       Updates conda packages to the latest compatible version.
> >     upgrade      Alias for conda update.
> > 
> > optional arguments:
> >   -h, --help     Show this help message and exit.
> >   -V, --version  Show the conda version number and exit.
> > 
> > conda commands available from other packages:
> >   build
> >   convert
> >   debug
> >   develop
> >   env
> >   index
> >   inspect
> >   metapackage
> >   render
> >   server
> >   skeleton
> > ```
>
> **4 下载并配置pyTorch库及其依赖：**
>
> > https://ac.sugon.com/doc/1.0.6/11250/general-handbook/compile/PyTorch1.8&1.9.html
>
> 创建并激活虚拟环境
>
> ```
> conda create -n pytorch-1.9 python=3.6
> conda activate pytorch-1.9
> ```
>
> 安装pyTorch包（路径为公共目录下包的位置）
>
> ```
> pip install /public/software/apps/DeepLearning/whl/rocm-4.0.1/torch-1.9.0+rocm4.0.1-cp36-cp36m-linux_x86_64.whl -i https://pypi.tuna.tsinghua.edu.cn/simple/
> ```
>
> 将公共目录中torchvision包移动到用户目录下，使用pip安装
>
> /public/software/apps/DeepLearning/whl/rocm-4.0.1/torchvision-0.10.0a0-cp36-cp36m-linux_x86_64.whl
>
> ```
> cd
> pip install /public/software/apps/DeepLearning/whl/rocm-4.0.1
> ```
>
> > 提前将安装包在用户目录中准备好是为了缓解安装速度慢（外网）
> >
> > 安装超时报错，将pillow包传到用户文件夹内，手动安装pillow包
> >
> > ```
> > pip install Pillow-8.4.0-cp36-cp36m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
> > ```
> >
> > 再执行命令安装torchvision
> >
> > ```
> > pip install torchvision-0.10.0a0-cp36-cp36m-linux_x86_64.whl
> > ```
>
> **5 申请GPU资源：**
>
> ```
> salloc -p kshdtest -N 1 --gres=dcu:4 --cpus-per-task=6
> ```
>
> ```
> (pytorch-1.9) [acavbgmcz3@login04 ~]$ salloc -p kshdtest -N 1 --gres=dcu:4 --cpus-per-task=6
> salloc: Pending job allocation 23325988
> salloc: job 23325988 queued and waiting for resources
> salloc: job 23325988 has been allocated resources
> salloc: Granted job allocation 23325988
> salloc: Waiting for resource configuration
> salloc: Nodes i02r1n17 are ready for job
> ```
>
> ```
> // squeue 查看排队信息&申请的超算编号（下为i02r1n17）
> (base) [acavbgmcz3@login04 ~]$ squeue
>              JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
>           23325988  kshdtest     bash acavbgmc  R       1:16      1 i02r1n17
> ```
>
> **6 在超算中训练模型：**
>
> ```
> // 连接到超算
> ssh i02r1n17
> ```
>
> 文件包\1_segmentation_sugon上载到超算，执行脚本文件submit（注意更改参数配置：虚拟环境名）
>
> ```
> cd 1_segmentation_sugon
> cd unet_torch
> ./submit.sh
> ```
>
> 



# 图像分割模型

## 数据处理

① 采集到的图片数据读入电脑

② 百度EasyDL对数据进行标记

③ 将图像处理成训练可以使用的灰度图像

> 工作目录切换至dataset，配置解释器，安装所需包
>
> ```
> pip install opencv-python==3.4.2.17
> ```
>
> 运行tempt.py查看标签数据rgb值
>
> 数据文件夹放在与运行处理程序相同的文件夹
>
> > 数据文件夹中需要手动创建一个Masks文件夹（小bug）
>
> ```
> conda activate torch_1_12
> python mask_rgb_to_gray.py
> ```

## 模型训练

cmd中：

① 切换到文件目录所在位置

② 启用虚拟环境

③ `python train.py` 开始训练

```
// 参数说明
> python train.py -h
usage: train.py [-h] [--epochs E] [--batch-size B] [--learning-rate LR]
                [--load LOAD] [--scale SCALE] [--validation VAL] [--amp]

Train the UNet on images and target masks

optional arguments:
  -h, --help            show this help message and exit
  --epochs E, -e E      Number of epochs
  --batch-size B, -b B  Batch size
  --learning-rate LR, -l LR
                        Learning rate
  --load LOAD, -f LOAD  Load model from a .pth file
  --scale SCALE, -s SCALE
                        Downscaling factor of the images
  --validation VAL, -v VAL
                        Percent of the data that is used as validation (0-100)
  --amp                 Use mixed precision
  --class               the number of lable class
```

```
cd D:\SDUer\2022summer_driverless\1_segmentation\unet_torch
conda activate torch_1_12
pip install numpy pillow
python train.py -e 2 -b 1 -l 0.0001 -c 3
```

```
(torch_1_12) D:\SDUer\2022summer_driverless\1_segmentation\unet_torch>python train.py -e 2 -b 1 -l 0.0001 -c 3
INFO: Using device cpu
INFO: Network:
        3 input channels
        3 output channels (classes)
        Transposed conv upscaling
----------------------------------------------------------------
        Layer (type)               Output Shape         Param #
================================================================
            Conv2d-1          [4, 16, 800, 600]             432
       BatchNorm2d-2          [4, 16, 800, 600]              32
              ReLU-3          [4, 16, 800, 600]               0
            Conv2d-4          [4, 16, 800, 600]           2,304
       BatchNorm2d-5          [4, 16, 800, 600]              32
              ReLU-6          [4, 16, 800, 600]               0
        DoubleConv-7          [4, 16, 800, 600]               0
         MaxPool2d-8          [4, 16, 400, 300]               0
            Conv2d-9          [4, 32, 400, 300]           4,608
      BatchNorm2d-10          [4, 32, 400, 300]              64
             ReLU-11          [4, 32, 400, 300]               0
           Conv2d-12          [4, 32, 400, 300]           9,216
      BatchNorm2d-13          [4, 32, 400, 300]              64
             ReLU-14          [4, 32, 400, 300]               0
       DoubleConv-15          [4, 32, 400, 300]               0
             Down-16          [4, 32, 400, 300]               0
        MaxPool2d-17          [4, 32, 200, 150]               0
           Conv2d-18          [4, 64, 200, 150]          18,432
      BatchNorm2d-19          [4, 64, 200, 150]             128
             ReLU-20          [4, 64, 200, 150]               0
           Conv2d-21          [4, 64, 200, 150]          36,864
      BatchNorm2d-22          [4, 64, 200, 150]             128
             ReLU-23          [4, 64, 200, 150]               0
       DoubleConv-24          [4, 64, 200, 150]               0
             Down-25          [4, 64, 200, 150]               0
        MaxPool2d-26           [4, 64, 100, 75]               0
           Conv2d-27          [4, 128, 100, 75]          73,728
      BatchNorm2d-28          [4, 128, 100, 75]             256
             ReLU-29          [4, 128, 100, 75]               0
           Conv2d-30          [4, 128, 100, 75]         147,456
      BatchNorm2d-31          [4, 128, 100, 75]             256
             ReLU-32          [4, 128, 100, 75]               0
       DoubleConv-33          [4, 128, 100, 75]               0
             Down-34          [4, 128, 100, 75]               0
        MaxPool2d-35           [4, 128, 50, 37]               0
           Conv2d-36           [4, 256, 50, 37]         294,912
      BatchNorm2d-37           [4, 256, 50, 37]             512
             ReLU-38           [4, 256, 50, 37]               0
           Conv2d-39           [4, 256, 50, 37]         589,824
      BatchNorm2d-40           [4, 256, 50, 37]             512
             ReLU-41           [4, 256, 50, 37]               0
       DoubleConv-42           [4, 256, 50, 37]               0
             Down-43           [4, 256, 50, 37]               0
  ConvTranspose2d-44          [4, 128, 100, 74]         131,200
           Conv2d-45          [4, 128, 100, 75]         294,912
      BatchNorm2d-46          [4, 128, 100, 75]             256
             ReLU-47          [4, 128, 100, 75]               0
           Conv2d-48          [4, 128, 100, 75]         147,456
      BatchNorm2d-49          [4, 128, 100, 75]             256
             ReLU-50          [4, 128, 100, 75]               0
       DoubleConv-51          [4, 128, 100, 75]               0
               Up-52          [4, 128, 100, 75]               0
  ConvTranspose2d-53          [4, 64, 200, 150]          32,832
           Conv2d-54          [4, 64, 200, 150]          73,728
      BatchNorm2d-55          [4, 64, 200, 150]             128
             ReLU-56          [4, 64, 200, 150]               0
           Conv2d-57          [4, 64, 200, 150]          36,864
      BatchNorm2d-58          [4, 64, 200, 150]             128
             ReLU-59          [4, 64, 200, 150]               0
       DoubleConv-60          [4, 64, 200, 150]               0
               Up-61          [4, 64, 200, 150]               0
  ConvTranspose2d-62          [4, 32, 400, 300]           8,224
           Conv2d-63          [4, 32, 400, 300]          18,432
      BatchNorm2d-64          [4, 32, 400, 300]              64
             ReLU-65          [4, 32, 400, 300]               0
           Conv2d-66          [4, 32, 400, 300]           9,216
      BatchNorm2d-67          [4, 32, 400, 300]              64
             ReLU-68          [4, 32, 400, 300]               0
       DoubleConv-69          [4, 32, 400, 300]               0
               Up-70          [4, 32, 400, 300]               0
  ConvTranspose2d-71          [4, 16, 800, 600]           2,064
           Conv2d-72          [4, 16, 800, 600]           4,608
      BatchNorm2d-73          [4, 16, 800, 600]              32
             ReLU-74          [4, 16, 800, 600]               0
           Conv2d-75          [4, 16, 800, 600]           2,304
      BatchNorm2d-76          [4, 16, 800, 600]              32
             ReLU-77          [4, 16, 800, 600]               0
       DoubleConv-78          [4, 16, 800, 600]               0
               Up-79          [4, 16, 800, 600]               0
           Conv2d-80           [4, 3, 800, 600]              51
          OutConv-81           [4, 3, 800, 600]               0
================================================================
Total params: 1,942,611
Trainable params: 1,942,611
Non-trainable params: 0
----------------------------------------------------------------
Input size (MB): 21.97
Forward/backward pass size (MB): 7549.22
Params size (MB): 7.41
Estimated Total Size (MB): 7578.60
----------------------------------------------------------------
None
..\datasets\0719_labeled\JPEGImages ..\datasets\0719_labeled\Masks
INFO: Creating dataset with 233 examples
Epoch 1/2:   0%|                                                                              | 0/210 [00:00<?, ?img/s]D:\ANACONDA\envs\torch_1_12\lib\site-packages\torch\amp\autocast_mode.py:198: UserWarning: User provided device_type of 'cuda', but CUDA is not available. Disabling
  warnings.warn('User provided device_type of \'cuda\', but CUDA is not available. Disabling')
Epoch 1/2: 100%|████████████████████████████████████████████████| 210/210 [04:52<00:00,  1.39s/img, loss (batch)=0.228]
INFO: Checkpoint 1 saved!
Epoch 2/2: 100%|████████████████████████████████████████████████| 210/210 [03:28<00:00,  1.01img/s, loss (batch)=0.154]
INFO: Checkpoint 2 saved!
```

文件参数更改：

```python
# train.py 主程序中
# 原图片目录
dir_img = Path('...')
# 标记/处理后图片目录
dir_mask = Path('...')
# 存储目录
dir_checkpoint = Path('...')
```

> 路径表示：
>
> ./ → 当前工作文件夹下
>
> ../ → 后退一级文件夹下

## 模型预测

① 安装包 six matplotlib

```
conda activate torch_1_12
pip install six matplotlib
```

② 更新 module：torchvision

```
(torch_1_12) D:\SDUer\2022summer_driverless\1_segmentation\unet_torch>python predict_custom.py -i .\test\photo_19700101_000737.jpg -n -v -m ./checkpoints/0720/checkpoint_epoch1.pth -s 0.5
modeling time: 0.12813520431518555
Traceback (most recent call last):
  File "predict_custom.py", line 111, in <module>
    device=device)
  File "predict_custom.py", line 38, in predict_img
    transforms.Resize((full_img.size[1], full_img.size[0])),
AttributeError: module 'torchvision.transforms' has no attribute 'Resize'

// 报错：torchvision 的'torchvision.transforms'没有Resize属性
// 下载的torch-vision库版本与predict.py中调用的版本非对应，函数接口未对应
```

```
pip uninstall torchvision
pip install torchvision==0.13.0
```

③ 预测

参数含义见readme（以下使用训练第一轮迭代所得模型、预测test图片photo_19700101_000737.jpg的分割结果）

```
(torch_1_12) D:\SDUer\2022summer_driverless\1_segmentation\unet_torch>python predict_custom.py -i .\test\photo_19700101_000737.jpg -n -v -m ./checkpoints/0720/checkpoint_epoch1.pth -s 0.5
modeling time: 0.03682994842529297
infering time: 0.24144864082336426
```

# 路线规划

## 建图

原图像 → 去畸变 → 鸟瞰图

> **相机标定：**求解相机内参数，通过一种理论数学模型和优化的手段来近似实际的物理成像关系
>
> 在图像测量过程以及机器视觉应用中，为确定空间物体表面某点的**三维几何位置与其在图像中对应点之间的相互关系**，必须建立**相机成像的几何模型**，这些几何模型参数就是**相机参数**。在大多数条件下这些参数必须通过实验与计算才能得到，这个求解参数的过程就称之为相机标定。
>
> 相机成像原理（理想状态下）：
>
> <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220823181752057.png" alt="image-20220823181752057" style="zoom:67%;" />
>
> <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220823181812873.png" alt="image-20220823181812873" style="zoom:67%;" />
>
> > 相机标定处理阶段：
> >
> > > 世界坐标系、相机坐标系、图像坐标系、像素坐标系之间的误差
> >
> > 造成偏差的因素
> >
> > **相机内参（相机坐标系-图像坐标系）**
> >
> > 1 主点偏移
> >
> > 实际情况中，图像坐标系往往在图片的左上角，光轴过图像中心，因此图像坐标系和相机坐标系不重合（存在一个平移运动）
> >
> > 2 图像传感器特性
> >
> > 图像传感器像原尺寸在制造过程可能不是正方形，同时可能存在歪斜，因此需要考虑这些影响因素，传感器歪斜和不是正方形主要对相机x和y方向的焦距产生影响
> >
> > 3 镜头畸变
> >
> > 透过镜头边缘的光线很容易产生径向畸变，光线离镜头中心越远，畸变越大
> >
> > > 在不考虑畸变的情况下，考虑主点偏移、图像传感器的特性，相机坐标系下的点P与图像坐标的关系可以表达为：
> > >
> > > <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220823184126269.png" alt="image-20220823184126269" style="zoom:60%;" />
> > >
> > > 这就是相机内部参数对成像的影响，因此K称为内参矩阵。
> > >
> > > **相机内参标定**主要是标定相机的焦距、主点、歪斜等内部参数
> >
> > **相机外参（世界坐标系-相机坐标系）**
> >
> > 需要知道**成像平面内的物体在机器人或者车辆坐标系下的位置**时，需要进行一个坐标转换，它只与相机在世界坐标系内的安装位置和角度有关。从纯数学的角度来说，刚体运动和坐标变换总是可以分解为一个旋转运动和一个平移运动。
> >
> > 世界坐标系下的点P与图像坐标的关系可以表达为：
> >
> > <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220823184946424.png" alt="image-20220823184946424" style="zoom:60%;" />
>
> > https://zhuanlan.zhihu.com/p/87334006

> **图像变换：**
>
> 图像的几何变换主要分为：刚性变换、相似变换、仿射变换和透视变换（也称为投影变换）。
>
> 刚性变换：平移、旋转；
>
> 相似变换：缩放、剪切；
>
> 仿射变换：从一个二维坐标系变换到另一个二维坐标系的过程，属于线性变换。通过已知3对坐标点便可求取变换矩阵。
>
> 透视变换：从二维坐标系变换到三维坐标系，在从三维坐标系投影到二维平面，属于非线性变换。通过已知4对坐标点便可求取变换矩阵。
>
> <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220823185726268.png" alt="image-20220823185726268" style="zoom:50%;" />
>
> https://zhuanlan.zhihu.com/p/467543573

> 实验：
>
> **matlab工具箱实验**
>
> **python程序实现（3_birdview）：**
>
> 1 intrinsic calibrate：通过棋盘物理信息和标定得到畸变参数
>
> 2 distort correct：去畸变
>
> 3 perspective：透视变换（鸟瞰图准备）
>
> 4 bird view：生成鸟瞰图

## 最优路线算法

> 算法：
>
> A*算法（出发点到当前位置距离+当前位置到终点距离）
>
> >对比：广度优先算法、贪婪算法、Dijstra算法
> >
> >https://www.redblobgames.com/pathfinding/a-star/introduction.html
> >
> >https://www.bilibili.com/video/BV1bv411y79P?spm_id_from=333.337.search-card.all.click&vd_source=4172983847c80a53fb2e62caed743dc8

> **python程序实现（4 Astar）**

## 路线寻踪

> 算法：pure pursuit
>
> 小车结构几何模型：阿克曼转向几何
>
> <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220823194821101.png" alt="image-20220823194821101" style="zoom:50%;" />
>
> <img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220823194851611.png" alt="image-20220823194851611" style="zoom:50%;" />
>
> > https://zhuanlan.zhihu.com/p/95744069

> python程序实现（5 stanley）

# 综合实验

## 测试程序

> python程序：6 multi_steps

### 1/2.pure pursuit with imu (final)

**测速针：**

> 实质：计数器
>
> 接线：VCC GND G02（码盘A）
>
> 传感器发出/接收光线，电机上安装光栅（码盘）（遮挡光线），电机转速直接影响光栅转速，进而影响传感器接收到光线的频率，依此推算出速度

测试所需程序：
主板：开启热点、启用WebServer

PC端：运行py文件，发送get请求到主板Server，读取测速针数据

**imu：**

> 用于测角速度/加速度（加速度间接计算速度两次积分较复杂，实际的速度计算改用测速针）
>
> 接线：VCC GND SCL SDA INIT(接G05)

### 3.mask to path

> 从灰度分割图像到规划路线
>
> > 程序顺序：
> >
> > ① 处理灰度图片（放缩图片大小 → 矫正畸变 → 生成鸟瞰图）
> >
> > > 道路分割鸟瞰图 → cv2.resize()放缩 → 降低像素加快寻径速度
> > >
> > > 像素放缩：800×600 → 40×30
> > >
> > > > 小车在行进时无需精确到毫米级，两组像素参数下路径规划结果基本一致，没有必要用原图片像素规划，而降低像素有利于加快寻径速度
> >
> > ② 获取可行使区域图
> >
> > ③ 选取终点（在可行使范围内，距离小车80cm，随机选取）
> >
> > ④ 路径规划

### 4/5 photo to path

