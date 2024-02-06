# 3D Math Primer for Graphics and Game Development Notes 01
书籍网址:[3D Math Primer for Graphics and Game Development](https://gamemath.com/book/)  
阅读时为第二版。
## Chapter 1 笛卡尔坐标系
### 1.1 一维数学  
主要介绍数(Count)和数系发展的历史。  
大部分情况，这个世界都是用离散数学来描述的，而连续数学更多的是在工程中作为工具使用。在计算机中选择shorts，int还是float，double主要考虑的是精度问题。以前的图形学书籍会推荐使用整数，因为整数运算比浮点数快，但现在矢量化的浮点计算在很多情况下甚至比整数计算还要快。  
##### The First Law of Computer Graphics
> If it looks right, it is right.  

由此，图形学程序的debug会和传统的程序debug有所不同(虎书里提到过)，很多时候(在代码没有报错，而是输出错误的情况下)是根据输出图像来进行假设，再用实践来验证假设，而不是用debugger去一行行检查代码逻辑中的错误。
### 1.2 二维坐标空间  
屏幕坐标空间，默认左上为原点，x轴向右为正方向，y轴向下为正方向。  
![Scrren Coordinate Space](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@1.0/Notes/3D_Math_Primer_1_01_Screen_coordinate_space.png)  
(图片来自原书Figure 1.6)

2D空间内，坐标系均可以通过旋转或翻转得到，从某种意义上来说，二维坐标系都是等价的。  
### 1.3 三维坐标空间
3D空间中左手系和右手系无法相互转换，在不同的领域会采用不同的手系，通常，这可以简单理解成z轴的正方向的符号问题。  
本书使用左手系作为默认坐标系。  
### 1.4 一些零碎的基础数学知识  
一些中学知识的回顾。  
### 1.5 练习  
都比较简单，就不记录了。
### 第一章总结  
语言很生动，适边看边学英语，但是作为学知识来说，信息密度有点低(例子太具体)。当然，可能只是刚开始确实比较简单。

#### 英语学习:  
be reminiscent of 让人想起……  
functionary 公务员  
vice versa 反之亦然  
hypotenuse  斜边  
obtuse 迟钝的，钝角的


