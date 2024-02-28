# 3D Math Primer for Graphics and Game Development Notes 05
书籍网址:[3D Math Primer for Graphics and Game Development](https://gamemath.com/book/)  
阅读时为第二版。  
## Chapter 6 矩阵的更多内容
### 6.1 矩阵的行列式  
行列式(determinant),有时也缩写为 det M,记作 $|M|$ 。
这部分基本所有线性代数课本都会讲,简单行列式的计算、余子式、行列式的性质。  
行列式可以用来判断变换矩阵的一些性质:  
1.  行列式大小变化可以反映向量形成的平行四边形(平行六面体)的大小变化  
2.  行列式的符号可以帮助判断变换中是否包含投影和反射
3.  如果行列式为0,说明变换中包含投影变换。  

个人理解,行列式在图形中的作用主要还是作为工具,来计算逆矩阵或者伴随矩阵之类的,可能在编写数学库的时候有用,而行列式本身在实际过程中并不太常用。  

### 6.2 矩阵的逆  
  
可逆矩阵又称非奇异(nonsingular)矩阵,行列式不为0,可逆方阵的行或列都是线性无关的;  
不可逆矩阵又称奇异(singular)矩阵,行列式为0。 

虽然对于高维方阵来说,高斯消元法会更快,但是对于通常只有4维以下方阵的图形学来说,用伴随矩阵计算逆更合适,因为伴随矩阵计算不会产生if分支或者循环。  

### 6.3 正交矩阵  
正交矩阵的转置与逆相同。  
```math
M^T = M^{-1}
```
正交矩阵的这一性质,使得正交矩阵的逆很好求,因此正交矩阵在图形学实践中很常见。  
只包含旋转或反射的变换矩阵是正交的。  
正交矩阵的行都是单位向量,且相互垂直——是一组正交基。  
**通常我们都是在已知矩阵是正交矩阵的情况下才利用其性质,** 判断一个矩阵是否是正交矩阵会产生很大开销,且如果最后发现该矩阵不正交,那么判断产生的开销就被浪费掉了。  
可以用施密特正交化来产生一个正交矩阵。

### 6.4 四维齐次矩阵  
w $\neq$ 0, 表示的是点(x/w, y/w, z/w, 1);  
w = 0, 可以视为一个位于无穷远的点,故可视为一个方向,而不是一个位置。  
因此, $(x, y, z, 0)^T$ 表示向量, $(x, y, z, 1)^T$ 表示点。  
齐次矩阵统一了线性变换和平移变换的操作,也就是仿射变换。  

### 6.5 四维矩阵和透视投影  
本书介绍的透视投影,是小孔成像,成像与原物体在投影中心两侧,因此是倒立且反向的。实际中,由于投影的负号没有必要,因此把该成像对称到原物体的同侧。  
理论:
![perspective_projection_1](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/perspective_projection_1.png)  
```math
\begin{aligned}
\vec{p} &= [x \quad y \quad z] \\
\vec{p^{\prime}} &= [-dx/z \quad -dy/z \quad -d]
\end{aligned}
```  
实际:  
![perspective_projection_2](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/perspective_projection_2.png)  
```math
\begin{aligned}
\vec{p} &= [x \quad y \quad z] \\
\vec{p^{\prime}} &= [dx/z \quad dy/z \quad d]
\end{aligned}
```  
### 6.6 练习  
都是些变换相关的练习,把变换写成齐次形式。