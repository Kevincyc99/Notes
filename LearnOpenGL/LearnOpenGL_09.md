# LearnOpenGL

## Part II	Lighting

### 12	Colors

能够被我们看到的是物体反射的颜色，即无法被物体所吸收的颜色。

将光源的颜色与物体的颜色相乘，得到的就是反射的颜色。

物体的颜色可以定义为**物体从一个光源反射各个颜色分量的大小**。

```c++
glm::vec3 lightColor(1.0f, 1.0f, 1.0f);
glm::vec3 toyColor(1.0f, 0.5f, 0.31f);
glm::vec3 result = lightColor * toyColor; // = (1.0f, 0.5f, 0.31f);
```

#### A lighting scene

创建一个光照场景

首先创建一个箱子，作为被投光的对象。

```c++
//用顶点着色器来绘制箱子
#version 330 core
layout (location = 0) in vec3 aPos;

uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

void main()
{
    gl_Position = projection * view * model * vec4(aPos, 1.0);
}
```

创建一个表示光源的立方体，并创建专门的VAO。

```c++
unsigned int lightVAO;
glGenVertexArrays(1, &lightVAO);
glBindVertexArray(lightVAO);
// 只需要绑定VBO不用再次设置VBO的数据，因为箱子的VBO数据中已经包含了正确的立方体顶点数据
glBindBuffer(GL_ARRAY_BUFFER, VBO);
// 设置灯立方体的顶点属性（对我们的灯来说仅仅只有位置数据）
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
```

创建一个片段着色器，从uniform变量中接收物体以及光源的颜色。

```c++
#version 330 core
out vec4 FragColor;

uniform vec3 objectColor;
uniform vec3 lightColor;

void main()
{
    FragColor = vec4(lightColor * objectColor, 1.0);
}
```

```c++
//在渲染循环中设置颜色
lightingShader.use();
lightingShader.setVec3("objectColor", 1.0f, 0.5f, 0.31f);
lightingShader.setVec3("lightColor",  1.0f, 1.0f, 1.0f);
```

为了让灯源的绘制不受计算影响，需要为灯的绘制创建另一个着色器。

```c++
#version 330 core
out vec4 FragColor;

void main()
{
    FragColor = vec4(1.0); // 将向量的四个分量全部设置为1.0，将光定义为白色
}
```