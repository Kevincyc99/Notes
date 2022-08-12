# Fundamentals of Computer Graphics, Fourth Edition

## 03	Raster Images

pixel = picture element

raster images（光栅图像）

vector images（矢量图像）

### 3.1	Raster Devices（已读完）

Output：

​	Display：

​			Transmissive（透射）： liquid crystal display（LCD）	液晶显示器

​			Emissive（发射）： light-emitting diode（LED）display	发光二极管显示

​	Hardcopy：

​			Binary（二元的）： ink-jet printer	喷墨打印机

​			Continuous tone（连续色调）： dye sublimation printer	染料升华打印机

Input：

​	2D array sensor： digital camera	数码相机

​	1D array sensor： flatbed scanner	平板扫描仪

#### 3.1.1	Displays

LED的原理是自身发光，通过设置开关来控制一个pixel发光或关闭

LCD的原理是投射发光，即用两片滤波板来对光源进行滤波，投过去的即为显示在屏幕上的光

#### 3.1.2	Hardcopy Devices

打印机的分辨率取决于其像素密度（pixels per inch, ppi或dots per inch, dpi）而不取决于屏幕大小

dpi多用于表示二进制的显示设备，即只有黑白，而ppi表示连续色调的显示设备。

#### 3.1.3	Input Devices

### 3.2	Images，Pixels， and Geometry（已读完）

通常像素坐标系以（0，0）为原点，（n<sub>x</sub> - 1, n<sub>y</sub> - 1）为右上角

#### 3.2.1	Pixel Values

一些常见像素格式：

​	1-bit grayscale——不需要中间灰度的文本和黑白图像

​	8-bit RGB fixed-range color(24 bits total per pixel)——网页和电子邮件程序

​	12- to 14-bit fixed-range RGB (36–42 bits/pixel)——计算机显示

​	16-bit fixed-range RGB (48bits/pixel)——专业相机的原始图像

​	16-bit grayscale(16 bits/pixel)——放射学和医学成像

​	16-bit “half-precision”floating-point RGB——HDR图像，实时渲染的中间格式

​	32-bit floating-point RGB——用于软件渲染和HDR处理的通用中间格式

#### 3.2.2	Monitor Intensities and Gamma

$$
displayed\space intensity = (maximum\space intensity)\alpha^\gamma
$$

显示强度 = 最大强度x输入像素值的伽马值次幂