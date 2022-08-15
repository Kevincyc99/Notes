# LearnOpenGL

## Part I	Getting started

### 5	Hello Triangle

顶点数组对象：Vertex Array Object，VAO

顶点缓冲对象：Vertex Buffer Object，VBO

索引缓冲对象：Element Buffer Object，EBO 或者Index Buffer Object，IBO

**图形渲染管线(Graphics Pipeline)**

第一部分：将3D坐标转换为2D坐标

第二部分：将2D坐标转换为实际着色的像素

图形渲染管线可以分为多个阶段，每个阶段，通过接收前一个阶段的输出作为输入。由于这些阶段高度专门化，且具有并行执行的特性，现代显卡有成千上万的小处理核心来处理数据。这些核心在GPU上为管线的每个阶段运行小型程序，这些小型程序被称为<font color = 'green'>着色器(Shader)</font>。

OpenGL的着色器用的是<font color = 'green'>OpenGL Shading Language(GLSL)</font>。

渲染管线的抽象划分(蓝色部分代表我们可以加入自己编写的着色器)

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/01_Graphics%20Pipeline.png)

图形渲染管线的输入：3个3D坐标组成的数组，用来表示一个三角形，被称为<font color = 'green'>顶点数据(Vertex Data)</font>，顶点通过<font color = 'green'>顶点属性(Vertex attributes)</font>来表示。顶点属性基本由一些3D位置和一些颜色值组成。

通过<font color = 'green'>图元(Primitive)</font>，我们可以告诉OpenGL如何渲染顶点数据。例如GL_POINTS，GL_TRIANGLES，GL_LINE_STRIP。

第一阶段	<font color = 'green'>顶点着色器(Vertex Shader)</font>

将3D坐标转换为另一种3D坐标，并允许我们对顶点属性进行一些基本处理

第二阶段	<font color = 'green'>图元装配(Primitive Assembly)</font>

将顶点着色器中输出的所有顶点作为输入，并将它们装配成指定形状。

第三阶段	<font color = 'green'>几何着色器(Geometry Shader)</font>

将图元形式的顶点集合作为输入并生成一些其他形状。

第四阶段	<font color = 'green'>光栅化阶段(Rasterization Stage)</font>

将图元映射为最终屏幕上对应的像素，生成<font color = 'green'>片段(Fragment)</font>，并且，在下一个阶段前进行<font color = 'green'>裁切(Clipping)</font>，丢弃超出视图以外的像素，用来提升执行效率。

一个片段是OpenGL中渲染一个像素所需的所有数据

第五阶段	<font color = 'green'>片段着色器(Fragment Shader)</font>

片段着色器是用来计算像素的最终颜色；这也是所有OpenGL高级效果发生的阶段。

第六阶段	<font color = 'green'>测试与混合(Test and Blending)</font>

最后一个阶段为Alpha测试和混合阶段。该阶段进行深度和模板检测，也会检查Alpha值并对对象进行混合。

大多数场合，我们的工作集中在**顶点着色**和**片段着色**；几何着色通常默认即可。

现代OpenGL要求我们定义至少一个顶点着色器和一个片段着色器。

#### Vertex input

OpenGL只会处理坐标轴上(-1.0,1.0)范围内的3D坐标，这个范围内的坐标被称为<font color = 'green'>归一化设备坐标(Normalized Device Coordinates)</font>。这个范围内的坐标会被显示在屏幕上，而其他区域不会被显示。

```c++
//在教程中使用的是基本类型(例如float)，在实际开发中更多使用OpenGL的类型
//使用OpenGL的类型(例如GLfloat)的好处是保证跨平台时每一种类型的大小是统一的。

//定义顶点数组
float vertices[] = {
	-0.5f, -0.5f, 0.0f,
    0.5f, -0.5f, 0.0f,
    0.0f, 0.5f, 0.0f
};
```

通过**GLViewport**可以将NDC转化为<font color = 'green'>屏幕空间坐标(Screen-space Coordinates)</font>

我们通过顶点缓冲对象(VBO)管理这个内存，缓冲对象的优点是可以一次向显卡发送一大批数据。

从CPU向显卡发送数据相对是比较慢的，因此我们尽量一次发送尽可能多的数据。

```c++
//通过glGenBuffers和一个缓冲ID生成一个VBO
unsigned int VBO;
glGenBuffers(1, &VBO);

//通过glBindBuffer将缓冲对象与GL_ARRAY_BUFFER目标绑定
glBindBuffer(GL_ARRAY_BUFFER, VBO);

//通过glBufferData把之前定义的顶点数组复制到缓冲的内存中
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```

**glBufferData**的参数

1	目标缓冲的类型

2	指定数据大小(以字节为单位)

3	实际要传输的数据

4	显卡管理数据的方式。管理方式可以分为三种：

​		GL_STREAM_DRAW：数据不改变且只会被GPU使用很少次数

​		GL_STATIC_DRAW：数据不改变且会被使用很多次

​		GL_DYNAMIC_DRAW：数据会多次被改变且会被使用很多次



以上，我们就将顶点数据存储到显卡的内存中，用VBO进行管理。

#### Vertex shader

```c
#version 330 core		//声明OpenGL版本3.3以及核心模式
layout (location = 0) in vec3 aPos;	
//通过in关键字声明所有输入顶点属性(Input vertex Attributes)

void main()
{
    //在实际应用当中，我们可能需要在这里对数据先进行归一化
	gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);   
}
```

#### Compiling a shader

```c
//我们将顶点着色器的源代码存储到代码文件顶部的C风格常数字符串中
const char *vertexShaderSource = "#version 330 core\n"
    "layout (location = 0) in vec3 aPos;\n"
    "void main()\n"
    "{\n"
    "gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1,0f);\n"
    "}\0";
```

为了让OpenGL使用它，我们必须在运行时动态编译它的源代码。

然后通过**glCreateShader**创建着色器对象，把着色器源代码附加到着色器对象上，然后编译它。

```c
//创建着色器对象
unsigned int vertexShader;
vertexShader = glCreateShader(GL_VERTEX_SHADER);
//附加顶点着色器代码，编译着色器
glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);
glCompileShader(vertexShader);
```

**glShaderSource**的参数：

1	待编译的着色器对象

2	传递的源代码字符串数量

3	实际源代码

4	暂时不管

```c++
//检查编译是否成功
int success;
char infoLog[512];
glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &success);

//若编译失败，返回错误信息
if (!success)
{
    glGetShaderInfoLog(vertexShader, 512, NULL, infoLog);
    std::cout << "ERROR::SHADER::VERTEX::COMPILATION_FAILED\n" << 
        infoLog << std::endl;
}
```

#### Fragment shader

片段着色器是用来计算像素的颜色输出

```c++
//片段着色器的源代码
const char *fragmentShaderSource = "#version 330 core\n"
    								"out vec4 FragColor\n"
    								"void main()\n"
    								"{\n"
    								"FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f);\n"
    								"}\n";
```

```c++
//与顶点着色器类似，创建着色器对象，附加着色器代码，编译着色器
unsigned int fragmentShader;
fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);
glCompileShader(fragmentShader);

//检查编译略
```

接下来需要把两个着色器链接到一个用于渲染的<font color = 'green'>着色器程序(Shader program)</font>。

#### Shader program

通过**glCreateProgram**创建着色器程序，**glAttachShader**附加着色器对象，**glLinkProgram**链接着色器。

```c++
//创建着色器程序
unsigned int shaderProgram;
shaderProgram = glCreateProgram();

//附加着色器对象到着色器程序
glAttachShader(shaderProgram, vertexShader);
glAttachShader(shaderProgram, fragmentShader);
//链接着色器
glLinkProgram(shaderProgram);

//检查编译错误
glGetProgramiv(shaderProgram, GL_LINK_STATUS, &success);
if (!success)
{
    glGetProgramInfoLog(shaderProgram, 512, NULL, infoLog);
    std::cout << "ERROR::SHADER_PROGRAM::LINK_FAILED\n"
                  << infoLog << std::endl;
}
```

通过**glUseProgram**激活着色器程序对象

```c++
//激活程序对象
glUseProgram(shaderProgram);
```

在链接完成后通过glDeleteShader删除着色器对象

```c++
//链接完成之后删除着色器
glDeleteShader(vertexShader);
glDeleteShader(fragmentShader);
```

#### Linking Vertex Attributes

通过glVertexAttribPointer函数告诉OpenGL如何解析顶点数据

```c++
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
```

**glVertexAttribPointer**的参数：

1	要配置的顶点属性的位置值（定义在VertexShaderSource中）

2	顶点属性的大小，顶点属性是vec3，因此大小为3

3	数据类型

4	是否希望数据归一化

5	<font color = 'green'>步长(stride)</font>，连续顶点属性组之间的间隔，<font color = 'green'>紧密排列(tightly packed)</font>时也可以设置为0，由OpenGL决定具体步长

6	表示位置数据在缓冲中起始位置的偏移，是void*类型，需要强制类型转换

**glEnableVertexAttribArray**的参数接收顶点属性位置，启用对应的顶点属性。



在OpenGL中绘制一个物体的伪代码

```c++
//0.复制顶点数组到缓冲中
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
//1.设置顶点属性指针
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
//2.启用着色器程序
glUseProgram(shaderProgram);
//3.绘制物体
someOpenGLFunctionThatDrawsOurTriangle();
```

#### Vertex Array Object

<font color = 'green'>顶点数组对象(Vertex Array Object，VAO)</font>用来存储顶点属性。

OpenGL核心模式需要我们使用VAO，若绑定失败，OpenGL会拒绝绘制任何事情。

一个VAO会存储以下内容：

**glEnableVertexAttribArray**或者**glDisableVertexAttribArray**的调用

通过**glVertexAttribPointer**设置顶点属性

通过**glVertexAttribPointer**调用与顶点属性关联的顶点缓冲对象

```c++
//创建VAO
unsigned int VAO;
glGenVertexArrays(1, &VAO);
```

```c++
//使用VAO的伪代码
//1.绑定VAO
glBindVertexArray(VAO);
//2.将顶点数据拷贝到缓冲
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
//3.设置顶点属性指针
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
//绘制代码(在渲染循环中)
//绘制物体
glUseProgram(shaderProgram);
glBindVertexArray(VAO);
someOpenGLFunctionThatDrawsOurTriangle();
```

当我们需要绘制多个物体时，首先生成并配置VAO(和必须的VBO及属性指针)，然后存储它们。当需要绘制物体时就取出相应的VAO，绑定，在绘制物体后解绑VAO。

OpenGL提供了**glDrawArrays**函数来用当前激活的着色器，之前定义的顶点属性配置和VBO中的顶点数据来绘制图元。

```c++
glUseProgram(shaderProgram);
glBindVertexArray(VAO);
glDrawArrays(GL_TRIANGLES, 0, 3);
```

glDrayArrays的参数

1	绘制的图元种类

2	顶点数组的起始索引

3	绘制的顶点的数量

#### Element Buffer Objects

<font color = 'green'>索引缓冲对象(element buffer objects)</font>专门存储索引，OpenGL通过调用顶点的索引，来调用顶点数据。

```c++
//顶点
float vertices[] = {
    0.5f, 0.5f, 0.0f,
    0.5f, -0.5f, 0.0f,
    -0.5f, -0.5f, 0.0f,
    -0.5f, 0.5f, 0.0f
};
//索引
unsigned int indices[] = {
    0, 1, 3,
    1, 2, 3
};

//创建EBO
unsigned int EBO;
glGenBuffers(1, &EBO);
//绑定EBO缓冲对象
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
//填充EBO数据
glBufferData(GL_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
```

EBO的创建、绑定和填充数据都与VBO类似

```c++
//通过索引进行绘制
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

**glDrawElements**的参数

1	绘制类型

2	索引的个数

3	索引的类型

4	EBO的偏移量

<font color = 'green'>线框模式(Wireframe Mode)</font>

通过glPolygonMode(GL_FRONT_AND_BACK，GL_LINE)配置图元绘制的模式，第一个参数表示应用到三角形的正面和背面，第二个参数表示用线来绘制

### 总结

渲染管线的流程，分为两个部分：

一、把3D坐标转换为2D坐标

二、把2D坐标转换为实际着色的像素并输出到屏幕上

分为九个阶段：

顶点数据->应用阶段->顶点着色器->图元装配->几何着色器->裁剪->屏幕映射->光栅化->片段着色器->测试与混合

其中**顶点着色器**和**片段着色器**为主要的可编程部分

​	顶点着色器主要负责把3D坐标转换为其他类型的3D坐标

​	片段着色器主要负责计算最终像素颜色

着色器的流程

​	创建着色器对象，附加着色器代码，编译着色器，将着色器附加到着色器程序，链接着色器，链接成功后删除

VBO的作用(获取数据)

​	顶点着色器把VBO绑定到缓冲区，进行数据填充，将顶点填充到VBO

VAO的作用

​	通过顶点属性指针来解析数据，存储到VAO，使得OpenGL可以绘制多个物体

EBO的作用

​	通过索引访问顶点数据，节省空间，提高效率，创建、绑定、填充数据流程与VBO类似