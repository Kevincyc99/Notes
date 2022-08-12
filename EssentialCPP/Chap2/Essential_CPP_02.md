# Essential C++

## Chapter2. Procedural Programming

Three benefits of using independent functions:

​	1.programs would be simpler to understand.

​	2.These functions can be used across multiple programs

​	3.Easy to distribute the work across multiple programmers.

### 2.1 How to write a Function

Four parts of a function:

​	1.The return type of the function

​	2.The name of the function

​	3.The parameter list of the function

​	4.The body of the function

**A function must be declared(声明) before it can be called.**

A **function declaration** must specifies the return type, the name, the parameter list but not the function body. This is called the <u>function prototype</u>. (The name of parameters are not necessary in this part)

A **function definition** consists of the function prototype and the function body.

### 2.2 Invoking a Function

When we invoke a function, a special area of memory is set up on what is called the **program**
**stack**(程序堆栈).

<u>All manipulation of a reference acts on the object the reference refers to.</u>

<u>The period of time for which memory is allocated for an object</u> is called its **storage duration** or
**extent**.

<u>The region of the program over which an object is active</u> is called its **scope**.

动态内存分配

程序员自己通过new和delete分配和释放的内存叫做堆内存(heap memory)

### 2.3 Providing Default Parameter Values