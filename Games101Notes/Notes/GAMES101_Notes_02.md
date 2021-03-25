# Lecture2 Review of Linear Algebra

### Graphics' Dependencies

#### &emsp;Basic Mathematics

Linear Algebra, Calculus, Statistics

#### &emsp;Basic physics

Optics, Mechanics

#### &emsp;Misc

Signal Processing  分析走样问题

Numerical Analysis  数值分析（渲染--解递归定义的积分、模拟/仿真--解有限元问题或扩散方程）

#### &emsp;A bit of of Aesthetics

------

### Vectors

向量(数学)/矢量(物理)

Direction  &  Length

### Vector Normalization (归一化)

Unit Vector $\hat{a} = \vec{a} / ||\vec{a}||$   单位向量，长度为1，用来表达方向

### Vector Addition

几何意义: Parallelogram law (平行四边形法则)

代数意义: 坐标相加

### Cartesian Coordinates (笛卡尔坐标系，直角坐标系)

图形学中，向量默认是列向量表示 

直角坐标易于计算向量长度: $A = \left(\begin{matrix}x\\y\end{matrix} \right), ||A|| = \sqrt{x^2 + y^2}$

### Vector Multiplication

#### Dot Product (Scalar Product)

$\vec{a} \cdot \vec{b} = ||\hat{a}||||\hat{b}||cos\theta$    向量（点乘）向量 = 标量

$cos\theta = \frac{\vec{a} \cdot \vec{b}}{ ||\hat{a}||||\hat{b}||} = \hat{a} \cdot \hat{b}$    通过单位向量的点乘可以得到两个向量的夹角

##### Properties

交换律、结合律、分配律

