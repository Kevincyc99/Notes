# GAMES101 Lecture Notes 01  
对GAMES101学习回顾的笔记,以及对于虎书的学习。
#### 虎书(Fundamentals of Computer Graphics) 第四版  
![Tiger_Book_Cover](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/Tiger_Book_Cover.png)  
(虎书已经出了第5版,其中对于光线追踪相关章节以及PBR部分有较大调整,其余基本不变;但目前的PDF中很多页面仍然是图片,不太方便,故仍使用第四版进行学习)

个人学习计划:  
先学完Lecture1\~9,完整的光栅化流程,读完虎书对应章节(Chapter 1\~3,5\~8,10),完成HW1~3  
再学习Lecture13\~16,光线追踪相关内容,读虎书对应章节(Chapter 4,12,18),完成HW5\~7。  
其他部分之后有时间再学。  
## Lecture 01  Overview of Computer Graphics  
#### 图形学三大主题(本课分为四个部分介绍):  
1) 渲染(光栅化+光线追踪)  
2) 建模(几何)  
3) 仿真(动画模拟)  

实时(Real-time): 一般认为是30FPS  

关于CG和CV的关系:  ![CG_CV_Difference](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/CG_CV_Difference.png)  
1) 渲染: 从建模数据综合得到图像, 对材质等进行建模
2) 建模: 如何描述三维模型
3) 仿真: 通过三维模型模拟各种物理表现
4) 计算机视觉: 从图像分析得到不同的三维建模结构
5) 图像处理: 对图像进行分析,通过深度学习得到另一张图  

##### 虎书相关章节: 第一章

