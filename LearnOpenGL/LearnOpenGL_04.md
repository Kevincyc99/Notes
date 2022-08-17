# LearnOpenGL

## Part I	Getting started

### 7	Textures

<font color = 'green'>纹理(texture)</font>是一个2D图片，可以用来添加物体的细节。

为了把纹理<font color = 'green'>映射(map)</font>到三角形上，需要每个顶点关联一个<font color = 'green'>纹理坐标(texture coordinate)</font>，之后再对其他片段进行片段插值。通过纹理坐标获取纹理颜色的过程叫<font color = 'green'>采样(sampling)</font>。

#### Texture Wrapping

纹理坐标的范围通常是(0,0)到(1,1)。如果纹理坐标设置在范围之外，OpenGL默认会重复这个纹理图像，但也有其他方式：

GL_REPEAT：默认行为

GL_MIRRORED_REPEAT：镜像重复

GL_CLAMP_TO_EDGE：超出部分会重复纹理坐标的边缘，产生一种边缘被拉伸的效果

GL_CLAMP_TO_BORDER：超出坐标为用户指定的边缘颜色

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/02_Texture_Wrapping.png)



```c++
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_MIRRORED_REPEAT);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_MIRRORED_REPEAT);
```

**glTexParameter***函数可以分别对单独的一个坐标轴设置

**glTexParameter**的参数：

1	纹理目标

2	纹理轴

3	纹理环绕方式

(4)	边缘颜色	(只在纹理环绕方式为GL_CLAMP_TO_BORDER时需要)

#### Texture Filtering

纹理坐标与纹理像素

​	纹理坐标是我们给顶点设置的数组，OpenGL以这个顶点的纹理坐标去查找纹理图像上的像素，进行采样，提取纹理像素的颜色。

两种纹理过滤：

​	GL_NEAREST(<font color = 'green'>邻近过滤，(Nearest Neighbor Filtering)</font>)，OpenGL默认纹理过滤方式，会选择最接近纹理坐标的那个像素。

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/03_Nearest_Neighbor_Filtering.png)

​	GL_LINEAR(<font color = 'green'>线性过滤，(bi)linear filtering)</font>)，基于纹理坐标附近的纹理像素，计算出一个插值，近似出一个颜色。

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/04_Linear_Filtering.png)

两种纹理过滤方式对比

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/05_Texture_Filtering.png)

邻近过滤会产生颗粒，线性过滤会更平滑但模糊。

我们可以在放大或缩小时分别设置纹理过滤的选项。

```c++
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```

#### Mipmaps

为了调整不同纹理大小的显示，OpenGL引入<font color = 'green'>多级渐远纹理(Mipmap)</font>。

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/06_Mipmap.png)

可以根据物体距离观察者的距离来选取相应大小的纹理

在切换不同级别的Mipmap时也会产生瑕疵(artifacts)，  与纹理过滤一样，可以采用过滤方式：

GL_NEAREST_MIPMAP_NEAREST：使用最邻近的Mipmap匹配像素大小，并用邻近插值进行纹理采样

GL_LINEAR_MIPMAP_NEAREST：使用最邻近的Mipmap匹配像素大小，并用线性插值进行纹理采样

GL_NEAREST_MIPMAP_LINEAR：在两个最匹配的Mipmaps之间进行线性插值，并用邻近插值进行纹理采样

GL_LINEAR_MIPMAP_LINEAR：在两个最匹配的Mipmaps之间进行线性插值，并用线性插值进行纹理采样

Mipmap通过用于纹理被缩小的情况，在纹理放大过滤时设置mipmap过滤没有任何效果。

#### Loading and creating textures

在加载纹理时我们通常使用一个支持多种流行格式的图像加载库，例如stb_image.h。

我们可以使用**stbi_load**加载图片到项目中。

```c++
int width, height, nrChannels;
unsigned char *data = stbi_load("container.jpg", &width, &height, &nrChannels, 0);
```

**stbi_load**的参数：

1	图像文件的位置

2~4	宽度、高度、颜色通道的个数(例如rgb为3，rgba为4)

#### Generating a texture

和之前生成的OpenGL一样，纹理也是使用ID引用。

```c++
//生成纹理
unsigned int texture;
glGenTextures(1, &texture);
//绑定纹理
glBindTexture(GL_TEXTURE_2D, texture);

//加载图片
int width, height, nrChannels;
unsigned char *data = stbi_load("container.jpg", &width, &height, &nrChannels, 0);

//加载图片到纹理
glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
//生成Mipmap
glGenerateMipmap(GL_TEXTURE_2D);
```

**glTexImage2D**的参数：

1	纹理目标，设置为GL_TEXTURE_2D意味着会生成与当前绑定的纹理对象在同一个目标上的纹理

2	mipmap级别，0为基本级别

3	纹理的格式

4~5	宽度和高度

6	设置为0，没有实际意义，历史遗留问题

7~8	源图的格式以及数据类型

9	实际图像数据

整个加载、创建、生成纹理的过程如下：

```c++
unsigned int texture;
glGenTextures(1, &texture);
glBindTexture(GL_TEXTURE_2D, texture);
// 为当前绑定的纹理对象设置环绕、过滤方式
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);   
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
// 加载并生成纹理
int width, height, nrChannels;
unsigned char *data = stbi_load("container.jpg", &width, &height, &nrChannels, 0);
if (data)
{
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
    glGenerateMipmap(GL_TEXTURE_2D);
}
else
{
    std::cout << "Failed to load texture" << std::endl;
}
//释放图像内存
stbi_image_free(data);
```

#### Applying textures

需要把纹理坐标设置到顶点数组中

```c++
float vertices[] = {
//     ---- 位置 ----       ---- 颜色 ----     - 纹理坐标 -
     0.5f,  0.5f, 0.0f,   1.0f, 0.0f, 0.0f,   1.0f, 1.0f,   // 右上
     0.5f, -0.5f, 0.0f,   0.0f, 1.0f, 0.0f,   1.0f, 0.0f,   // 右下
    -0.5f, -0.5f, 0.0f,   0.0f, 0.0f, 1.0f,   0.0f, 0.0f,   // 左下
    -0.5f,  0.5f, 0.0f,   1.0f, 1.0f, 0.0f,   0.0f, 1.0f    // 左上
};
```

添加顶点属性指针

```c++
glVertexAttribPointer(2, 2, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)(6 * sizeof(float)));
glEnableVertexAttribArray(2);
```

调整顶点着色器，让顶点多接收一个属性(纹理坐标)，输出TexCoord

```c++
#version 330 core
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec3 aColor;
layout (location = 2) in vec2 aTexCoord;

out vec3 ourColor;
out vec2 TexCoord;

void main()
{
    gl_Position = vec4(aPos, 1.0);
    ourColor = aColor;
    TexCoord = aTexCoord;
}
```

在片段着色器中接收TexCoor，创建uniform采样器(Sampler)

```c++
#version 330 core
out vec4 FragColor;

in vec3 ourColor;
in vec2 TexCoord;

uniform sampler2D ourTexture;

void main()
{
    FragColor = texture(ourTexture, TexCoord);
}
```

**texture**函数接收两个参数，第一个是纹理采样器，第二个是对应的纹理坐标

通过纹理坐标对纹理进行采样，将得到的颜色输出

在glDrawElements之前绑定纹理，在glDrawElements中会自动将纹理赋给纹理采样器

```c++
glBindTexture(GL_TEXTURE_2D, texture);
glBindVertexArray(VAO);
gDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

#### Texture Units

我们可以用glUniform1i给纹理采样器分配一个位置值。一个纹理的位置值通常称为一个<font color = 'green'>纹理单元(Texture Unit)</font>。一个纹理的默认单元为0。纹理单元可以让我们在着色器中使用多个纹理。通过**glActiveTexture**激活纹理单元，再进行绑定。

OpenGL至少保证有16个纹理单元可以使用。

在片段着色器中可以通过**mix**进行线性插值。

```c++
#version 330 core
...

uniform sampler2D texture1;
uniform sampler2D texture2;

void main()
{
    FragColor = mix(texture(texture1, TexCoord), texture(texture2, TexCoord), 0.2);
}
```

mix的参数

1~2	纹理的颜色输出

3	一个0到1的float，0为第一个纹理输出，1为第二个纹理输出，0.2为0.8的第一个输出和0.2的第二个输出进行混合

通过绑定不同的纹理单元来激活不同的纹理

```c++
glActiveTexture(GL_TEXTURE0);
glBindTexture(GL_TEXTURE_2D, texture1);
glActiveTexture(GL_TEXTURE1);
glBindTexture(GL_TEXTURE_2D, texture2);

glBindVertexArray(VAO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

在渲染循环之前，通过glUniform1i设置每个采样器来告诉OpenGL每个Sampler属于哪个Texture Unit

```c++
ourShader.use();//需要先激活着色器程序
//glUniform1i(glGetUniformLocation(ourShader.ID, "texture1"), 0);//手动设置
ourShader.setInt("ourTexture2", 0);//使用着色器设置
ourShader.setInt("ourTexture2", 1);

while(...)
{
    [...]
}
```

OpenGL中y轴的0.0是在图片底部，而通常图片的y轴0.0是在顶部，因此纹理会上下颠倒。

```c++
//将一下语句加入到加载图像之前就可以让纹理正常显示
stbi_set_flip_vertically_on_load(true);//y轴翻转
```

#### 总结

纹理可以是一张图片，可以是1D、2D或者3D的。(纹理也可以存储其他格式的内容)

图片的坐标轴向

![]()

纹理的坐标范围是(0,0)到(1,1)

![]()

纹理的环绕方式

​	GL_REPEAT	重复

​	GL_MIRRORED_REPEAT	镜像重复

​	GL_CLAMP_TO_EDGE	边界外重复边缘

​	GL_CLAMP_TO_BORDER	边界外设置默认颜色

纹理的过滤方式

​	邻近过滤	直接取最邻近的像素，会产生锯齿

​	线性过滤	用邻近的几个像素进行插值，比较平滑但模糊

Mipmap

​	主要用于缩小，根据占用像素的多少来调用合适级别的mipmap

将纹理映射到三角面上的过程

​	在顶点数据中定义纹理坐标，创建纹理对象，绑定到纹理目标，设置纹理的环绕方式和过滤方式，加载图片，激活纹理单元，在片段着色器中定义采样器，根据纹理坐标进行插值，得到最终的纹理映射。

可以通过mix将纹理进行混合