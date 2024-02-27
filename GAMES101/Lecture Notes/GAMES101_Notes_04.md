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

### 相关学习  
1.  **虎书**相关章节: **第六章、第七章**
2.  **3D数学基础(3D Math Primer)**
    三维旋转: [第五章](https://gamemath.com/book/matrixtransforms.html#rotation_3d_cardinal_axis)(5.1.2、5.1.3)、[第八章](https://gamemath.com/book/orient.html)  
    罗德里格斯旋转公式的推导在5.1.3,个人感觉讲的很清晰。  
    四元数的介绍在8.5  

