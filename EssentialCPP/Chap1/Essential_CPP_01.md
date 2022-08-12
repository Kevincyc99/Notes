# Essential C++

## Preface

*C++ Primer*

P205 附录A

P255 附录B

## Chapter 1. Basic C++ Programming

### 1.1 How to Write a C++ Program

**main() returns 0 to indicate success.** A nonzero return value indicates something went wrong.

A **class** is a user-defined data type. <u>The class mechanism allows us to add layers of type abstraction to our programs.</u>

The definition of a class is typically two parts: a ***header file*** that <u>provides a declaration of the operationis supported by the class</u> and a ***program text file*** that <u>contains the implemetation of those operations.</u>

string literal: 字符串常量

**Namespace** is a way to avoid *name clash*(命名冲突).

### 1.2 Defining and Initializing a Data Object

Two initialization syntaxes:

1) The use of the assignment operator (=) for initialization is inherited from the C language.

2) **Constructor syntax**(构造函数语法)

构造函数语法是为了解决类对象的多值初始化问题

The alternative constructor initialization syntax was introduced to handle <u>multiple</u>
<u>value initialization.</u>

complex\<double> purei(0, 7);

尖括号表示complex是一个模板类(**template class**)

A template class allows us to <u>define a class without having to specify the data type of one or all of its members.</u>

The template class mechanism allows the programmer to <u>defer deciding on the data type to use for a template class.</u>(模板类可以推迟数据类型的决定)

构造函数语法使得内置数据类型(built-in data type)和类数据类型(class data type)的初始化语法得到统一

转义字符(**escape sequence**):	以反斜杠开头

'\n'	newline

'\t'	tab

'\0'	null

'\\''	single quote

'\\"'	double quote

'\\\\'	backslash

A **const** object cannot be modified from its initial value. Any attempt to assign a value to a const object results in a compile-time error.

### 1.3 Writing Expressions

##### Operator Precedence

logical Not > arithmetic (*, /, %) > arithmetic (+, -) > relational (<, >, <=, >=) 

\> relational (==, !=) > logical AND > logical OR > assignment

### 1.4 Writing Conditional and Loop Statements

##### Conditional Statements

Switch只能判断整数表达式(short、char、byte、enum都可以隐含转换为int)

##### Loop Statements

### 1.5 How to Use Arrays and Vectors

The vector class is a template, so we indicate the type of its element in brackets following the name of the class.

vector\<int> pell_seq;

### 1.6 Pointers Allow for Flexibility

rand() and srand()	in \<cstdlib>

srand() seeds the generator with its parameter. Each call of rand() returns an integer value in a range between 0 and the maximum integer value an int can represent.

fibonacci.empty() and pv->empty()

The dot connecting fibonacci and empty() is called a **member selection operator**. It is used to <u>select class operations through an object of the class</u>. <u>To select a class operation through a pointer</u>, we use the **arrow member selection operator (->)**

### 1.7 Writing and Reading Files

To read and write to a file, we must include the fstream header file:

```c++
#include <fstream>
```

<u>To open a file for output</u>, we define an **ofstream**  class object (an output file stream), passing it the name of the file to open:

```C++
ofstream outfile("seq_data.txt");
```

When we declare outfile, if it doesn't exist, the file is created and opened for output, if it
does exist, it is opened for output and <u>any existing data in the file is discarded.</u>

If we wish to add to rather than replace the data within an existing file, we must open the file in append mode.

```C++
ofstream outfile("seq_data.txt", ios_base::app);

if (! outfile)
	// open failed for some reason ...
	cerr << "Oops! Unable to save session data!\n";
else
{
	// ok: outfile is open, let's write the data
	outfile << usr_name << ' '
			<< num_tries << ' '
			<< num_right << endl;
}
```

The difference between **cout** and **cerr** is that <u>cerr's output is not buffered.</u>

<u>To open a file for input</u>, we define an **ifstream** class object (an input file stream), passing it the name of a file.

```c++
ifstream infile("seq_data.txt");
string name;
int nt, nc;
infile >> name;
infile >> nt >> nc;
```

If we wish to <u>both read from and write to the same file</u>, we define an **fstream** class object. To open it in append mode, we must provide a second value of the form ios_base::in|ios_base::app.

```c++
fstream iofile("seq_data.txt",ios_base::in|ios_base::app);
if (! iofile)
	// open failed for some reason ... darn!
else
{
	// reposition to front of file to begin reading
iofile.seekg(0);
	// ok: everything else is the same ...
}
```