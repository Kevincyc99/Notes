# GAMES101 Lecture Notes 03  
## Lecture 03  Transformation

### 二维变换
#### 缩放变换(Scale Transform)  
![Scale_Transform](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/Scale_Transform.png)  
```math
\begin{aligned}
\left[
\begin{matrix}
x^{\prime} \\ 
y^{\prime}
\end{matrix}
\right]
 = 
\left[
\begin{matrix}
s_x & 0 \\ 
0 & s_y
\end{matrix}
\right]
\left[
\begin{matrix}
x \\ 
y
\end{matrix}
\right]
\end{aligned}
```
#### 反射变换(Reflection Transform)  
水平反射(Horizontal reflection)  
![Horizontal_Reflection](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/Horizontal_Reflection.png)  
```math
\begin{aligned}
\left[
\begin{matrix}
x^{\prime} \\ 
y^{\prime}
\end{matrix}
\right]
 = 
\left[
\begin{matrix}
-1 & 0 \\ 
0 & 1
\end{matrix}
\right]
\left[
\begin{matrix}
x \\ 
y
\end{matrix}
\right]
\end{aligned}
```  
#### 切变变换(Shear Transform)  
![Shear_Transform](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/Shear_Transform.png)  
沿水平方向切变(把y坐标的a倍加到x坐标,y坐标不变)
```math
\begin{aligned}
\left[
\begin{matrix}
x^{\prime} \\ 
y^{\prime}
\end{matrix}
\right]
 = 
\left[
\begin{matrix}
1 & a \\ 
0 & 1
\end{matrix}
\right]
\left[
\begin{matrix}
x \\ 
y
\end{matrix}
\right]
\end{aligned}
```  
#### 旋转变换(Rotate Transform)  
![Rotate_Transform](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/Rotate_Transform.png)  
旋转矩阵的两列可以理解为新的基向量。  
```math
\begin{aligned}
R_{\theta}
 = 
\left[
\begin{matrix}
cos\theta & -sin\theta \\ 
sin\theta & cos\theta
\end{matrix}
\right]
\end{aligned}
```  
#### 线性变换(Linear Transform)  
任何可以由矩阵乘法实现的变换都是线性变换。  
线性变换有可加性和齐次性:
```math
\begin{aligned}
M(\vec{v_1}+\vec{v_2}) &= M\vec{v_1} + M\vec{v_2}\\ 
M(k\vec{v}) &= kM\vec{v}
\end{aligned}
```   
#### 齐次坐标(Homogenous Coordinates)  
由于平移变换不是线性变换,无法用一个矩阵乘法表示。因此,引入齐次坐标来统一成仿射变换。  
2D Point  : $(x, y, 1)^T$  
2D Vector : $(x, y, 0)^T$  
#### 仿射变换(Affine Transform)
仿射变换 = 线性变换 + 平移变换  
```math
\begin{aligned}
\left[
\begin{matrix}
x^{\prime} \\ 
y^{\prime} \\ 
1
\end{matrix}
\right]
 = 
\left[
\begin{matrix}
a & b & t_x \\ 
c & d & t_y \\
0 & 0 & 1
\end{matrix}
\right]
\left[
\begin{matrix}
x \\ 
y \\
1
\end{matrix}
\right]
\end{aligned}
```  
统一成仿射变换之后,变换就可以任意分解或合成。  
变换的顺序很重要。(一般是先线性变换再平移)
### 三维变换  
同二维,齐次坐标多一个维度。  
3D Point  : $(x, y, z, 1)^T$  
3D Vector : $(x, y, z, 0)^T$  

### 相关学习
1. **虎书**相关章节: **第六章**  
2. 推荐观看3Blue1Brown的**线性代数的本质** 第3、4集,从基向量变换的角度解释了变换(向量与矩阵相乘)。  
[【【官方双语/合集】线性代数的本质 - 系列合集】 ](https://www.bilibili.com/video/BV1ys411472E/)  
1. **3D数学基础(3D Math Primer)** [第五章](https://gamemath.com/book/matrixtransforms.html)、[第六章](https://gamemath.com/book/matrixmore.html)  
**(注意,本书是左手系,与GAMES101不同,所以部分变换有一些不同)**  
3D数学基础第4章4.2,和3B1B类似,从基变换的角度解释变换矩阵的作用。
[4.2 Geometric Interpretation of Matrix](https://gamemath.com/book/matrixintro.html#geometric_definition)  
