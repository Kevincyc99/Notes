# 3D Math Primer for Graphics and Game Development Notes 05
书籍网址:[3D Math Primer for Graphics and Game Development](https://gamemath.com/book/)  
阅读时为第二版。 
## Chapter 5 矩阵和线性变换  
### 5.1 旋转  
#### 5.1.1 2D旋转  
2D旋转是绕一点旋转;  
绕原点逆时针旋转 $\theta$ :  
```math
R(\theta)=
\left[
\begin{matrix}
cos\theta & sin\theta \\
-sin\theta & cos\theta
\end{matrix}
\right]
```
#### 5.1.2 3D绕坐标轴旋转  
3D旋转是以一条直线为轴旋转。  
绕x轴逆时针旋转 $\theta$ :  
```math
R_x(\theta) = 
\left[
\begin{matrix}
1 & 0 & 0 \\
0 & cos\theta & sin\theta \\
0 & -sin\theta & cos\theta
\end{matrix}
\right]
```  
绕y轴逆时针旋转 $\theta$ :  
```math
R_y(\theta) = 
\left[
\begin{matrix}
cos\theta & 0 & -sin\theta \\
0 & 1 & 0 \\
sin\theta & 0 & cos\theta
\end{matrix}
\right]
```  
绕z轴逆时针旋转 $\theta$ :  
```math
R_z(\theta) = 
\left[
\begin{matrix}
cos\theta & sin\theta & 0 \\
-sin\theta & cos\theta & 0 \\
0 & 0 & 1
\end{matrix}
\right]
```  
#### 5.1.3 3D绕任意轴旋转  
定义旋转轴为 $\hat{n}$ ,旋转角度为 $\theta$ 。  
```math
\vec{v^{\prime}} = \vec{v}R(\hat{n},\theta)
```  
和向量点乘一样, 可以把向量拆成与旋转轴平行和垂直分量。  
```math
\vec{v} = \vec{v_{||}} + \vec{v_{\perp}};
```  
平行于旋转轴的分量不受影响,故只需要求 $\vec{v_{\perp}^{\prime}}$ 。  
由于 $\vec{v_{||}}$ 是 $\vec{v}$ 平行于 $\hat{n}$ 的分量,也可看作 是 $\vec{v}$ 在 $\hat{n}$ 方向的投影,即
 
```math
\begin{aligned}
\vec{v_{||}} &= (\vec{v}\cdot\hat{n}) \hat{n} \\
\vec{v_{\perp}} &= \vec{v} - \vec{v_{||}} 
\end{aligned}
```  
可以得到与 $\vec{v_{||}}$ 和 $\vec{v_{\perp}}$ 都垂直, 且模长与 $|\vec{v_{\perp}}|$ 相同的向量 $\vec{w}$ 。  
```math
\begin{aligned}
\vec{w} &= \hat{n} \times \vec{v_{\perp}} \\ 
&= \hat{n} \times (\vec{v} - \vec{v_{||}}) \\ 
&= \hat{n} \times \vec{v} - \hat{n} \times \vec{v_{||}} \\ 
&= \hat{n} \times \vec{v} - 0 \\ 
&= \hat{n} \times \vec{v}
\end{aligned}
```  
![Rotate_about_an_arbitrary_axis](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/Rotate_about_an_arbitrary_axis.png)  
(图片来自原书Figure5.5)  
如图所示, 对于 $\vec{v_{\perp}}$ ,绕 $\hat{n}$ 旋转 $\theta$ 就是在 $\vec{v_{\perp}}$ 和 $\vec{w}$ 形成的平面内旋转 $\theta$ 。  
因此  
```math
\begin{aligned}
\vec{v_{\perp}^{\prime}} &= cos\theta \vec{v_{\perp}} + sin\theta \vec{w} \\ 
&= cos\theta (\vec{v} - \vec{v_{||}}) + sin\theta (\hat{n} \times \vec{v}) \\ 
&= cos\theta (\vec{v} - (\vec{v}\cdot\hat{n}) \hat{n}) + sin\theta (\hat{n} \times \vec{v}) \end{aligned}
```  
因此,
```math
\begin{aligned} 
\vec{v^{\prime}} &= \vec{v_{||}} + \vec{v_{\perp}^{\prime}} \\ 
&= (\vec{v}\cdot\hat{n}) \hat{n} + cos\theta (\vec{v} - (\vec{v}\cdot\hat{n}) \hat{n}) + sin\theta (\hat{n} \times \vec{v}) \\ 
&= (1 - cos\theta) (\vec{v}\cdot\hat{n}) \hat{n} + cos\theta \vec{v} + sin\theta (\hat{n} \times \vec{v})
\end{aligned}
```  
根据Exercise2.24, 可以将方程写成矩阵形式。先将向量沿坐标轴分解:   
```math
\begin{aligned}
\vec{p} &= [1 \quad 0 \quad 0], \quad
\vec{p^{\prime}} = 
\left[
\begin{matrix}
n_x(1-cos\theta)^2 + cos\theta \\
n_x n_y(1-cos\theta) + n_z sin\theta \\ 
n_x n_z(1-cos\theta) - n_y sin\theta
\end{matrix}
\right]^T \\ 
\vec{q} &= [0 \quad 1 \quad 0], \quad
\vec{q^{\prime}} = 
\left[
\begin{matrix}
n_x n_y(1-cos\theta) - n_z sin\theta \\
n_y(1-cos\theta)^2 + cos\theta \\ 
n_y n_z(1-cos\theta) + n_x sin\theta
\end{matrix}
\right]^T \\ 
\vec{r} &= [0 \quad 0 \quad 1], \quad
\vec{r^{\prime}} = 
\left[
\begin{matrix}
n_x n_z(1-cos\theta) + n_y sin\theta \\
n_y n_z(1-cos\theta) - n_x sin\theta \\ 
n_z(1-cos\theta)^2 + cos\theta
\end{matrix}
\right]^T
\end{aligned}
```  
将分量合起来,得到最终的旋转矩阵:  
```math
\begin{aligned}
R(\hat{n}, \theta) &= 
\left[
\begin{matrix}
-p^{\prime}- \\ 
-q^{\prime}- \\ 
-r^{\prime}-
\end{matrix}
\right] \\ 
&= 
\left[
\begin{matrix}
n_x(1-cos\theta)^2 + cos\theta &
n_x n_y(1-cos\theta) + n_z sin\theta & 
n_x n_z(1-cos\theta) - n_y sin\theta \\
n_x n_y(1-cos\theta) - n_z sin\theta &
n_y(1-cos\theta)^2 + cos\theta & 
n_y n_z(1-cos\theta) + n_x sin\theta \\
n_x n_z(1-cos\theta) + n_y sin\theta &
n_y n_z(1-cos\theta) - n_x sin\theta & 
n_z(1-cos\theta)^2 + cos\theta
\end{matrix}
\right]
\end{aligned}
```  
这个旋转矩阵又称罗德里格斯公式。  
### 5.2 缩放
缩放矩阵是对角矩阵。
#### 5.2.1 沿坐标轴缩放
```math
\begin{aligned}
S(k_x,k_y,k_z) = 
\left[
\begin{matrix}
k_x & 0 & 0 \\ 
0 & k_y & 0 \\ 
0 & 0 & k_z
\end{matrix}
\right]
\end{aligned}
```
#### 5.2.2 沿任意轴缩放  
定义缩放轴为 $\hat{n}$ ,缩放系数为k。  
与绕任意轴旋转的处理类似,可以把 $\vec{v}$ 分解成与 $\hat{n}$ 同向以及垂直两个分量, 分别记为 $\vec{v_{||}},\vec{v_{\perp}}$ 。不难看出, 垂直分量沿缩放轴不变,故只有平行分量进行缩放。
```math
\begin{aligned}
\vec{v_{\perp}} &= \vec{v} - \vec{v_{||}} \\ 
\vec{v_{||}} &= (\vec{v}\cdot\hat{n})\hat{n}\\ 
\vec{v_{||}^{\prime}} &= k(\vec{v}\cdot\hat{n})\hat{n}\\ 
\vec{v^{\prime}} &= \vec{v_{||}^{\prime}} + \vec{v_{\perp}} \\ 
&= k(\vec{v}\cdot\hat{n})\hat{n} + \vec{v} - (\vec{v}\cdot\hat{n})\hat{n} \\ 
&= \vec{v} + (k-1)(\vec{v}\cdot\hat{n})\hat{n} 
\end{aligned}
```  
根据Exercise2.23,把方程写成矩阵形式,先将向量沿坐标轴分解: 
```math
\begin{aligned}
\vec{p} &= [1 \quad 0 \quad 0], \quad
\vec{p^{\prime}} = 
\left[
\begin{matrix}
(k-1)n_x^2 + 1 \\
(k-1)n_xn_y \\ 
(k-1)n_xn_z
\end{matrix}
\right]^T \\ 
\vec{q} &= [0 \quad 1 \quad 0], \quad
\vec{q^{\prime}} = 
\left[
\begin{matrix}
(k-1)n_xn_y \\
(k-1)n_y^2 + 1 \\ 
(k-1)n_yn_z 
\end{matrix}
\right]^T \\ 
\vec{r} &= [0 \quad 0 \quad 1], \quad
\vec{r^{\prime}} = 
\left[
\begin{matrix}
(k-1)n_xn_z \\
(k-1)n_yn_z \\ 
(k-1)n_z^2 + 1 
\end{matrix}
\right]^T
\end{aligned}
```  
将分量合起来,得到最终的缩放矩阵:  
```math
\begin{aligned}
S(\hat{n}, k) &= 
\left[
\begin{matrix}
-p^{\prime}- \\ 
-q^{\prime}- \\ 
-r^{\prime}-
\end{matrix}
\right] \\ 
&= 
\left[
\begin{matrix}
(k-1)n_x^2 + 1 &
(k-1)n_xn_y & 
(k-1)n_xn_z \\
(k-1)n_xn_y &
(k-1)n_y^2 + 1 & 
(k-1)n_yn_z \\
(k-1)n_xn_z &
(k-1)n_yn_z & 
(k-1)n_z^2 + 1 
\end{matrix}
\right]
\end{aligned}
```  
### 5.3 正交投影  
投影可以理解为降维操作。  
正交投影是一种特殊的缩放, 即把投影平面的法向量的方向轴的缩放系数k设置为0。
#### 5.3.1 沿主平面投影  
```math
\begin{aligned}
P_{xy} &= 
\left[
\begin{matrix}
1 & 0 & 0 \\ 
0 & 1 & 0 \\ 
0 & 0 & 0
\end{matrix}
\right] \\ 
P_{xz} &= 
\left[
\begin{matrix}
1 & 0 & 0 \\ 
0 & 0 & 0 \\ 
0 & 0 & 1
\end{matrix}
\right] \\ 
P_{yz} &= 
\left[
\begin{matrix}
0 & 0 & 0 \\ 
0 & 1 & 0 \\ 
0 & 0 & 1
\end{matrix}
\right]
\end{aligned}
```  
#### 5.3.2 沿任意平面投影
5.2.2得到的缩放矩阵,令k=0,得到:  
```math
\begin{aligned}
P(\hat{n}) &= 
\left[
\begin{matrix}
-n_x^2 + 1 &
-n_xn_y & 
-n_xn_z \\
-n_xn_y &
-n_y^2 + 1 & 
-n_yn_z \\
-n_xn_z &
-n_yn_z & 
-n_z^2 + 1 
\end{matrix}
\right]
\end{aligned}
```  
### 5.4 反射  
反射又叫镜像;也是一种特殊的缩放,即k=-1。  
```math
\begin{aligned}
R(\hat{n}) &= 
\left[
\begin{matrix}
-2n_x^2 + 1 &
-2n_xn_y & 
-2n_xn_z \\
-2n_xn_y &
-2n_y^2 + 1 & 
-2n_yn_z \\
-2n_xn_z &
-2n_yn_z & 
-2n_z^2 + 1 
\end{matrix}
\right]
\end{aligned}
```  
### 5.5 切变 (shear,也有翻译为错切)  
切变可以理解为一种不均匀的拉伸,其角度不保留,但面积(体积)不变。其基本思想是把一个坐标的倍数加到其他坐标上。 
```math 
\begin{aligned}
x^{\prime} &= x + sy \\ 
H_x(s) &= 
\left[
\begin{matrix}
1 & 0 \\
s & 1
\end{matrix}
\right] \\ 
y^{\prime} &= y + sx \\ 
H_y(s) &= 
\left[
\begin{matrix}
1 & s \\
0 & 1
\end{matrix}
\right] \\
H_{xy}(s,t) &= 
\left[
\begin{matrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
s & t & 1
\end{matrix}
\right] \\
H_{xz}(s,t) &= 
\left[
\begin{matrix}
1 & 0 & 0 \\
s & 1 & t \\
0 & 0 & 1
\end{matrix}
\right] \\
H_{yz}(s,t) &= 
\left[
\begin{matrix}
1 & s & t \\
0 & 1 & 0 \\
0 & 0 & 1
\end{matrix}
\right] 
\end{aligned}
```  
作者说切变很少被使用。  
注:切变其实可以理解为旋转和缩放的组合;虎书上说在一些情况下,切变要更有效,例如对一个模型的大量三角形进行缩放和旋转,由于计算精度的问题,原本稠密的三角形排列,经过变换可能会出现空隙,而切变则不会产生这种空隙。  
### 5.6 组合变换    
矩阵相乘可以通过结合律改变顺序,多个矩阵可以合并成一个复杂矩阵,也可以把一个矩阵分解成多个简单矩阵。  
### 5.7 变换的分类  
#### 5.7.1 线性变换  
线性函数满足:
```math
F(a+b) = F(a) + F(b)\\ 
F(ka) = kF(a)
```
> 1.    任何可以用矩阵乘法完成的变换都是线性变换。
> 2.    线性变换不包含平移。

#### 5.7.2 仿射变换  
仿射变换是线性变换加上平移变换。因此,仿射变换是线性变换的超集。  
#### 5.7.3 可逆变换  
除了投影以外的初等变换均是可逆变换。后续在6.2讨论。  
#### 5.7.4 保角变换  
旋转和均匀缩放是唯二能保持角度不变的变换。  
保角变换均是仿射且可逆的。
#### 5.7.5 正交变换  
正交变换由正交基得到,其逆变换很容易得到。  
平移、旋转、反射是仅有的正交变换。  
正交变换均是仿射且可逆的。正交变换矩阵的行列式为$\pm$1。  
#### 5.7.6 刚体变换  
刚体变换是改变一个对象的位置与方向,但不改变其形状的变换。平移和旋转是唯二的刚体变换。(反射不被认为是刚体变换)  
刚体变换是仿射的,可逆的,保角的,正交的。刚体变换矩阵的行列式为1。  
#### 5.7.7 变换种类的总结  
|变换|线性|仿射|可逆|保角|正交|刚体|保长度|保面积,体积|行列式|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|线性变换|Y|Y|
|仿射变换||Y||||||| $\neq$ 0|
|可逆变换|||Y|
|保角变换||Y|Y|Y|
|正交变换||Y|Y||Y|||| $\pm$ 1|
|刚体变换||Y|Y|Y|Y|Y|Y|Y|1|
|平移变换||Y|Y|Y|Y|Y|Y|Y|1|
|旋转变换|Y|Y|Y|Y|Y|Y|Y|Y|1|
|均匀缩放变换|Y|Y|Y|Y||||| $k^n$ |
|非均匀缩放变换|Y|Y|Y|
|正交投影变换|Y|Y|||||||0|
|反射变换|Y|Y|Y||Y||Y|Y|-1|
|切变变换|Y|Y|Y|||||Y|1|  

### 5.8 练习  
都是直接计算,代入公式即可。真正做项目也不会手算的。  

#### 英语学习  
synonymous 同义的  
