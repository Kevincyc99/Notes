# Lecture2 Review of Linear Algebra

### Graphics' Dependencies

#### &emsp;Basic Mathematics

&emsp;&emsp;Linear Algebra, Calculus, Statistics

#### &emsp;Basic physics

&emsp;&emsp;Optics, Mechanics

#### &emsp;Misc

&emsp;&emsp;Signal Processing  分析走样问题

&emsp;&emsp;Numerical Analysis  数值分析（渲染--解递归定义的积分、模拟/仿真--解有限元问题或扩散方程）

#### &emsp;A bit of of Aesthetics

------

### Vectors

&emsp;向量(数学)/矢量(物理)

&emsp;Direction  &  Length

### Vector Normalization (归一化)

&emsp;Unit Vector&emsp;&emsp; ![](https://latex.codecogs.com/svg.latex?\hat{a}%20=%20\frac{\vec{a}%20}%20{||\vec{a}||})  

&emsp;单位向量，长度为1，用来表达方向

### Vector Addition

&emsp;几何意义: Parallelogram law (平行四边形法则)

&emsp;代数意义: 坐标相加

### Cartesian Coordinates (笛卡尔坐标系，直角坐标系)

&emsp;图形学中，向量默认是列向量表示 

&emsp;直角坐标易于计算向量长度: <img src = "https://latex.codecogs.com/svg.latex?A%20=%20\left(\begin{matrix}x\\y\end{matrix}%20\right),%20||A||%20=%20\sqrt{x^2%20+%20y^2}">

### Vector Multiplication

#### &emsp;Dot Product (Scalar Product)

&emsp;&emsp;![](https://latex.codecogs.com/svg.latex?\vec{a}%20\cdot%20\vec{b}%20=%20||\hat{a}||||\hat{b}||cos\theta)&emsp;&emsp;&emsp;&emsp;向量（点乘）向量 = 标量

&emsp;&emsp;![](https://latex.codecogs.com/svg.latex?cos\theta%20=%20\frac{\vec{a}%20\cdot%20\vec{b}}{%20||\hat{a}||||\hat{b}||}%20=%20\hat{a}%20\cdot%20\hat{b})&emsp;&emsp;通过单位向量的点乘可以得到两个向量的夹角

##### &emsp;Properties

&emsp;&emsp;交换律、结合律、分配律

##### &emsp;Dot Product in Graphics

&emsp;&emsp;获得两向量的夹角

&emsp;&emsp;获得一个向量在另一个向量的投影（可以分解向量为两垂直分量）

&emsp;&emsp;判断向量的“前后”（正面或背面）

![02_Dot_product_forward_backward](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/02_Dot_product_forward_backward.PNG)

#### &emsp;Cross Product (Vector Product)

&emsp;向量（叉乘）向量 = 向量

&emsp;右手定则（<img src="https://latex.codecogs.com/svg.image?\vec{a}" title="\vec{a}" />旋转到<img src="https://latex.codecogs.com/svg.image?\vec{b}" title="\vec{b}" />，拇指方向为叉积方向）

##### &emsp;Properties

![02_Cross_product_properties](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/02_Cross_product_properties.PNG)

&emsp;<img src="https://latex.codecogs.com/svg.image?\vec{x}" title="\vec{x}" />叉乘<img src="https://latex.codecogs.com/svg.image?\vec{y}" title="\vec{y}" />得到<img src="https://latex.codecogs.com/svg.image?\vec{z}" title="\vec{z}" />，规定为右手坐标系

&emsp;向量叉乘可以表示为矩阵相乘

##### &emsp;&emsp;Cross Product in Graphics

&emsp;&emsp;判定左右（内外）----->光栅化判断像素是否在三角形内

### Matrices

&emsp;Array of numbers (m x n = m rows, n columns)

#### &emsp;Matrix-Matrix Multiplication

&emsp;&emsp;(M x N)(N x P) = (M x P)

&emsp;&emsp;Element (i, j) in the product is the dot product of row i from A and column j from B  

##### &emsp;Properties

&emsp;&emsp;结合律，分配律，没有交换律

&emsp;转置矩阵、逆矩阵、单位矩阵

