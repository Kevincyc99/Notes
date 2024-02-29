# 3D Math Primer for Graphics and Game Development Notes 05
书籍网址:[3D Math Primer for Graphics and Game Development](https://gamemath.com/book/)  
阅读时为第二版。  
## Chapter 7 极坐标系
### 7.1 二维极坐标空间
极坐标由 $\theta$ 和 r 组成,r表示距离原点的距离, $\theta$ 表示从极轴转过的角度(逆时针为正方向)。  
从极坐标转笛卡尔坐标很简单:  
```math
x = r\cos{\theta} \qquad y = r\sin{\theta}
```  
从笛卡尔坐标转极坐标,r 很好表示:
```math
r = \sqrt{x^2 + y^2}
```  
但 $\theta$ 不好表示。 
$\theta = \arctan(\frac{y}{x})$ 会丢失一些信息,如x,y同正或同负在这个式子中没有区分。  
CS中一般用atan2来表示 $\theta$ 。  
```math
\theta = atan2(y,x) = \left\{
\begin{aligned}
&0, \qquad &x=0, &y=0,\\
&+90\degree, &x=0, &y>0,\\
&-90\degree, &x=0, &y<0,\\
&\arctan(y/x), &x>0,\\
&\arctan(y/x) + 180\degree, &x<0, &y\geq0,\\
&\arctan(y/x) + 180\degree, &x<0, &y<0.
\end{aligned}
\right.
```  
很多数学库中的atan2没有定义原点。

### 7.2 为什么会有人使用极坐标?  
实际上,人类更习惯于使用极坐标,(例: A在B东北方向10公里),而计算机则更多使用笛卡尔坐标系来解决几何问题。  
在游戏中,在把相机、武器等对准某个目标时,会用笛卡尔坐标转极坐标处理,因为在这种情况下,转向的角度通常是我们需要得到的。  
另外,在球表面通常会用极坐标(比如地球的经纬度)。

### 7.3 三维极坐标空间  
在二维极坐标的基础上,如果再加一个距离值,得到圆柱坐标系,如果再加一个角度,得到球坐标系。  
圆柱坐标系就是二维极坐标再加上z轴。  
球坐标表示如图:  
![spherical_coordinates](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/spherical_coordinates.png)  
$\theta$ 为方位角, $\phi$ 为仰角。  
基于游戏工业的实际应用以及本书采用左手系,作者重新调整了一下球坐标系的参数:  
![renamed_spherical_coordinates](https://cdn.jsdelivr.net/gh/Kevincyc99/PicBed@master/Notes/renamed_spherical_coordinates.png)  
为了让球坐标系能够被无歧义地使用,需要一些约束:  
```math
\begin{aligned}
&r \geq 0 \\
-180 \degree < &h \leq 180 \degree \\
-90 \degree < &p \leq 90 \degree \\
r = 0 &\Rightarrow h = p = 0 \\
|p| = 90 \degree &\Rightarrow h = 0
\end{aligned}
```  
球坐标系转笛卡尔坐标系,方法基本同大学高数课上的坐标转换。
```math
\begin{aligned}
x &= r\cos{p}\sin{h} \qquad &y &= -r\sin{p} \qquad &z &= r\cos{p}\cos{h} \\
r &= \sqrt{x^2 + y^2 + z^2} \qquad &h &= atan2(x,z) \qquad &p &= \arcsin(-y/r)
\end{aligned}
```  
### 7.4 用极坐标表示向量  
基本和表示点的方法一样,表示从原点出发的向量。

### 7.5 练习  
1~10都是计算,11画出图形,12是道古早的测试题。

### 英语学习
esoteric 秘传的、只有内行才懂的  
azimuth 方位角  
zenith 天顶、仰角  
semantics 语义学,(其他符号系统的)含义  
grievance 委屈、抱怨  
