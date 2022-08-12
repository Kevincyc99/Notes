**Ex1.1**

<font color=blue>Compile and execute the code</font>

```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string user_name;
    cout << "Please enter your first name: ";
    cin >> user_name;
    cout << '\n'
         << "Hello, " 
         << user_name 
         << "... and goodbye!\n";

    return 0;
}
```

**Ex1.2**	

<font color=blue>Comment out the string header file:</font>

```c++
// #include <string>
```

<font color=blue>What happens?</font>

```
运行正常 
根据编译器不同有不同的反应，有些编译器中，iostream间接包含string类：
<iostream>需要用到std::ios_base类型，std::ios_base有个成员类叫failure，std::ios_base::failure有个构造函数接受std::string
```

<font color=blue>Comment out the namespace:</font>

```c++
//using namespace std;
```

<font color = blue>What happens?</font>

```
Error: 'string' was not declared in this scope
Error: identifier "string" is undefined
Error: 'cout' was not declared in this scope
Error: identifier "cout" is undefined
Error: 'cin' was not declared in this scope
Error: identifier "cin" is undefined
Error: 'user_name' was not declared in this scope
```

**Ex1.3**

<font color = blue>Change the name of main() to my_main(). What happens?</font>

```
error: ld returned 1 exit status
```

**Ex1.4**

<font color = blue>Try to extend the program: (1) Ask the user to enter both a first and last name and (2) modify the output to write out both names.</font>

```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string first_name, last_name;
    cout << "Please enter your first name: ";
    cin >> first_name;
    cout << "Hi, " << first_name << ", please enter your last name: ";
    cin >> last_name;
    cout << '\n'
         << "Hello, " 
         << first_name << ' ' << last_name 
         << "... and goodbye!\n";

    return 0;
}
```

**Ex1.5**

<font color = blue>Write a program to ask the user his or her name. Read the response. Confirm that the input is at least two
characters in length. If the name seems valid, respond to the user. Provide two implementations: one using
a C-style character string, and the other using a string class object.</font>

```c++
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    //C-style
    char ch;
    int len = 0;
    char name[256] = {0};
    printf("What's your name\n");
    while((ch = getchar()) != '\n')
    {
        name[len] = ch;
        len++;
        
    }
    if (len > 2)
    {
        printf("Hello, ");
        for(int i = 0; i < len; i++)
        {
            printf("%c", name[i]);
        }
        printf(".\n");
    }
    else
    {
        printf("It is an invalid name.\n");
    }

    //string style
    string user_name;
    cout << "What's your name?\n";
    while(cin >> user_name)
    {
        if (user_name.length() > 2)
        {
            cout << "Hello, " << user_name << "." << endl;
            break;
        }
        else
        {
            cout <<"Please be sure that your name is longer than two characters." << endl
                 << "What's yout name?\n";
        }
    }
    return 0;
}
```

**Ex1.6**

<font color = blue>Write a program to read in a sequence of integers from standard input. Place the values, in turn, in a built-
in array and a vector. Iterate over the containers to sum the values. Display the sum and average of the
entered values to standard output.</font>

```c++
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    int nums[100];
    int cnt = 0;
    int num;
    vector<int> n;
    int arrSum = 0, arrAvg = 0, vecSum = 0, vecAvg = 0;
    while(cin >> num)
    {
        n.emplace_back(num);
        nums[cnt] = num;
        cnt++;
    }
    for(int i = 0; i < cnt; i++)
    {
        arrSum += nums[i];
    }
    arrAvg = arrSum / cnt;
    cout << "Sum: " << arrSum << " Avg: " << arrAvg;
    cout << endl;
    for(int i = 0; i < n.size(); i++)
    {
        vecSum += n[i];
    }
    vecAvg = vecSum / n.size();
    cout << "Sum: " << vecSum << " Avg: " << vecAvg;
    cout << endl;
    return 0;
}
```

**Ex1.7**

<font color = blue>Using your favorite editor, type two or more lines of text into a file. Write a program to open the file,
reading each word into a vector\<string> object. Iterate over the vector, displaying it to cout . That
done, sort the words using the sort() generic algorithm,</font>

```c++
#include <algorithm>
sort(container.begin(), container.end());
```

<font color = blue>Then print the sorted words to an output file.</font>

```C++
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
#include <fstream>
using namespace std;

int main()
{
    ifstream infile("ex7.txt"); //手写一个txt
    ofstream outfile("ex7_sort.txt");
    if (!infile || !outfile)
    {
        cerr << "Unable to open the file." << endl;
    }
    else
    {
        string word;
        vector<string> words;
        while(infile >> word)
        {
            words.emplace_back(word);
        }
        cout << "Unsorted text: " << endl;
        for(auto w: words)
            cout << w << ' ';
        cout << endl;
        sort(words.begin(), words.end());
        for(auto w: words)
            outfile << w << ' ';
    }
    return 0;
}
```

**Ex1.8**

<font color = blue>The switch statement of Section 1.4 displays a different consolation message based on the number of
wrong guesses. Replace this with an array of four string messages that can be indexed based on the number
of wrong guesses.</font>

```c++
#include <iostream>
using namespace std;

int main()
{
    int num;
    string response[4] = {"Oops, nice guess but not quite it.\n", "Sorry, wrong a second time!\n", 
    "Ah, this is harder than it looks, isn't it?\n", "It must be getting pretty frustrating by now!!!\n"};
    cout << "How many times did you failed?\n";
    cin >> num;
    cout << response[num - 1] << endl;
    return 0;
}
```

