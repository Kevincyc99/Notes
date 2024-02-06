# GAMES101 Lecture Notes 02  
## Lecture 02  Review of Linear Algebra
图形学涉及的学科知识:
基础数学: 线性代数, 微积分, 数理统计
基础物理: 光学, 力学
其他进阶: 信号处理, 数值分析

线性代数, 图形学主要依赖对于vectors(向量),matrices(矩阵)的理解。
（虎书中关于线性代数,还着重介绍了特征值分解和SVD分解, 其目的可以理解为把一个复杂矩阵分解为多个简单矩阵[如正交矩阵和对角矩阵]的乘积）

#### 点乘(dot product) 
1) 可以得到两个向量的夹角;
2) 一个向量点乘另一个单位向量可以得到该方向上的投影的大小(在实际计算中经常用到),用来分解向量;
#### 叉乘(cross product)
1) 用来建立正交基,可以通过已知的两个向量或者一个向量(需要再指定一个向量)来建立;(虎书2.4.6、2.4.7)
2) 可以表示两个向量所形成的平行四边形面积;
3) 要注意API用的是左手系还是右手系,本课默认右手系;
4) 可以判断向量的左右关系;以及判断一点在三角形的内外;
#### 矩阵
1) 向量点乘可以看成一个nx1的矩阵的转置乘以一个nx1的矩阵  
```math
\vec{a} \cdot \vec{b} = \vec{a}^T\vec{b}=(x_a \quad y_a\quad z_a)
\left(
\begin{matrix}
x_b \\ y_b \\ z_b
\end{matrix}
\right)
 = (x_ax_b+y_ay_b+z_az_b)
```
2) 向量叉乘可以表示成一个特殊的矩阵与b向量的乘积  
```math
\vec{a}\times\vec{b} = A^*\vec{b}=
\left(
\begin{matrix}
0&-z_a&y_a\\ z_a&0&-x_a\\ -y_a&x_a&0
\end{matrix}
\right)
\left(
\begin{matrix}
x_b \\ y_b \\ z_b
\end{matrix}
\right)
```
##### 虎书相关章节: 第二章2.4, 第五章  
PS: 推荐观看3Blue1Brown的线性代数的本质,较为清晰地展现了线性代数的几何意义。  
[【【官方双语/合集】线性代数的本质 - 系列合集】 ](https://www.bilibili.com/video/BV1ys411472E/)



