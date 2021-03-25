# Lecture2 Review of Linear Algebra

### Graphics' Dependencies

#### &emsp;Basic Mathematics

&emsp;Linear Algebra, Calculus, Statistics

#### &emsp;Basic physics

&emsp;Optics, Mechanics

#### &emsp;Misc

&emsp;Signal Processing  分析走样问题

&emsp;Numerical Analysis  数值分析（渲染--解递归定义的积分、模拟/仿真--解有限元问题或扩散方程）

#### &emsp;A bit of of Aesthetics

------

### Vectors

&emsp;向量(数学)/矢量(物理)

&emsp;Direction  &  Length

### Vector Normalization (归一化)

&emsp;Unit Vector ![](https://latex.codecogs.com/svg.latex?\hat{a}%20=%20\frac{\vec{a}%20}%20{||\vec{a}||})  单位向量，长度为1，用来表达方向

### Vector Addition

&emsp;几何意义: Parallelogram law (平行四边形法则)

&emsp;代数意义: 坐标相加

### Cartesian Coordinates (笛卡尔坐标系，直角坐标系)

&emsp;图形学中，向量默认是列向量表示 

&emsp;直角坐标易于计算向量长度: &emsp;![1](https://latex.codecogs.com/svg.latex?A%20=%20\left(\begin{matrix}x\\y\end{matrix}%20\right),%20||A||%20=%20\sqrt{x^2%20+%20y^2})

### Vector Multiplication

#### &emsp;Dot Product (Scalar Product)

&emsp;![](https://latex.codecogs.com/svg.latex?\vec{a}%20\cdot%20\vec{b}%20=%20||\hat{a}||||\hat{b}||cos\theta)&emsp;&emsp;&emsp;&emsp;向量（点乘）向量 = 标量

&emsp;![](https://latex.codecogs.com/svg.latex?cos\theta%20=%20\frac{\vec{a}%20\cdot%20\vec{b}}{%20||\hat{a}||||\hat{b}||}%20=%20\hat{a}%20\cdot%20\hat{b})&emsp;&emsp;通过单位向量的点乘可以得到两个向量的夹角

##### &emsp;Properties

&emsp;交换律、结合律、分配律

