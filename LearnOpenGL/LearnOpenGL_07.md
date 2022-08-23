# LearnOpenGL

## Part I	Getting started

### 10	Camera

OpenGL中本身没有摄像机的概念，但我们可以通过将场景中所有物体进行移动，产生一种是我们(摄像机)在移动，而不是场景在移动。

#### View space

为了定义一个摄像机，我们需要定义其在世界空间的位置、观察的方向、一个指向它右侧的向量和一个指向它上方的向量。

```c++
//摄像机位置
glm::vec3 cameraPos = glm::vec3(0.0f, 0.0f, 3.0f);
```

z轴是从屏幕指向我们的，因此，如果我们希望摄像机向后移动，则应该让摄像机沿z轴正方向移动。

通过用看向的目标的坐标减去摄像机坐标，得到的向量就是摄像机方向

```c++
//假设摄像机看向原点
glm::vec3 cameraTarget = glm::vec3(0.0f, 0.0f, 0.0f);
//glm::vec3 cameraDirection = glm::normalize(cameraTarget - cameraPos);
glm::vec3 cameraDirection = glm::normalize(cameraPos - cameraTarget);
//为了让方向向量指向z轴正向，交换了顺序，如此得到的方向向量与摄像机实际指向的方向正好相反
```

```c++
//右轴
glm::vec3 up = glm::vec3(0.0f, 1.0f, 0.0f);
glm::vec3 cameraRight = glm::normalize(glm::cross(up, cameraDirection));
//上轴
glm::vec3 cameraUp = glm::cross(cameraDirection, cameraRight);
```

通过Gram-Schmidt Process得到摄像机的位置、朝向、右轴以及上轴。

#### Look At

通过刚才得到的三个轴和平移向量(摄像机的位置向量)，可以建立LookAt矩阵：

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/10_LookAt_Matrix.png)

R是右轴，U是上轴，D是方向向量，P是位置向量

在GLM中，只需要定义一个摄像机位置、一个目标位置以及一个表示世界空间中的上向量就可以创建LookAt矩阵。

```c++
glm::mat4 view;
view = glm::lookAt(glm::vec3(0.0f, 0.0f, 3.0f),
                   glm::vec3(0.0f, 0.0f, 0.0f),
                   glm::vec3(0.0f, 1.0f, 0.0f));
```

#### Walk around

```c++
//定义摄像机
glm::vec3 cameraPos = glm::vec3(0.0f, 0.0f, 3.0f);
glm::vec3 cameraFront = glm::vec3(0.0f, 0.0f, -1.0f);
glm::vec3 cameraUp = glm::vec3(0.0f, 1.0f, 0.0f);

//LookAt可以根据Camera进行调整
view = glm::lookAt(cameraPos, cameraPos + cameraFront, cameraUp);
```

```c++
//通过按键进行摄像机位置的的修改
void processInput(GLFWwindow *window)
{
    ...
    float cameraSpeed = 0.01f; // adjust accordingly
    if (glfwGetKey(window, GLFW_KEY_W) == GLFW_PRESS)
        cameraPos += cameraSpeed * cameraFront;
    if (glfwGetKey(window, GLFW_KEY_S) == GLFW_PRESS)
        cameraPos -= cameraSpeed * cameraFront;
    if (glfwGetKey(window, GLFW_KEY_A) == GLFW_PRESS)
        cameraPos -= glm::normalize(glm::cross(cameraFront, cameraUp)) * cameraSpeed;
    if (glfwGetKey(window, GLFW_KEY_D) == GLFW_PRESS)
        cameraPos += glm::normalize(glm::cross(cameraFront, cameraUp)) * cameraSpeed;
}
```

#### Movement speed

为了让摄像机在任何硬件上的移动速度都一样，可以给cameraSpeed乘上一个deltaTime

```c++
float deltaTime = 0.0f;	//当前帧与上一帧的时间差
float lastFrame = 0.0f;	//上一帧的时间

//每一帧进行deltaTime计算
float currentFrame = glfwGetTime();
deltaTime = currentFrame - lastFrame;
lastFrame = currentFrame;

//新设置的摄像机速度
void processInput(GLFWwindow *window)
{
  float cameraSpeed = 2.5f * deltaTime;
  ...
}
```

为了能够改变视角，我们可以引入鼠标的输入改变cameraFront

#### Euler angles

欧拉角是可以表示3D空间中任何旋转的三个值，共有三种：俯仰角(Pitch)、偏航角(Yaw)、滚转角(Roll)

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/11_EulerAngles.png)

对于摄像机系统，我们只关心俯仰角和偏航角

```c++
glm::vec3 direction;
//考虑yaw
direction.x = cos(glm::radians(yaw));
direction.z = sin(glm::radians(yaw));
//考虑pitch
direction.x = cos(glm::radians(yaw)) * cos(glm::radians(pitch));
direction.y = sin(glm::radians(pitch));
direction.z = sin(glm::radians(yaw)) * cos(glm::radians(pitch));
//这里的direction表示摄像机的前轴(Front)
```

鼠标的水平移动影响偏航角，竖直移动影响俯仰角，原理是存储上一帧的鼠标位置，根据当前帧鼠标位置的改变来控制角度变化。

```c++
//通过以下函数告诉GLFW，要隐藏光标，并捕捉它
glfwSetInputMode(window, GLFW_CURSOR, GLFW_CURSOR_DISABLED);
//监听鼠标移动事件
void mouse_callback(GLFWwindow* window, double xpos, double ypos);
//当鼠标移动时，mouse_callback
glfwSetCursorPosCallback(window, mouse_callback);
```

处理FPS风格的摄像机的鼠标输入时，在最终取得方向向量之前：

1、计算鼠标距上一帧的位置偏移量

2、把偏移量添加到摄像机的俯仰角和偏航角中

3、对俯仰角和偏航角进行最大最小值的限制

4、计算方向向量

```c++

//将上一帧的位置初始化为屏幕中心
float lastX = 400, lastY = 300;

void mouse_callback(GLFWwindow* window, double xpos, double ypos)
{
    //通过判断是否是第一次鼠标输入，若是则进行初始化，避免突变
    if (firstMouse)
    {
        lastX = xpos;
        lastY = ypos;
        firstMouse = false;
    }
    //1
    //在鼠标回调函数中计算偏移量
    float xoffset = xpos - lastX;
    float yoffset = lastY - ypos;	//这里相反是因为y轴从下到上是递增的，而mouse_callback中(x,y)是相对于窗口左上角的位置
    lastX = xpos;
    lastY = ypos;
    //通过灵敏度控制旋转的速度
    float sensitivity = 0.05f;
    xoffset *= sensitivity;
    yoffset *= sensitivity;
    //2
    yaw += xoffset;
    pitch += yoffset;
    //3
    if (pitch > 89.0f)
        pitch = 89.0f;
    if (pitch < -89.0f)
        pitch = -89.0f;
    //4
    glm::vec3 front;
    front.x = cos(glm::radians(yaw)) * cos(glm::radians(pitch));
    front.y = sin(glm::radians(pitch));
    front.z = sin(glm::radians(yaw)) * cos(glm::radians(pitch));
    cameraFront = glm::normalize(front);
}
```

#### Zoom

通过鼠标滚轮改变FOV

```c++
void scroll_callback(GLFWwindow* window, double xoffset, double yoffset)
{
    fov -= (float)yoffset;
    if (fov < 1.0f)
        fov = 1.0f;
   	if (fov > 45.0f)
        fov = 45.0f;
}
//注册鼠标滚轮的回调函数
glfwSetScrollCallback(window, scroll_callback);
```

欧拉角摄像机会遭遇万向死锁的问题，可以通过使用四元数摄像机来解决。