# Camera

## 相机类型

- fisheye camera: 鱼眼相机

  受到水下斯涅尔窗口的现象的启发，采用不同的投影方式，来得到极大的视场角

  投影方式

  - 等距投影
  - 等积投影
  - 体视投影
  - 正交投影

- omnidirectional camera: 全向相机，360度相机

- perspective camera or pinhole camera: 

  透视相机, 小孔成像

  普通镜头和针孔相机在数学模型上可以等价对等，都是透视变换，Perspective transformation

  



## 相机数学模型，pinhole camera model

- 从**世界坐标系**到**相机坐标系**

- 从**相机坐标系**到**成像平面坐标系**

  f就是相机的物理焦距，单位是米

  手机、数码相机等拍照设备内部软件在处理时做了自动翻转

  ![img](https://pic2.zhimg.com/80/v2-6ffb5c73df0ed05732d4e3cff7b79ba5_720w.jpg)

  ![img](https://pic3.zhimg.com/80/v2-aae4d90f66073126df093222d0a32992_720w.jpg)

  

- 从**成像平面坐标系**到**像素坐标系**

  数字相机一般使用CMOS或者CCD作为传感器将光信号转为电信号，并记录下来生成数字图像。与传统胶片不同，这类传感器是由一个个感光元件组成的，工作时候每个感光缘检独立记录自己所接受的光信号，导致生成数字图像是离散的

  - CCD（电荷耦合器件 charge coupled device）相机

    像素（Pixel），CCD上植入的微小光敏物质称为像素

    一般为4：3或3：2

    2400万像素：4016*6016 像素

    一般来说，一个pixel需要发给RGB，如果是彩色的话，所以300w像素相当于只有150w像素，150G，75R，75B

    

  

  - CMOS相机

    互补性氧化金属半导体CMOS(Complementary Metal-Oxide Semiconductor)





手写部分：

<img src="..\picture\camera_1.png" alt="camera_1" style="zoom:33%;" />

<img src="..\picture\camera_2.png" alt="camera_2" style="zoom:33%;" />



成像过程是一个perspective transformation contains

- the projection from 3d world coordinates to 3d camera coordinates，which is a mapping from R^3^ to R^3^
- the projection from 3d camera coordinates to 2d image coordinates, which by contrast is a mapping from R^3^ to R^2^







## 畸变系数 图像去畸变





## 视觉测距

- 单目测距（单目距离估计）

  相机模型的本质是：**世界坐标系3d**->**像素坐标系2d**的投影变换，在投影变换的过程中**失去了深度z信息**

  

  假设相机Zc轴和水平地面平行，到地面高度为h，且被测物体在地面上（约束条件）。在相机下方h1距离有个标定板：在测距之前首先拍摄一张标定板图像，当测距时可以根据地面上的物体在图像中的位置读取d1，可以估计出地面和相机真实距离为：

  d = h*d1/h1

  其中h和h1已经知道，d1可以从标定板的图像中读取出来。这是一种估计方法，而非精确计算

  ![img](https://pic2.zhimg.com/80/v2-b7b48a5d8ef4ab13786c51c98e7b9a41_720w.jpg)

- 双目测距

  <img src="https://pic3.zhimg.com/80/v2-8be654f738dbaf08dd878b27420f2d62_720w.jpg" alt="img" style="zoom:33%;" />

  注意：双目测距要求两个相机坐标系zc和zc'平行，否则变为对极几何的问题

  

  两个相机分别对远处距离zc处同一目标拍照得到左右两张不同的图像，由于视角不同，同一点在两张图像像素位置不同，即存在视差d = u‘ - u

  相机坐标系->像素坐标系uv的公式为：

  <img src="..\picture\双目视差测距.png" alt="双目视差测距" style="zoom:50%;" />

  视差可以通过图像来获取

  存在的问题：

  - 双目测距要求Zc和Zc'轴平行，测距精度严重依赖于Zc和Zc'的平行精度
  - 为了计算视差d1，需要匹配世界中同一点p1在左右两幅图像中的像素点p1和p1’双目立体匹配
  - 距离世界中越远的点p2，在左右视图中的视差d2越小，测距结果越容易收到双目立体匹配误差的影响。（**双目测距精度**与**被测物体距离**成反相关）

  ![img](https://pic1.zhimg.com/80/v2-bb8e4b82781ce10953f3988671df09dc_720w.jpg)







reference：

https://zhuanlan.zhihu.com/p/135943895

https://zhuanlan.zhihu.com/p/38397650

https://zhuanlan.zhihu.com/p/29273352