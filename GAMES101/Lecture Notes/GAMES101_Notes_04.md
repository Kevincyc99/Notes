# GAMES101 Lecture Notes 04  
## Lecture 04  Transformation Cont.

### 三维旋转
#### 绕坐标轴(齐次坐标下)
```math
\begin{aligned}
R_x{(\alpha)} &= 
\left[
\begin{matrix}
1 & 0 & 0 & 0 \\
0 & cos\alpha & -sin\alpha & 0 \\ 
0 & sin\alpha & cos\alpha & 0 \\
0 & 0 & 0 & 1
\end{matrix}
\right]\\
R_y{(\alpha)} &= 
\left[
\begin{matrix}
cos\alpha & 0 & sin\alpha & 0 \\ 
0 & 1 & 0 & 0 \\
-sin\alpha & 0 & cos\alpha & 0 \\
0 & 0 & 0 & 1
\end{matrix}
\right]\\
R_z{(\alpha)} &= 
\left[
\begin{matrix}
cos\alpha & -sin\alpha & 0 & 0 \\ 
sin\alpha & cos\alpha & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{matrix}
\right]
\end{aligned}
```  
#### 绕任意轴   
罗德里格斯旋转公式:  
```math
\begin{aligned}
R(\vec{n}, \alpha) = cos(\alpha)\vec{I} + (1 - cos(\alpha))\vec{n}\vec{n^T} + sin(\alpha)
\left(
\begin{matrix}
0 & -n_z & n_y \\
n_z & 0 & -n_x \\
-n_y & n_x & 0
\end{matrix}
\right)
\end{aligned}
```  
这里的轴 $\vec{n}$ 其实并不是任意轴,必须要过原点。  
闫老师课后的推导很简洁,如果看不懂,可以看[这里](https://gamemath.com/book/matrixtransforms.html#rotation_3d_arbitrary_axis),逐步推导。
#### 四元数(Quaternion)  
为了解决旋转角度插值问题而引入;详细可以看[四元数](https://gamemath.com/book/orient.html#quaternions)。  

### Viewing Transformation (观测变换)  
1.  View /Camera Transformation (视图变换)
2.  Projection Transformation (投影变换)
    a. Orthographic Projection (正交变换)
    b. Perspective Projection (透视变换)
#### 视图变换
类比拍照的过程:
1.  设置场景(Model Transformation)  
2.  设置相机的位置和角度(View Transformation)
3.  拍摄(Projection Transformation)  

如何确定一个相机?
1.  位置 $\vec{e}$ 
2.  相机朝向 $\hat{g}$
3.  相机的向上方向 $\hat{t}$
![Set_Camera](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/Set_Camera.png)  

为了方便,把相机固定放在原点,朝向-z轴方向,向上方向为y轴。  
因此,把相机放在默认位置需要变换M~view~  
```math
\begin{aligned}
M_{view} &= R_{view}T_{view} \\
&= 
\left[
\begin{matrix}
x_{\hat{g}\times\hat{t}} & y_{\hat{g}\times\hat{t}} & z_{\hat{g}\times\hat{t}} & 0 \\
x_t & y_t & z_t & 0 \\
x_{-g} & y_{-g} & z_{-g} & 0\\
0 & 0 & 0 & 1
\end{matrix}
\right]
\left[
\begin{matrix}
1 & 0 & 0 & -x_e \\ 
0 & 1 & 0 & -y_e \\
0 & 0 & 1 & -z_e \\
0 & 0 & 0 & 1
\end{matrix}
\right]
\end{aligned}
```
#### 投影变换
##### 正交投影
1.  先将选定的长方体平移到原点(中心移到原点)
2.  再把长方体缩放到[-1,1]^3^的正则(canonical)立方体  

```math
\begin{aligned}
M_{ortho} &= 
\left[
\begin{matrix}
\frac{2}{r-l} & 0 & 0 & 0 \\
0 & \frac{2}{t-b} & 0 & 0 \\
0 & 0 & \frac{2}{n-f} & 0\\
0 & 0 & 0 & 1
\end{matrix}
\right]
\left[
\begin{matrix}
1 & 0 & 0 & -\frac{r+l}{2} \\ 
0 & 1 & 0 & -\frac{t+b}{2} \\
0 & 0 & 1 & -\frac{n+f}{2} \\
0 & 0 & 0 & 1
\end{matrix}
\right]
\end{aligned}
```  
##### 透视投影
1.  先将截锥体压缩成长方体
2.  再对1中得到的长方体做正交投影

在1的压缩过程中: 近平面不变, 远平面中心点不变, 远平面z方向不变  
```math
\begin{aligned}
M_{persp \rightarrow ortho} &= 
\left[
\begin{matrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
0 & 0 & n + f & -nf \\
0 & 0 & 1 & 0
\end{matrix}
\right]\\
M_{persp} &= M_{ortho}M_{persp \rightarrow ortho}
\end{aligned}
```

##### 思考题
> 在透视投影的过程中,中间平面的z值如何变化?

个人推导:  
假设近远平面之间某点P(x,y,z,1)^T^,z $\in$ (n,f) (此处n,f均取绝对值,故0 < n < f)  
```math
\begin{aligned}
P^{\prime} = M_{persp \rightarrow ortho} P &= 
\left[
\begin{matrix}
n & 0 & 0 & 0 \\
0 & n & 0 & 0 \\
0 & 0 & n + f & -nf \\
0 & 0 & 1 & 0
\end{matrix}
\right]
\left[ 
\begin{matrix}
x \\ y \\ z \\ 1
\end{matrix}
\right] \\
&= 
\left[ 
\begin{matrix}
nx \\ ny \\ (n+f)z - nf \\ z
\end{matrix}
\right]  = 
\left[ 
\begin{matrix}
nx/z \\ ny/z \\ n+f-nf/z \\ 1
\end{matrix}
\right]\\
\end{aligned}
```
```math
z^{\prime} = n+f-nf/z \\
设f(z) = n+f-nf/z, \\ 
解得 f^{\prime}(z) = nf / z^2 > 0, \quad f^{\prime \prime}(z) = -2nf / z^3 < 0 \\
```
关系大致如图  
![lecture4_exercise1](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/lecture4_exercise1.jpg)  
当z $\in$ (n,f)时,恒有 $z^{\prime} > z $ ,故z坐标在变换后|z|变大,向远平面靠拢。

### 相关学习  
1.  **虎书**相关章节: **第六章、第七章**
2.  **3D数学基础(3D Math Primer)**
    三维旋转: [第五章](https://gamemath.com/book/matrixtransforms.html#rotation_3d_cardinal_axis)(5.1.2、5.1.3)、[第八章](https://gamemath.com/book/orient.html)  
    罗德里格斯旋转公式的推导在5.1.3,个人感觉讲的很清晰。  
    四元数的介绍在8.5  

