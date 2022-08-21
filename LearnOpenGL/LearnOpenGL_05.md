# LearnOpenGL

## Part I	Getting started

### 8	Transformations

基本向量、矩阵运算部分略

#### GLM

通过引入GLM库来使用

```c++
#include <glm/glm.hpp>
#include <glm/gtc/matrix_transform.hpp>
#include <glm/gtc/type_ptr.hpp>
```

将一个向量平移

```c++
//创建位移向量
glm::vec4 vec(1.0f, 0.0f, 0.0f, 1.0f);
//创建单位矩阵
glm::mat4 trans = glm::mat4(1.0f);
//通过translate得到位移矩阵
trans = glm::translate(trans, glm::vec3(1.0f, 1.0f, 0.0f));
//对原向量乘以位移矩阵，得到最终结果
vec = trans * vec;
std::cout << vec.x << " " << vec.y << " " << vec.z << std::endl;
```

旋转和缩放

```c++
//旋转矩阵和缩放矩阵
trans = glm::rotate(trans, glm::radians(90.0f), glm::vec3(0.0, 0.0, 1.0));
trans = glm::scale(trans, glm::vec3(0.5f, 0.5f, 0.5f));
```

```c++
//在顶点着色器中设置一个uniform矩阵
#version 330 core
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec2 aTexCoord;

out vec2 TexCoord;
uniform mat4 transform;

void main()
{
    gl_Position = transform * vec4(aPos, 1.0f);
    TexCoord = vec2(aTexCoord.x, aTexCoord.y);
}
```

```c++
//将变换矩阵传递到着色器
unsigned int transformLoc = glGetUniformLocation(ourShader.ID, "transform");
glUniformMatrix4fv(transformLoc, 1, GL_FALSE, glm::value_ptr(trans));
```

#### 总结

OpenGL与GLM的矩阵使用的是列主序