# LearnOpenGL

## Part I	Getting started

### 6	Shaders

着色器是用一个类C语言GLSL写成的。

着色器的结构：

```c++
//版本号

//输入变量

//输出变量

//uniform变量

int main()
{
	//处理输入并进行一些图形操作
    //将处理过的结果输出到输出变量
}
```

对于顶点着色器，其输入变量也叫<font color = 'green'>顶点属性(Vertex Attribute)</font>

能够声明的顶点属性的数量是有限的，一般由硬件决定，可以通过GL_MAX_VERTEX_ATTRIBS来获取具体上限

```c++
int nrAttributes;
    glGetIntegerv(GL_MAX_VERTEX_ATTRIBS, &nrAttributes);
    std::cout << "Maximum nr of vertex attributes supported: " << nrAttributes << std::endl;
```

#### Types

基本类型：int、float、double、uint、bool

容器：vectors、matrices

#### Vectors

向量有以下形式

```c++
vecn	//默认向量，n个float分量
bvecn	//n个bool分量
ivecn	//n个int分量
uvecn	//n个unsigned int分量
dvecn	//n个double分量
```

对于vec，可以通过vec.x，vec.y， vec.z，vec.w来访问4个分量

对于颜色，使用v.r、v.g、v.b、v.a访问分量，对于纹理，使用v.s、v.t、v.p、v.q访问分量

Vector的特殊语法<font color = 'green'>重组(Swizzling)</font>

```c++
vec2 someVec;
vec4 differentVec = someVec.xyxx;
vec3 anotherVec   = differentVec.zyw;
vec4 otherVec     = someVec.xxxx + anotherVec.yxzy;
```

#### Ins and outs

对于顶点着色器，为了提高效率，它直接从顶点数据中接收输入；通过layout(location = 0)来链接顶点数据

对于片段着色器，它需要一个vec4的颜色输出

可以通过相同的变量名，在不同的着色器之间传递变量

```c
//顶点着色器
#version 330 core
layout (location = 0) in vec3 aPos;		//位置变量的属性位置值为0

out vec4 vertexColor;

void main()
{
    gl_Position = vec4(aPos, 1.0);
    vertexColor = vec4(0.5, 0.0, 0.0, 1.0);
}
```

```c
//片段着色器
#version 330 core
out vec4 FragColor;

in vec4 vertexColor;	//从顶点着色器传入的输入变量，名称和类型必须相同

void main()
{
    FragColor = vertexColor;
}
```

#### Uniforms

Uniforms是一种从CPU中的应用向GPU中的着色器传递数据的方式。

uniforms是全局的，意味着它在每个着色器程序对象中都是独一无二的，且可以被着色器程序的任意着色器在任意阶段访问。

```c
#version 330 core
out vec4 FragColor;
uniform vec4 ourColor;

void main()
{
    FragColor = ourColor;
}
```

此时uniform还是空的，我们可以根据时间来设置它

```c
float timeValue = glfwGetTime();
float greenValue = (sin(timeValue) / 2.0f) + 0.5f;
int vertexColorLocation = glGetUniformLocation(shaderProgram, "ourColor");
glUseProgram(shaderProgram);
glUniform4f(vertexColorLocation, 0.0f, greenValue, 0.0f, 1.0f);
```

通过**glfwGetTime()**获取运行的秒数，然后用**sin**函数设置颜色随时间变化的值，存储到greenValue。

通过**glGetUniformLocation**查询uniform的位置值，为其提供着色器程序和uniform的名字。

最后，通过glUniform4f设置uniform

OpenGL核心是一个C库，因此不支持类型重载

uniform非常适合设置一个会在渲染迭代中改变的值，以及在程序和着色器之间交换数据。



片段着色器中的<font color = 'green'>片段插值(fragment interpolation)</font>：当渲染三角形时，三角形内部的像素会根据顶点颜色进行插值，得到渐变的效果。

#### 总结

着色器：运行在GPU的小程序，负责渲染管线中的几个独立的阶段

​	只能够有输入和输出

​	着色器之间无法直接进行通信，可以通过同名变量的输入与输出进行数据的传递

​	uniform作为全局变量可以作用于各个Shader中

