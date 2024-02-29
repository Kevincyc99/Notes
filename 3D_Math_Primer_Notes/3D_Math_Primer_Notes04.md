# 3D Math Primer for Graphics and Game Development Notes 04
书籍网址:[3D Math Primer for Graphics and Game Development](https://gamemath.com/book/)  
阅读时为第二版。  
## Chapter 4 矩阵简介  
### 4.1 矩阵的数学定义  
矩阵的下标是从1,1开始的,而编程中用来存储矩阵的数组,下标是从0开始的,因此为了防止混淆,包含用于几何目的的小型矩阵的类通常用一些变量来定义用到的元素,比如float m11,而不是用float elem[0][0]。  
出于图形中应用的考虑,一般讨论n阶方阵,特别是2x2、3x3、4x4方阵。  
一个矩阵可以左乘一个行向量(都是n阶),得到一个行向量;  
也可以右乘一个列向量(都是n阶),得到一个列向量。  
矩阵与向量相乘,可以看作是行(列)向量与矩阵每一列(行)的点乘。  
对于其他领域来说,向量与矩阵相乘,通常使用列向量(出于美观考虑),而对于计算机来说,代码的可读性要比方程的可读性更重要,因此一些图形书籍或API会使用行向量(DirectX),另一些使用列向量(OpenGL)。本书主要使用行向量(在一些不涉及矩阵的运算,可能会用列向量)。  
### 4.2 矩阵的几何解释  
变换就是一个向量 $\vec{v}$ 与一个变换矩阵M相乘。方阵可以表示任意的线性变换。  
常见的一些变换:旋转、缩放、正交投影、反射、切变  
理解变换的过程:  
1.  观察单位行向量左乘矩阵得到:
```math
\begin{aligned}
\vec{i} M &= [1 \quad 0 \quad 0]
\left[
\begin{matrix}
m_{11}&m_{12}&m_{13}\\
m_{21}&m_{22}&m_{23}\\
m_{31}&m_{32}&m_{33}
\end{matrix}
\right] = [m_{11} \quad m_{12} \quad m_{13}];\\

\vec{j} M &= [0 \quad 1 \quad 0]
\left[
\begin{matrix}
m_{11}&m_{12}&m_{13}\\
m_{21}&m_{22}&m_{23}\\
m_{31}&m_{32}&m_{33}
\end{matrix}
\right] = [m_{21} \quad m_{22} \quad m_{23}];\\

\vec{k} M &= [0 \quad 0 \quad 1]
\left[
\begin{matrix}
m_{11}&m_{12}&m_{13}\\
m_{21}&m_{22}&m_{23}\\
m_{31}&m_{32}&m_{33}
\end{matrix}
\right] = [m_{31} \quad m_{32} \quad m_{33}];
\end{aligned}
```
因此,矩阵M的第1行可以理解为对基向量 $\vec{v}$ 的 $\vec{i}$ 进行变换,第2行对 $\vec{j}$ 变换,第3行对 $\vec{k}$ 变换。  

2.  而所有的向量 $\vec{v}$ 都可以用基向量表示:  
```math
\vec{v} = v_x \vec{i} + v_y \vec{j} + v_z \vec{k}
```  
3.  因此,行向量左乘矩阵可以表示为:
```math
\begin{aligned}
\vec{v} M &= (v_x \vec{i} + v_y \vec{j} + v_z \vec{k})M \\
&= v_x(\vec{i}M) + v_y(\vec{j}M) + v_z(\vec{k}M)\\
&= v_x[m_{11} \quad m_{12} \quad m_{13}] + v_y[m_{21} \quad m_{22} \quad m_{23}]
+ v_z[m_{31} \quad m_{32} \quad m_{33}]
\end{aligned}
```
4.  因此行向量左乘矩阵可以理解为对矩阵行的线性组合。即:  
```math
\vec{v} M = [v_x \quad v_y \quad v_z]
\left[
\begin{matrix}
-\vec{p}-\\
-\vec{q}-\\
-\vec{r}-
\end{matrix}
\right]=v_x \vec{p} + v_y \vec{q} + v_z \vec{r}
```
> 变换可以理解为基向量从 $(\vec{x},\vec{y},\vec{z})$ 到 $(\vec{p},\vec{q},\vec{r})$ 的变换,对 $\vec{v}$ 做相同的变换。  
> 矩阵M的各行就是新的一组基向量。    

### 4.3 线性代数的宏大图景  
线性代数起初创立是为了解决线性方程组。  
在电子游戏中,与此最相关的是物理引擎:比如强制非穿透性和满足用户需求的关节的约束与动力学体的速度相关;  
其他领域中对线性代数最常见的应用是最小二乘近似(least squares approximation)以及一些数据拟合问题。  
作者推荐阅读MIT教授Gilbert Strang的*Introduction to Linear Algebra*(本科线性代数教材)、*Computational Science and Engineering*(研究生线性代数教材),都是从工科角度应用线性代数。 

### 4.4 练习  
8是用变换解释叉乘  

### 英语学习  
eminently 特别的  
