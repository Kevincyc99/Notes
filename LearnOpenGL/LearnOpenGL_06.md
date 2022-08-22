# LearnOpenGL

## Part I	Getting started

### 9	Coordinate Systems

五个重要的坐标系统：

局部空间(物体空间)	Local space(Object space)

世界空间					 World space

观察空间(视觉空间)	View space(Eye space)

裁剪空间					 Clip space

屏幕空间					 Screen space

#### The global picture

顶点坐标最初位于<font color = 'green'>局部空间(Local Space)</font>，称作<font color = 'green'>局部坐标(Local Coordinates)</font>，之后会转变为<font color = 'green'>世界坐标(World Coordinates)</font>、<font color = 'green'>观察坐标(View Coordinates)</font>、<font color = 'green'>裁剪坐标(Clip Coordinates)</font>，并最终以<font color = 'green'>屏幕坐标(Screen Coordinates)</font>的形式结束。

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/09_Coordinate_Systems.png)

局部坐标是物体相对于局部原点的坐标，是物体起始坐标

世界空间坐标是相对于世界的全局原点

观察空间坐标是从摄像机或者观察者的角度进行观察的

之后投影到裁剪坐标，会保留(-1,1)的部分，丢弃范围外的部分

进行<font color = 'green'>视口变换(Viewport Transform)</font>，得到屏幕坐标

#### Local space

模型的所有顶点都是位于局部空间的。

#### World space

通过**模型矩阵**，可以将局部坐标变换到世界坐标。

#### View space

将世界坐标转化为用户视野前方的坐标，通过**观察矩阵**从世界坐标转变为观察坐标。

#### Clip space

通过**投影矩阵**将观察坐标转变为裁剪坐标，也是NDC的范围之内。

由投影矩阵创建的观察箱(viewing box)，又叫<font color = 'green'>平截头体(frustum)</font>，每个出现在平截头体范围内的坐标会最终显示在用户的屏幕上。

在所有顶点被变换到裁剪空间之后，通过<font color = 'green'>透视除法(Perspective Division)</font>将平截头体上的坐标压缩到一个标桩立方体的范围上。

​	正射投影(Orthographic projection)

通过{near，far}，{left，right}，{bottom，top}三对坐标指定了一个平截头体（此时为一长方体）的范围

```c++
//创建一个正射投影矩阵
glm::ortho(0.0f, 800.0f, 0.0f, 600.0f, 0.1f, 100.0f);
```

前两个参数定义了左右坐标，第三、四个坐标定义了底部和顶部，最后两个定义了近、远坐标。

​	透视投影(Perspective projection)

可以实现近大远小的透视效果

```c++
//创建一个透视投影矩阵
glm::mat4 proj = glm::perspective(glm::radians(45.0f), (float)width / (float)height, 0.1f, 100.0f);
```

**perspective**函数的参数：

1	fov值，表示视角

2	设置宽高比(aspect ratio)

3、4	设置近远平面到摄像机的距离

​	近平面通常设置为0.1f，当near值设置的太大(10.0f)时，OpenGL会将靠近摄像机的坐标都裁剪掉，产生的效果是<u>在太过靠近一个物体时你的视线会直接穿过去</u>。

#### Putting it all together

一个顶点坐标到裁剪坐标的过程：
$$
V_{clip} = M_{projection} \cdot M_{view} \cdot M_{model} \cdot V_{local}
$$
当得到裁剪坐标之后，可以赋值到gl_Position，OpenGL会自动进行透视除法和裁剪，将它们进行归一化，映射到屏幕坐标

#### Going 3D

```c++
//模型矩阵
glm::mat4 model = glm::mat4(1.0f);
model = glm::rotate(model, glm::radians(-55.0f), glm::vec3(1.0f, 0.0f, 0.0f));
//通过该变换，让图片绕x轴顺时针转55°，即向后方向转55°，视觉上就好像图片躺在地面上
//实际是做一个旋转变换
```

```c++
//观察矩阵
glm::mat4 view = glm::mat4(1.0f);
view = glm::translate(view, glm::vec3(0.0f, 0.0f, -3.0f));
//实际是做一个平移变换
```

```c++
//投影矩阵
glm::mat4 projection = glm::mat4(1.0f);
projection = glm::perspective(glm::radians(45.0f), 800.0f / 600.0f, 0.1f, 100.0f);
```

```c++
//着色器中声明矩阵
#version 330 core
layout (location = 0) in vec3 aPos;
...
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

void main()
{
    // 注意乘法要从右向左读
    gl_Position = projection * view * model * vec4(aPos, 1.0);
    ...
}
```

```c++
//将矩阵传入着色器(分别展示了三种传矩阵的方法)
unsigned int modelLoc = glGetUniformLocation(ourShader.ID, "model");
glUniformMatrix4fv(modelLoc, 1, GL_FALSE, glm::value_ptr(model));

unsigned int viewLoc = glGetUniformLocation(ourShader.ID, "view");
glUniformMatrix4fv(viewLoc, 1, GL_FALSE, &view[0][0]);

ourShader.setMat4("projection", projection);
```

通过引入更多的顶点，我们用上述方法可以得到立方体。



但是对于立方体，有些遮挡关系是错误的，(当物体运动起来更加明显)，我们需要引入深度测试来判断是否进行覆盖。

#### z-buffer

OpenGL将所有深度信息存储于一个z缓冲，也称为<font color = 'green'>深度缓冲(Depth Buffer)</font>，当片段想要输出

颜色时，OpenGL会将它的深度值和z缓冲中的深度值进行对比，若当前片段的深度值大，则会被丢弃，否则将覆盖原来的z缓冲，该过程也被称为<font color = 'green'>深度测试(Depth Testing)</font>。

```c++
//开启深度测试
glEnable(GL_DEPTH_TEST);
//在glClear函数中指定DEPTH_BUFFER_BIT位来清除深度缓冲
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
```



#### 总结

坐标空间：

​	**局部空间**——模型矩阵——**世界空间**——观察矩阵——**观察空间**——投影矩阵——**裁剪空间**——视口变换——**屏幕空间**

​	局部空间->世界空间，将多个物体放置在以全局原点为原点的坐标系中

​	世界空间->观察空间，从摄像机视角描述物体的坐标

​	观察空间->裁剪空间，通过投影，将物体的坐标归一化到NDC

​	裁剪空间->屏幕空间，通过视口变换，最终得到物体的屏幕坐标

正射投影 & 透视投影：

​	正射投影中，物体不会因为远近而发生大小变化；

​	透视投影由近大远小的效果，OpenGL中会自动利用透视除法和裁剪，得到屏幕坐标