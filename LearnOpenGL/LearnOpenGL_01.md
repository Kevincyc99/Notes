# LearnOpenGL

### 1	Introduction

## Part I	Getting started

### 2	OpenGL

OpenGL本身并不是一个API(Application Programming Interface)，它仅仅是由Khronos组织制定并维护的一个**规范(specification)**。它严格规定了每个函数如何执行以及它们的输出值，而实现由OpenGL库的开发者编写。OpenGL库的开发者通常是显卡生产商。

#### Core-profile vs Immediate mode

立即渲染模式(Immediate mode)在OpenGL3.2中被废弃，改用核心模式(Core-profile mode)

从OpenGL3.3开始都是现代OpenGL版本，越新的版本，需要越新的显卡支持。

#### Extensions

#### State Machine

通过OpenGL context 改变OpenGL的状态，通过state-changing函数改变context，通过state-using函数根据当前OpenGL状态执行一些操作。

#### Objects

OpenGL库是用C语言编写的，Object是OpenGL开发时引入的一个抽象层，它代表OpenGL状态的一个子集。

### 3	Creating a window

#### GLFW

GLFW是一个专门针对OpenGL的C语言库。它允许用户创建OpenGL context，定义窗口参数以及处理用户输入。

#### GLAD

GLAD是一个简化函数使用的库

### 4	Hello Window

```c++
#include <glad/glad.h>
#include <GLFW/glfw3.h>
```

GLAD的头文件中包含了正确的OpenGL头文件，所以要在其他依赖于OpenGL的头文件之前包含GLAD

```c++
int main()
{
	glfwInit();	//初始化GLFW
    //glfwWindowHint函数，第一个参数代表选项的名称，可以从GLFW_开头的枚举值中选择，
    //第二个参数接收一个整型，用来设置这个选项的值
    //设置主要版本和次要版本——3.3
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    //设置为核心模式
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
    
    reutrn 0;
}
```

```c++
//创建一个窗口对象，该窗口存放了所有和窗口有关的数据，并会被GLFW的其他函数频繁地调用
//glfwCreateWindow函数的前两个参数代表窗口的宽度和高度，第三个参数为窗口名称，（暂时忽略后两个参数）
GLFWwindow* window = glfwCreateWindow(800, 600, "LearnOpenGL", NULL, NULL);
if (window == NULL)
{
    std::cout << "Failed to create GLFW window" << std::endl;
    glfwTerminate();
    return -1;
}
//设置该窗口的context为当前context
glfwMakeContextCurrent(window);
```

```c++
//给GLAD传入用来加载系统相关的OpenGL函数指针的函数，GLFW给出glfwGetProcAddress，它根据编译的系统定义了正确的函数
if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
{
    std::cout << "Failed to initialize GLAD" << std::endl;
    return -1;
}
```

```c++
//在渲染之前必须告诉OpenGL渲染窗口的尺寸大小，即视口(Viewport)
//我们可以通过glViewport函数来设置窗口的维度
glViewport(0, 0, 800, 600);
//前两个参数控制窗口左下角的位置，后两个参数分别控制渲染窗口的宽度和高度(像素)
//OpenGL的视口可以比GLFW的窗口小

//glViewport实际上把OpenGL中的2D坐标转换成屏幕坐标
//OpenGL的坐标范围是(-1, 1)， 最终映射到屏幕的宽度以及高度

//当用户改变窗口大小时，视口也应该被调整。我们可以对窗口注册一个回调函数(Callback Function)
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
    glViewport(0, 0, width, height);
}
//我们还需要注册该函数，告诉GLFW当窗口调整大小时回调该函数
glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);
```

```c++
//为了让我们的窗口持续显示，需要添加一个渲染循环
while(!glfwWindowShouldClose(window))
{
    glfwSwapBuffers(window);
    glfwPollEvents();
}
//glfwWindowShouldClose函数会检查GLFW是否被要求退出
//glfwPollEvents函数检查是否触发什么事件、更新窗口状态，并调用相应的回调函数
//glfwSwapBuffers函数用来交换颜色缓冲(双缓冲)
```

**双缓冲(Double buffer)**

前缓冲保存着最终输出的图像，显示在屏幕上；后缓冲进行所有的渲染指令，当所有指令执行完毕，交换(swap)前缓冲和后缓冲，这样图像就立即显示出来。

```c++
//渲染循环结束后释放之前分配的资源
glfwTerminate();
return 0;
```

```c++
//该函数需要一个窗口和一个按键作为输入，并返回这个按键是否正在被按下
void processInput(GLFWwindow* window)
{
    if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)	//检查esc建是否被按下
        glfwSetWindowShouldClose(window, true);				//若按下则关闭窗口
}
```

```c++
//在渲染循环中调用processInput函数，通过检测esc键来控制窗口的关闭
while(!glfwWindowShouldClose(window))
{
    //输入
    processInput(window);
    
    //渲染指令...
    
    //检查并调用事件，并交换缓冲
    glfwPollEvents();
    glfwSwapBuffers(window);
}
```

```c++
//glClear函数可以用来清空屏幕的缓冲，它接收一个缓冲位(Buffer Bit)来指定要清空的缓冲
//可能的缓冲位有GL_COLOR_BUFFER_BIT、GL_DEPTH_BUFFER_BIT、GL_STENCIL_BUFFER_BIT
//glClearCOlor来设置清空屏幕所用的颜色
glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
glClear(GL_COLOR_BUFFER_BIT);

//glClearColor是state-setting函数，glClear是state-using函数
```

