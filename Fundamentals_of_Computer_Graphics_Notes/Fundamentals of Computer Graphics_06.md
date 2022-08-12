# Fundamentals of Computer Graphics, Fourth Edition

## 06 Transformation Matrices(已读完)

### 6.1	2D Linear Transformations

The operation takes in a 2-vector and produces another 2-vector by a simple matrix multiplication, ins a ***linear transformation***.

#### 6.1.1	Scaling

$$
\begin{bmatrix}
s_x & 0 \\
0  & s_y \\
\end{bmatrix}
$$

#### 6.1.2	Shearing

shear-x(s)
$$
\begin{bmatrix}
1 & s \\
0  & 1 \\
\end{bmatrix}
$$
shear-y(s)
$$
\begin{bmatrix}
1 & 0 \\
s  & 1 \\
\end{bmatrix}
$$

#### 6.1.3	Rotation

$$
\begin{bmatrix}
cos{\theta} & -sin{\theta} \\
sin{\theta}  & cos{\theta} \\
\end{bmatrix}
$$

默认逆时针旋转(counterclockwise) θ

#### 6.1.4	Reflection

同Scaling

#### 6.1.5	Composition and Decomposition of Transformations

Transforms are applied from the right side first.

#### 6.1.6	Decomposition of Transformations

**Symmetric Eigenvalue Decomposition**

对称矩阵可以表示为 A = RSR<sup>T</sup>

其中R为一正交矩阵，S为对角矩阵，因此，可以R可以看作旋转矩阵，S为缩放矩阵

整个过程可以总结为切变变换

**Singular Value Decomposition（SVD）**

A = USV<sup>T</sup>

旋转再缩放再旋转

**Paeth Decomposition of Rotations**

一个旋转可以用三个切变(shear)来完成

### 6.2	3D Linear Transformations

#### 6.2.1	Arbitrary 3D Rotations

可以通过正交基创建任何旋转矩阵

#### 6.2.2	Transforming Normal Vectors

### 6.3	Translation and Affine Transformations

变换：先平移到原点，做缩放，再旋转，再平移

### 6.4	Inverses of Transformation Matrices

正交矩阵的逆矩阵是它的逆矩阵

可以通过SVD把任何一个矩阵都给降解成R<sub>1</sub>SR<sub>2</sub>的格式
$$
M = R_1scale(\sigma_1, \sigma_2, \sigma_3)R_2
	\\
	M^{-1} = R_1^Tscale(1/\sigma_1, 1/\sigma_2, 1/\sigma_3)R_2^T
$$

### 6.5	Coordinate Transformations

任何坐标轴之间都可以互相转化，过程是互逆的。不过我们通常默认用原点为**o**，基向量为**x**，**y**，**z**的坐标轴作为坐标系统



exercise 有空再做