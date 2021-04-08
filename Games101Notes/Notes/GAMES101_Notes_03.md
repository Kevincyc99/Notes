# Lecture3 Transformation

Modeling (Translation, Rotation, Scaling)

正运动学：已知各关节的角度，求末端的位姿

逆运动学：已知末端的位姿、求各关节的转角

Viewing (Projection)

### 2D transformations

缩放变换

![scale](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/03_Scale.PNG)

![scale non-uniform](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/03_Scale_Non_Uniform.PNG)

反射/对称变换

![reflection](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/03_Reflection.PNG)

切变变换

![shear](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/03_Shear.PNG)

旋转变换（默认围绕原点逆时针旋转）

![rotate](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/03_Rotate.PNG)

线性变换可以用矩阵相乘表示

![linear transforms](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/03_Linear%20Transforms.PNG)

### Homogeneous Coordinates  

Why？

&emsp;解决非线性变换的表达（平移）

&emsp;引入齐次坐标可以统一所有的变换表达

![Why_Homogenous_Coordinates](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/03_Why_Homogenous_Coordinates.PNG)

点与向量的运算

![](https://raw.githubusercontent.com/Kevincyc99/Notes/main/Games101Notes/Images_Notes/03_Point_Vector_Operation.PNG)

Affine Transformations（仿射变换）= 线性变换+平移变换

Inverse Transform（逆变换）：乘变换矩阵的逆矩阵

变换顺序影响结果----->矩阵相乘不满足交换律

矩阵相乘为左乘，从右到左逐个应用变换矩阵

2维变换->3维变换（同样添加一个维度）

仿射变换先线性变换，后平移