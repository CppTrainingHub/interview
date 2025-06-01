
## C++ 面试真题（1-10）

## 1. C 和 C++ 之间的区别是什么？

C 和 C++ 是两种广泛使用的编程语言，二者既有联系也存在诸多区别.

#### 语言范式

C 语言：是一种面向过程的编程语言。它主要关注解决问题的步骤和过程，通过函数将不同的任务模块化。

程序的执行流程是按照函数的调用顺序依次进行。

比如，我们编写一个计算两个数之和的程序，会定义一个求和函数，然后在主函数中调用该函数完成计算。

```c
include <stdio.h>

// 定义求和函数
int add(int a, int b) {
    return a + b;
}

int main() {
    int num1 = 3, num2 = 5;
    int result = add(num1, num2);
    printf("两数之和为: %d\n", result);
    return 0;
}
```

C++ 语言：是一种多范式编程语言，支持面向过程、面向对象和泛型编程。面向对象编程是 C++ 的重要特性，它将数据和操作数据的方法封装在类中，通过类的实例（对象）来实现程序的功能。

泛型编程则允许编写与数据类型无关的代码，提高代码的复用性。例如，使用 C++ 的类来实现上述求和功能：

```cpp
#include <iostream>

// 定义一个求和类
class Adder {
public:
    int add(int a, int b) {
        return a + b;
    }
};

int main() {
    Adder adder;
    int num1 = 3, num2 = 5;
    int result = adder.add(num1, num2);
    std::cout << "两数之和为: " << result << std::endl;
    return 0;
}
```

#### 数据抽象和封装

C 语言：没有内置的机制来实现数据抽象和封装。虽然可以使用结构体来组织数据，但结构体中的成员通常是公开的，外部代码可以直接访问和修改这些成员，这可能导致数据的不一致性和安全性问题。

```c
#include <stdio.h>

// 定义一个结构体
struct Point {
    int x;
    int y;
};

int main() {
    struct Point p;
    p.x = 10;
    p.y = 20;
    printf("点的坐标为: (%d, %d)\n", p.x, p.y);
    return 0;
}
```

C++ 语言：通过类和访问修饰符（如 `private`、`protected`、`public`）实现了数据抽象和封装。类中的私有成员只能通过类的公有成员函数来访问和修改，从而隐藏了数据的实现细节。

```cpp
#include <iostream>

// 定义一个类
class Point {
private:
    int x;
    int y;
public:
    void setCoordinates(int a, int b) {
        x = a;
        y = b;
    }
    void printCoordinates() {
        std::cout << "点的坐标为: (" << x << ", " << y << ")" << std::endl;
    }
};

int main() {
    Point p;
    p.setCoordinates(10, 20);
    p.printCoordinates();
    return 0;
}
```

#### 继承和多态

- C 语言：不支持继承和多态的概念。继承是指一个类可以继承另一个类的属性和方法，多态是指不同的对象可以对同一消息做出不同的响应。
- C++ 语言：支持继承和多态。通过继承，子类可以复用父类的代码，并且可以添加自己的特性。**多态可以通过虚函数和指针或引用实现**，使得程序在运行时能够根据对象的实际类型来调用相应的函数。

```cpp
#include <iostream>

// 定义基类
class Shape {
public:
    virtual void draw() {
        std::cout << "绘制形状" << std::endl;
    }
};

// 定义派生类
class Circle : public Shape {
public:
    void draw() override {
        std::cout << "绘制圆形" << std::endl;
    }
};

int main() {
    Shape* shape = new Circle();
    shape->draw();
    delete shape;
    return 0;
}
```

#### 标准库

- C 语言：标准库主要提供了一些基本的输入输出、字符串处理、数学运算等函数，如 `printf`、`scanf`、`strlen`、`sqrt` 等。这些函数以头文件的形式提供，使用时需要包含相应的头文件。

```c
#include <stdio.h>
#include <math.h>

int main() {
    double num = 16.0;
    double result = sqrt(num);
    printf("16的平方根是: %f\n", result);
    return 0;
}
```

- C++ 语言：除了兼容 C 语言的标准库外，还拥有自己的标准模板库（STL）。STL 提供了丰富的容器（如 `vector`、`list`、`map` 等）、算法（如 `sort`、`find` 等）和迭代器，大大提高了开发效率。

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {3, 1, 4, 1, 5, 9};
    std::sort(numbers.begin(), numbers.end());
    for (int num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    return 0;
}
```

#### 错误处理

- C 语言：通常使用返回错误码的方式来处理错误。函数在执行过程中如果出现错误，会返回一个特定的错误码，调用者需要根据这个错误码来判断函数的执行情况。这种方式比较繁琐，容易出错。

```c
#include <stdio.h>
#include <errno.h>

int divide(int a, int b, int* result) {
    if (b == 0) {
        errno = EDOM;
        return -1;
    }
    *result = a / b;
    return 0;
}

int main() {
    int num1 = 10, num2 = 0;
    int result;
    if (divide(num1, num2, &result) == -1) {
        perror("除法运算出错");
    } else {
        printf("结果为: %d\n", result);
    }
    return 0;
}
```

- C++ 语言：引入了异常处理机制，使用 `try`、`catch` 和 `throw` 关键字来处理异常。当程序出现异常时，可以抛出一个异常对象，调用者可以在 `catch` 块中捕获并处理这个异常，使错误处理更加清晰和灵活。（**但，平时编程中，还是建议使用错误码方式**）

```cpp
#include <iostream>

int divide(int a, int b) {
    if (b == 0) {
        throw std::runtime_error("除数不能为零");
    }
    return a / b;
}

int main() {
    int num1 = 10, num2 = 0;
    try {
        int result = divide(num1, num2);
        std::cout << "结果为: " << result << std::endl;
    } catch (const std::runtime_error& e) {
        std::cerr << "错误: " << e.what() << std::endl;
    }
    return 0;
}
```

#### C 与 C++ 核心区别对比

<table>
    <tr>
    <td>对比维度<br/></td><td>C 语言<br/></td><td>C++ 语言<br/></td></tr>
    <tr>
    <td> 语言范式 <br/></td><td>纯面向过程<br/></td><td>多范式（面向过程+面向对象+泛型）<br/></td></tr>
    <tr>
    <td> 代码组织 <br/></td><td>函数为核心<br/></td><td>类（Class）为核心<br/></td></tr>
    <tr>
    <td> 数据安全 <br/></td><td>结构体成员默认公开<br/></td><td>支持 `private`/`protected` 封装<br/></td></tr>
    <tr>
    <td> 继承/多态 <br/></td><td>❌ 不支持<br/></td><td>✅ 支持（虚函数、`override`）<br/></td></tr>
    <tr>
    <td> 内存管理 <br/></td><td>`malloc`/`free`<br/></td><td>`new`/`delete` + 智能指针<br/></td></tr>
    <tr>
    <td> 标准库 <br/></td><td>基础库（`stdio.h`, `math.h`）<br/></td><td>STL（容器、算法、迭代器）<br/></td></tr>
    <tr>
    <td> 错误处理 <br/></td><td>返回错误码（如 `-1`）<br/></td><td>异常机制（`try`/`catch`）<br/></td></tr>
    <tr>
    <td> 函数特性 <br/></td><td>无重载、无默认参数<br/></td><td>函数重载、默认参数、Lambda<br/></td></tr>
    <tr>
    <td> 编译方式 <br/></td><td>直接编译<br/></td><td>支持模板元编程<br/></td></tr>
    <tr>
    <td> 典型用途 <br/></td><td>嵌入式、操作系统底层<br/></td><td>游戏、高性能应用、大型软件<br/></td></tr>
    </table>



## 2. C++ 中有哪些不同的数据类型？

#### 基本数据类型

- **整数类型**：用于存储整数数值，有不同的长度和符号属性。

  - `int`：最常用的整数类型，一般占 32 位，可存储范围约为 -21 亿到 21 亿，适合存储日常使用的整数。
  - `short`：通常为 16 位，存储范围较小，适用于存储数值范围明确且较小的整数，能节省内存。
  - `long`：一般为 32 位或 64 位，存储范围比 `int` 更大，用于存储较大的整数。
  - `long long`：通常占 64 位，可存储非常大的整数，适用于处理大数值计算。
  - 这些类型都有对应的无符号版本（如 `unsigned int`），只能存储非负整数，存储的正数范围比有符号类型更大。
- **浮点类型**：用于存储小数。

  - `float`：单精度浮点型，占 32 位，精度相对较低，适用于对精度要求不高的计算场景。
  - `double`：双精度浮点型，占 64 位，精度比 `float` 高，是处理浮点运算时更常用的类型。
  - `long double`：扩展精度浮点型，精度更高，占用位数因编译器而异，用于对精度要求极高的科学计算等场景。
- **字符类型**：用于存储单个字符。

  - `char`：通常占 1 个字节，可表示 ASCII 字符集里的字符，是最常用的字符类型。
  - `wchar_t`：宽字符类型，用于表示宽字符集，如 Unicode 字符，可处理多种语言的字符。
  - `char16_t` 和 `char32_t`：分别用于表示 16 位和 32 位的字符，常用于处理 UTF - 16 和 UTF - 32 编码的字符。
- **布尔类型**：只有两个取值，`true`（真）和 `false`（假），常用于逻辑判断。

  - `bool`：占一个字节，让代码中的逻辑表达更清晰。

#### 派生数据类型

- **数组**：由相同类型元素组成的集合，元素存储在连续的内存位置。

  - 例如：`int arr[5];` 定义了一个包含 5 个整数的数组，可通过下标访问每个元素。
- **指针**：存储变量的内存地址，通过指针可以间接访问和操作内存中的数据。

  - 例如：`int* ptr;` 定义了一个指向整数的指针，可用于动态内存分配和操作。
- **引用**：变量的别名，对引用的操作等同于对被引用变量的操作。

  - 例如：`int num = 10; int& ref = num;` 这里 `ref` 就是 `num` 的引用，改变 `ref` 的值，`num` 的值也会改变。
- **函数**：实现特定功能的代码块，可通过参数传递数据，执行相应操作并返回结果。

  - 例如：`int add(int a, int b) { return a + b; }` 定义了一个返回两个整数之和的函数。

#### 用户自定义数据类型

- **结构体**：可以包含不同类型的数据成员，将相关的数据组织在一起。
  - 例如：

```cpp
struct Person {
    std::string name;
    int age;
};
```

```
这里定义了一个`Person`结构体，包含姓名和年龄两个成员。
```

- **类**：面向对象编程的核心，类中可包含数据成员和成员函数，用于实现数据的封装和操作。
  - 例如：

```cpp
class Rectangle {
private:
    int length;
    int width;
public:
    Rectangle(int l, int w) : length(l), width(w) {}
    int area() { return length * width; }
};
```

```
定义了一个`Rectangle`类，有长和宽两个私有成员，以及计算面积的成员函数。
```

- **枚举**：定义了一组命名的整数常量，使代码更具可读性。
  - 例如：

```cpp
enum Color { RED, GREEN, BLUE };
```

```
定义了一个`Color`枚举，包含红、绿、蓝三种颜色的常量。 
```

## 3. C++ 中的引用是什么？

在 C++ 中，引用是为另一个变量创建别名的另一种方式。

引用作为变量的同义词，允许你直接访问变量而不需要任何额外的语法。

它们必须在创建时初始化，并且之后不能改变以引用另一个变量。这个特性使得在函数中操作变量时避免了复制大对象的开销。

引用变量前面有一个 `&` 符号。

语法：

```
int tmp = 10; 

// 引用变量
int& ref = tmp;
```

## 4. C++ 中值传递和引用传递的区别？

#### 回答重点

1）值传递：在函数调用时，会触发一次参数的拷贝动作，所以对参数的修改不会影响原始的值。如果是较大的对象，复制整个对象，效率较低。

2）引用传递：函数调用时，函数接收的就是参数的引用，不会触发参数的拷贝动作，效率较高，但对参数的修改会直接作用于原始的值。

#### 扩展知识

看两种传递方式的示例代码：

值传递

```cpp
void modify_value(int value) {
    value = 100; // 只会修改函数内部的副本，不会影响原始变量
}

int main() {
    int a = 20;
    modify_value(a);
    std::cout << a; // 20，没变
    return 0;
}
```

引用传递

```cpp
void modify_value(int& value) {
    value = 100; // 修改引用指向的原始变量
}

int main() {
    int a = 20;
    modify_value(a);
    std::cout << a; // 100，因为是引用传递，所以这里已经改为了100
    return 0;
}
```

###### 深入理解

######## 什么场景下使用引用传递？

- 避免不必要的数据拷贝：对于比较大的对象参数（比如 std::vector、std::string、std::list），因为拷贝会导致大量的内存和时间开销。而引用传递可以避免这些开销。
- 允许函数修改实参原始值：有时候，我们就是希望函数能够直接修改传入的变量值，这时使用引用传递很合理。

######## 什么场景下使用值传递？

- 小型数据结构：对于 int、char、double、float 这种基础数据类型，可以直接简单的使用值传递。
- 不希望函数修改实参：有时候，我们需要修改变量数据，但是又不希望修改原始值，可以考虑使用值传递。

###### 值定义和引用定义

栈和链表，是在栈上分配内存还是堆上分配内存呢？两种情况都有可能，如果我们是在函数内直接创建的话，则是栈分配内存，如果是通过指针创建的话，则是在堆上分配内存，并且需要手动删除

栈上分配

```cpp
#include<iostream>
int main() {
    int arr[5] = {1, 2, 3, 4, 5}; // 在栈上分配一个包含 5 个整数的数组
    for (int i = 0; i < 5; ++i) {
        std::cout<< arr[i] << " ";
    }
    return 0;
}
```

堆上分配

```cpp
#include<iostream>
int main() {
    int* arr = new int[5]{1, 2, 3, 4, 5}; // 在堆上分配一个包含 5 个整数的数组
    for (int i = 0; i < 5; ++i) {
        std::cout<< arr[i] << " ";
    }
    delete[] arr; // 释放堆上分配的数组内存
    return 0;
}
```

当然链表也可以这样，无论是结构体还是类，都有两种定义方式，值定义和引用定义，值定义则是栈分配内存，引用定义则是堆分配内存。

```cpp
#include<iostream>
struct MyStruct {
    int x;
};
class MyClass {
public:
    int x;
};
int main() {
    // 结构体的值定义
    MyStruct structVar1{42};
    // 结构体的引用定义
    MyStruct* structVar2 = new MyStruct{42};
    // 类的值定义
    MyClass classVar1{42};
    // 类的引用定义
    MyClass* classVar2 = new MyClass{42};
    // 释放堆上分配的内存
    delete structVar2;
    delete classVar2;
    return 0;
}
```

#### C++ 值传递 vs 引用传递对比

<table>
    <tr>
    <td>对比维度<br/></td><td>值传递<br/></td><td>引用传递<br/></td></tr>
    <tr>
    <td> 定义 <br/></td><td>传递参数的副本<br/></td><td>传递参数的别名（原始变量的引用）<br/></td></tr>
    <tr>
    <td> 语法 <br/></td><td>`void func(Type param)`<br/></td><td>`void func(Type& param)`<br/></td></tr>
    <tr>
    <td> 内存开销 <br/></td><td>复制整个对象（可能高开销）<br/></td><td>仅传递地址（固定大小，低开销）<br/></td></tr>
    <tr>
    <td> 修改影响 <br/></td><td>不影响原始值<br/></td><td>直接修改原始值<br/></td></tr>
    <tr>
    <td> 性能 <br/></td><td>低效（大型对象拷贝耗时）<br/></td><td>高效（无拷贝操作）<br/></td></tr>
    <tr>
    <td> 安全性 <br/></td><td>高（隔离原始数据）<br/></td><td>低（可能意外修改外部变量）<br/></td></tr>
    </table>



## 5. 🌟C++ 中 static 的作用？什么场景下用到 static？

谈到 C++ 的 `static`，可以重点回答以下几个方面：

1）修饰局部变量：当 `static` 用于修饰局部变量时，这个变量的存储位置会在程序执行期间保持不变，且只在程序执行到该变量的声明处时初始化一次。

即使函数被多次调用，`static` 局部变量也只在第一次调用时初始化，之后的调用将不会重新初始化它。

2）修饰全局变量或函数：当 `static` 用于修饰全局变量或函数时，限制了这些变量或函数的作用域，它们只能在定义它们的文件内部访问。有助于避免在不同文件之间的命名冲突。

3）修饰类的成员变量或函数：在类内部，`static` 成员变量或函数属于类本身，而不是类的任何特定对象。这意味着所有对象共享同一个 `static` 成员变量，无需每个对象都存储一份拷贝。`static` 成员函数可以在没有类实例的情况下调用。

###### static 的用法

1. `static` 局部变量

```cpp
#include <iostream>
using namespace std;

void func() {
    static int count = 0; // 只在第一次调用func时初始化
    cout << "Count is: " << count << endl;
    count++;
}

int main() {
    for(int i = 0; i < 5; i++) {
        func(); // 每次调用都会显示增加的count值
    }
    return 0;
}
```

2. `static` 全局变量或函数

```cpp
// file1.cpp
static int count = 10; // count变量只能在file1.cpp中访问

static void func() { // func函数只能在file1.cpp中访问
    cout << "Function in file1" << endl;
}

// file2.cpp
extern int count; // 这里会导致编译错误，因为count是static的，不能在file2.cpp中访问

void anotherFunc() {
    func(); // 这里也会导致编译错误，因为func是static的，不能在file2.cpp中访问
}
```

3. `static` 类的成员变量或函数

```cpp
#include <iostream>
using namespace std;

class MyClass {
public:
    static int staticValue; // 静态成员变量
    static void staticFunction() { // 静态成员函数
        cout << "Static function called" << endl;
    }
};

int MyClass::staticValue = 10; // 静态成员变量的初始化

int main() {
    MyClass::staticFunction(); // 调用静态成员函数
    cout << MyClass::staticValue << endl; // 访问静态成员变量
    return 0;
}
```

###### 使用场景总结

- `static` 局部变量：当你需要在函数的多次调用之间保持某个变量的值时。
- `static` 全局变量或函数：当你想要限制变量或函数的作用域，防止它们在其他文件中被访问时。
- `static` 类的成员变量或函数：当你想要类的所有对象共享某个变量或函数时，或者当你想要在没有类实例的情况下访问某个函数时。

<table>
    <tr>
    <td>作用范围<br/></td><td>行为特性<br/></td><td>线程安全<br/></td></tr>
    <tr>
    <td> 局部变量 <br/></td><td>生命周期延长（程序全程），作用域不变<br/></td><td>C++11后初始化线程安全，但多线程访问需同步<br/></td></tr>
    <tr>
    <td> 全局变量/函数 <br/></td><td>限制作用域为当前文件（内部链接性）<br/></td><td>初始化线程安全（main前完成），多线程访问需同步<br/></td></tr>
    <tr>
    <td> 类成员 <br/></td><td>属于类而非对象，所有实例共享<br>可通过类名直接访问<br/></td><td>初始化线程安全（首次访问时），多线程访问需同步<br/></td></tr>
    </table>



###### static 线程安全吗

`static` 局部变量在 C++11 及之后的初始化是线程安全的，在 C++11 之前非线程安全，需要手动加锁。

`static` 的全局变量在程序启动前（`main` 之前）完成的，默认就是线程安全的。

而无论初始化是否安全，多线程访问 `static` 变量都不是线程安全的，都需要显式的处理同步问题。

## 6. 🌟C++ 中 const 的作用？谈谈你对 const 的理解？

#### 回答重点

`const` 最主要的作用就是声明一个变量为常量，即这个变量的值在初始化之后就不能被修改。

但 `const` 不仅可以用作普通常量，还可以用于指针、引用、成员函数、成员变量等。

具体作用如下：

1）定义普通常量：当修饰基本数据类型的变量时，表示常量含义，对应的值不能被修改。

```c
const int MAX_SIZE = 100; // MAX_SIZE是一个常量，其值不能被修改
```

2）修饰指针：这里分多种情况，比如指针本身是常量，指针指向的数据是常量，或者指针本身和其指向的数据都是常量。

3）修饰引用：`const` 修饰引用时，一般用作函数参数，表示函数不会修改传递的参数值。

```c
void func(const int& a) { // a是一个对常量的引用，不能通过a修改其值  
    // ...  
}
```

4）修饰类成员函数：`const` 修饰成员函数，表示函数不会修改类的任何成员变量，除非这些成员变量被声明为 `mutable`。

```c
class MyClass {  
public:  
    void myFunc() const { // myFunc是一个const成员函数，它不会修改类的任何成员变量  
        // ...  
    }  
};
```

5）修饰类成员变量：`const` 修饰成员变量，表示生命期内不可改动此值。

```c
class MyClass {  
public:  
    const int a = 5;
};
```

#### 扩展知识

`const` 修饰指针时，可以分多种情况：

1）指向常量的指针：指针指向的内容是常量，不能通过该指针修改其所指向的值。

```c
const intptr = &x; // ptr是一个指向常量的指针，不能通过ptr修改x的值
```

2）指针常量：指针本身是常量，指针的值（即指向的地址）不能被修改，但可以通过该指针修改其所指向的值（如果所指向的不是常量）。

```c
int* const ptr = &x; // ptr是一个常量，ptr的值（地址）不能被修改，但x的值可以被修改
```

3）指向常量的常量指针：指针本身是常量，且指针指向的内容也是常量。

```c
const int* const ptr = &x; // ptr的值和ptr指向的值都不能被修改
```

## 7. 解释 C++ 中的构造函数。

C++ 中的构造函数是一种特殊的成员函数，与类同名，无返回类型，用于在创建对象时，对对象进行初始化，有默认、带参、拷贝等多种类型，保证对象创建后处于有效状态。

## 8. C++ 中的析构函数是什么？

析构函数是 C++ 里的一种特殊成员函数，它和类同名，不过前面得加个波浪线 `~`，没有返回值，也不能有参数。

它的主要作用是在对象的生命周期结束时，做一些清理工作，像释放对象占用的动态内存、关闭打开的文件之类的。

当对象离开作用域或者用 `delete` 删除动态分配的对象时，析构函数就会自动被调用，保证资源能被正确释放，避免内存泄漏等问题。就好比你离开房间的时候，要把房间里的东西收拾好，把灯关掉一样。

## 9. 什么是虚析构函数？

虚析构函数就是在基类里用 `virtual` 关键字声明的析构函数。当用基类指针指向派生类对象，且通过该指针删除对象时，若基类析构函数不是虚拟的，那就只会调用基类析构函数，派生类析构函数不会被调用，可能造成资源泄漏；但如果基类析构函数是虚拟的，程序在运行时会先调用派生类析构函数，再调用基类析构函数，确保派生类对象能被完整地销毁。

## 10. 析构函数重载是否可能？如果可以，那么解释；如果不可以，那么为什么？

答案是不可以，我们不能重载析构函数。每个类中只能有一个析构函数。同样需要提到的是，析构函数既不接受参数，也没有可能帮助重载的参数。

## C++ 面试真题（11-20）

## 11. 🌟C++ 中都有哪些构造函数？

#### 回答重点

- 默认构造函数：这就是普通的构造函数，如果没有构建的话，则会默认帮你创建
- 参数构造函数：带有参数的构造函数，允许创建对象时传递参数，以便根据这些参数初始化对象
- 拷贝构造函数：拷贝构造函数用于通过已有对象初始化新对象，接收一个同类型对象的引用作为参数，可以理解为拷贝了一个情况相同的对象，但是分别位于不同的内存块，修改新对象的状态不会影响源对象，反之亦然。
- 委托构造函数：允许在一个构造函数内部调用另一个构造函数，从而减少代码重复，这通过在构造函数的初始化列表中使用 this 指针调用另一个构造函数来实现

#### 扩展知识

直接看代码，方便我们理解：

```cpp
#include<iostream>
class MyClass {
public:
    // 默认构造函数
    MyClass() {
        std::cout << "Default constructor called."<< std::endl;
    }
    // 参数化构造函数
    MyClass(int a) : x(a) {
        std::cout << "Parameterized constructor called."<< std::endl;
    }
    // 拷贝构造函数
    MyClass(const MyClass& other) {
        x = other.x;
        std::cout << "Copy constructor called."<< std::endl;
    }
    // 委托构造函数
    MyClass() : MyClass(0) {
        std::cout << "Delegating constructor called."<< std::endl;
    }
    MyClass(int a) : x(a) {
        std::cout << "Parameterized constructor called."<< std::endl;
    }
private:
    int x;
};
int main() {
    // 使用默认构造函数创建对象
    MyClass obj1;
    // 使用参数化构造函数创建对象
    MyClass obj2(42);
    // 使用拷贝构造函数创建对象
    MyClass obj3(obj2);
    // 使用委托构造函数创建对象
    MyClass obj4;
    return 0;
}
```

![image-20250511155439834](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/202505111554893.png)

## 12. 哪些操作允许在指针上进行？

在 C++ 里，指针允许进行下面这些操作：

赋值操作

可以把一个变量的地址赋给指针，或者让一个指针指向另一个指针所指向的地址。比如：

```cpp
int num = 10;
int* ptr = &num; // 把num的地址赋给ptr
int* anotherPtr = ptr; // anotherPtr指向ptr所指向的地址
```

解引用操作

使用解引用操作符 `*` 能访问指针所指向的变量的值。例如：

```cpp
int num = 10;
int* ptr = &num;
cout << *ptr; // 输出10
```

指针算术运算

- **指针加整数**：可以让指针向后移动若干个元素的位置。假设 `ptr` 是指向数组元素的指针，`ptr + n` 就表示向后移动 `n` 个元素。

```cpp
int arr[5] = {1, 2, 3, 4, 5};
int* ptr = arr;
int* newPtr = ptr + 2; // newPtr指向arr[2]
```

- **指针减整数**：和加整数相反，是让指针向前移动若干个元素的位置。
- **指针减指针**：两个指向同一个数组元素的指针相减，得到的是它们之间元素的个数。

比较操作

可以对指针进行比较，比如判断两个指针是否指向同一个地址，或者一个指针是否大于、小于另一个指针。

```cpp
int arr[5] = {1, 2, 3, 4, 5};
int* ptr1 = &arr[0];
int* ptr2 = &arr[2];
if (ptr1 < ptr2) {
    cout << "ptr1在ptr2前面";
}
```

######## 类型转换

能把指针从一种类型转换为另一种类型，但要注意这样做可能会带来风险，得谨慎使用。

```cpp
int num = 10;
int* intPtr = &num;
char* charPtr = reinterpret_cast<char*>(intPtr); // 强制类型转换
```

## 13. delete 运算符的目的是什么？

`delete` 运算符用于释放通过 `new` 运算符在堆上动态分配的内存，调用对象的析构函数完成资源清理，防止内存泄漏。

## 14. delete []与 delete 的区别是什么？

`delete` 用于释放单个由 `new` 分配的对象的内存并调用其析构函数，而 `delete []` 用于释放由 `new[]` 分配的数组对象的内存，会依次调用数组中每个对象的析构函数。

## 15. C++ 中 char*、const char*、char* const、const char* const 的区别？

#### 回答重点

一个小技巧，从后往前读：

1）`char*`：这是一个指向 `char` 类型数据的指针，指针以及它指向的数据都是可变的。可以改变指针的指向和指向的数据。

2）`const char*`：*指向 `const char`，这是一个指向 `const char` 类型数据的指针。指针本身是可变的，但指针指向的数据是不可变的。简单来说，可以改变指针的指向，但不能改变它指向的数据内容。

3）`char* const`：`const` 修饰 `char*`，这是一个指向 `char` 类型数据的常量指针。指向的数据是可变的，但指针本身是不可变的。也就是说，不能改变指针的指向，但能修改指向的数据。

4）`const char* const`：这是一个指向 `const char` 类型数据的常量指针。指针和指向的数据都是不可变的，也就是既不能改变指针的指向，也不能修改指针指向的数据。

#### 扩展知识

看一些代码示例。

1）`char*`:

```cpp
char data[] = "Hello";
char* p = data;
p[0] = 'h'; // 修改数据内容，是允许的
p = nullptr; // 改变指针指向，是允许的
```

2）`const char*`:

```cpp
const char* p = "Hello";
p[0] = 'h'; // 错误：不能修改数据内容
p = "World"; // 允许：改变指针指向
```

3）`char* const`:

```cpp
char data[] = "Hello";
char* const p = data;
p[0] = 'h'; // 允许：修改数据内容
p = nullptr; // 错误：不能改变指针指向
```

4）`const char* const`:

```cpp
const char* const p = "Hello";
p[0] = 'h'; // 错误：不能修改数据内容
p = "World"; // 错误：不能改变指针指向
```

除了这些类型修饰符，理解指针和引用在函数参数中的应用也很重要。

比如在接口设计中，通过使用 `const char*` 可以保证函数不会修改传入的字符串数据，从而提高代码的安全性。

合理使用 const 也有助于避免潜在的错误。例如，较为复杂的类和对象也可以通过 const 修饰，来确保在调用成员函数时不会意外修改对象的状态。

## 16. 🌟C++ 中数组和指针的区别？

详见：[数组不是指针，指针也不是数组](https://lb3fn675fh.feishu.cn/wiki/GzmDwzRNtiOmhzkHcbWcV8rKnHf?fromScene=spaceOverview)

#### 回答重点

主要的区别可以总结为以下几点：

1）内存分配：

- 数组：编译器会在栈上为数组的所有元素分配连续的内存空间。
- 指针：指针本身只占用一个内存单元（通常是 4 或 8 字节），它存储的是一个地址。初始化指针之后，可以通过动态内存分配（例如使用 `new` 或 `malloc`）来分配内存。

2）固定与动态大小：

- 数组：数组的大小在声明时就确定了，数组的大小需要是常量，无法在运行时改变。
- 指针：指针比较灵活，它指向的内存如果是堆内存，可以在运行时动态分配和释放内存，灵活性更好。

3）类型安全性：

- 数组：数组在声明时绑定了具体的类型，编译器在访问数组时可以进行类型检查。
- 指针：指针声明时也有类型，但指针所指向的内存地址可以重新赋值，容易引起类型不匹配的问题，可能导致运行时错误，特别是指针类型经常转换的场景，比如 `int*` 转 `void*` 等等。

4）运算操作：

- 数组：数组名可以看作是数组首元素的常量指针，但不能直接进行算术运算（如 `++` 或 `--`）。
- 指针：指针变量可以直接进行算术运算，比如递增、递减操作，从而访问不同的位置。

#### 扩展知识

我们可以从几个方面进一步探讨：

1）数组和指针的转换：

- 在表达式中，数组名会被自动转换为指向数组首元素的地址。例如，假设 `int arr[5]`，则 `arr` 会被转换为 `&arr[0]`。

2）动态数组：

- 动态数组的实现需要使用指针。例如，`int* arr = new int[5];` 这种方式在运行时分配的内存可以随意调整大小。

3）内存管理：

- 编程时需要注意内存泄漏问题。使用指针分配的内存（例如使用 `new`）需要显式地释放（例如使用 `delete` 或 `delete[]`），否则会导致内存泄漏。

4）多维数组与指针：

- 多维数组在内存中是按行优先顺序存储的，理解这一点有助于使用指针遍历多维数组。

5）高效代码：

- 在写高性能代码时，指针有时可以比数组更高效，因为指针的算术运算更加灵活。

## 17. 🌟C++ 中 sizeof 和 strlen 的区别？

#### 回答重点

两者的功能其实有很大区别：

1）`sizeof` 是一个编译时运算符，用于获取一个类型或者对象的大小（以字节为单位）。`sizeof` 在编译时计算结果，不涉及实际内容。

2）`strlen` 是一个库函数，用于计算 C 风格字符串的长度（不包括终止字符'\0'）。`strlen` 是在运行时计算结果的，因为它需要遍历字符串内容。

#### 扩展知识

1）`sizeof` 的应用：

- 用于计算基础类型的大小，比如 `sizeof(int)`。
- 用来计算结构体或类的内存占用，比如 `sizeof(MyClass)`。
- 对于静态数组，可以获取整个数组的内存大小，比如 `sizeof(arr)`，其中 `arr` 是一个静态数组。

注意，对于指针，`sizeof` 返回的是指针本身的大小，而不是指针所指向的内存区域的大小。例如：

```cpp
int *p = new int[10];
std::cout << sizeof(p); // 这会返回指针的大小，通常是4或8个字节，具体取决于系统架构。
```

2）`strlen` 的应用：

- 常用于计算 C 风格字符串的长度。注意，`strlen` 是不包括字符串末尾的终止符'\0'的。
- 对于一些特定字符数组，`strlen` 非常有用，比如 `char arr[] = "Hello";`，`strlen(arr)` 返回的是 5。

注意，`strlen` 不能用于未以 `\0` 结尾的字符数组，否则会导致未定义行为。例如：

```cpp
char arr[5] = {'H', 'e', 'l', 'l', 'o'};
std::cout << strlen(arr); // 这会导致未定义行为，因为没有终止符'\0'。
```

3）获取动态分配内存的大小：

对于使用 new 动态分配的内存，sizeof 不能直接获取分配的内存大小。在这种情况下，需要在分配内存时显式记录内存大小。

```cpp
int main() {
    int n = 10;
    int* arr = new int[n];
    // std::cout << "Size of dynamic array: "<< sizeof(arr) << " bytes"<< std::endl; // 错误：这将输出指针的大小，而不是数组的大小
    std::cout << "Size of dynamic array: " << n * sizeof(int) << " bytes"<< std::endl; // 正确：手动计算数组大小
    delete[] arr;
    return 0;
}
```

4）空类的大小

即使是一个空类（不包含任何数据成员和成员函数的类），sizeof 返回的值也至少为一个字节。这是为了确保在不同的编译器和平台上，空类的对象具有唯一标识。

```cpp
class EmptyClass {};
int main() {
    std::cout << "Size of EmptyClass: "<< sizeof(EmptyClass) << " bytes"<< std::endl;
    return 0;
}
```

总结，`sizeof` 和 `strlen` 有其各自的用途和使用场景，一个用于计算类型大小，一个用于计算字符串长度。

## 18. 🌟C++ 中四种类型转换的使用场景？

#### 回答重点

在 C++ 中，有四种常用的类型转换：

1）`static_cast`：用于在有明确定义的类型之间进行转换，如基本数据类型之间的转换以及指针或引用的上行转换（从派生类到基类）。

2）`dynamic_cast`：主要用于多态类型的指针或引用转换，特别适用于需要安全地执行下行转换（从基类到派生类）。

3）`const_cast`：用于移除对象的 `const` 或添加 `const` 属性，这在需要更改常量数据时非常有用。

4）`reinterpret_cast`：提供了一种最底层的转换方式，类似于 C 语言中的强转，通常用于指针类型之间的转换。

#### 扩展知识

我们详细地讨论下每种类型转换的更多细节和注意事项。

1）`static_cast`：

- **用法：**主要用于已知类型之间的转换，比如 `int` 转换为 `float` 或者从派生类指针转换为基类指针。
- **示例：**

```cpp
int a = 10;
float b = static_cast<float>(a);  // 将 int 转换为 float
```

- **注意事项：**`static_cast` 进行的类型转换在编译时检查，但不会进行运行时检查。

2）`dynamic_cast`：

- **用法：**主要用于多态基类，能够基于运行时类型信息将基类指针或引用转换为派生类指针或引用。
- **示例：**

```cpp
Base* basePtr = new Derived();
Derived* derivedPtr = dynamic_cast<Derived*>(basePtr);  // 运行时检查并转换
```

- **注意事项：**只有在类包含虚函数时才能使用 `dynamic_cast`，而如果转换失败，指针会返回 `nullptr`，引用则会抛出 `std::bad_cast` 异常。

3）`const_cast`：

- **用法：**移除或者添加常量修饰，通常在需要修改被标记为 `const` 的数据时使用。
- **示例：**

```cpp
const int a = 10;
int *b = const_cast<int*>(&a);  // 移除 const 属性
*b = 20;  // 修改原本为 const 的值
```

- **注意事项：**`const_cast` 仅影响底层 const 属性，而并不影响顶层 const。同样，修改原本为 const 的变量可能会引发未定义行为，应该谨慎使用。

4）`reinterpret_cast`：

- **用法：**主要用于在几乎无关的类型之间进行转换，比如将指针类型转换为整型，或相反。这通常用于底层操作，比如硬件编程，或某些预期内的操作。
- **示例：**

```cpp
long p = 0x12345678;
inti = reinterpret_cast<int*>(p);  // 将 long 转换为 int 指针
```

- **注意事项：**这种转换不会进行类型安全检查，可能会改变位模式，应尽量避免使用，除非确实需要进行底层操作。

![image-20250511155519335](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/202505111555400.png)

## 19. C++ 中 struct 和 class 的区别？

#### 回答重点

在 C++ 中，`struct` 和 `class` 的主要区别就在于它们的默认访问级别：

1）`struct` 的默认成员访问级别是 `public`。

2）`class` 的默认成员访问级别是 `private`。

#### 扩展知识

虽然默认访问级别是 `struct` 和 `class` 之间的主要区别，但在实际编程中，还有一些方面也值得注意：

**1）内存布局和性能：**

- 在大多数情况下，`struct` 和 `class` 的内存布局是一样的，因为它们本质上除了访问级别之外，其他都是相同的。
- 在访问级别相同的情况下，二者的性能并无区别。

**2）习惯用法：**

- `struct` 一般用于表示简单的数据结构或 `POD（Plain Old Data）` 类型。POD 类是所有非静态数据成员共享相同访问级别、没有虚函数、没有继承的类。
- `class` 一般用于表示复杂的数据类型，特别是在对象需要封装和抽象，以及需要使用功能性的成员函数时。

**3）继承模型：**

- 类可以有继承关系，通过 `private`、`protected` 和 `public` 指定继承的访问级别，默认是 `private` 继承。
- 结构体同样可以有继承关系，默认是 `public` 继承。
- 上述特指在 C++ 中，在 C 语言中的 `struct` 是没有继承能力的。

**4）编程风格：**

- 代码风格和团队规范可能对 `struct` 和 `class` 的使用有明确的指示。例如，在一个面向对象的项目中，使用 `class` 定义所有对象可能更为规范，而 `struct` 则更多地用于数据传输对象（DTO）。

**5）友元函数：**

- 友元函数或友元类在 `struct` 和 `class` 中都适用，用法完全一样，唯一的区别是，如果你在 `struct` 里通常不需要那么多封装，在这种情况下友元可能用的不多。

## 20. 🌟C++ 中 new 和 malloc 的区别？delete 和 free 的区别？

#### 回答重点

在 C++ 中，`new` 和 `malloc` 以及 `delete` 和 `free` 是内存管理的两对主要操作符和函数。它们虽然都有分配和释放内存的功能，但在很多方面都有区别。

1. `new` vs `malloc`:

   - `new` 是 C++ 的操作符，而 `malloc` 是 C 标准库的函数。
   - `new` 分配内存并调用构造函数，而 `malloc` 仅仅分配内存，不调用构造函数。
   - `new` 返回一个类型安全的指针，而 `malloc` 返回 `void*`，需要显式类型转换。
   - `new` 在分配失败时抛出 `std::bad_alloc` 异常，而 `malloc` 返回 `NULL`。
2. `delete` vs `free`:

   - `delete` 是 C++ 的操作符，而 `free` 是 C 标准库的函数。
   - `delete` 销毁对象并调用析构函数，然后释放内存，而 `free` 仅仅释放内存，不调用析构函数。
   - `delete` 必须与 `new` 配对使用，而 `free` 必须与 `malloc` 配对使用。
   - `delete` 和 `delete[]` 是不同的，前者用于单一对象，后者用于数组。`free` 没有这种区分。

#### 扩展知识

1. **更多关于**new和malloc的不同:
   - 异常处理: 在 `new` 语句中，如果内存分配失败，会抛出 `std::bad_alloc` 异常，你可以使用 `try-catch` 块处理这个异常。相比之下，`malloc` 返回 `NULL` 值，需要程序员手动检查并处理。
   - 类型兼容: `new` 更适合 C++ 中的类对象，因为它自动调用构造函数进行初始化，而 `malloc` 更适合简单的数据类型或 C 风格编程。
2. **更多关于**delete和free的不同:

   - 使用安全性: 使用 `delete` 时，不会像 `free` 那样导致未定义行为，因为它会调用析构函数来清理对象。在涉及复杂对象管理时，这种自动调用析构函数的特性非常有用。
   - 灵活性和匹配: 使用不同类型的 `delete` 操作符（`delete` 和 `delete[]`）来区分释放单个对象和对象数组。`free` 函数则没有这种灵活性。
3. **开发建议:**

   - 建议尽量使用智能指针（如 `std::unique_ptr` 和 `std::shared_ptr`），它们可以自动管理内存，减少内存泄漏和其他潜在的内存管理问题。


## C++ 面试真题（21-30）


## 21. 🌟左值和右值的区别？

#### 回答重点

什么是左值？什么是右值？

- 左值：可以出现在赋值运算符的左边，并且可以被取地址，通常是有名字的变量
- 右值：不能出现在赋值运算符的左边，不可以被取地址，表示一个具体的数据值，通常是常量、临时变量

一般可以从两个方向区分左值和右值。

方向 1：

- 左值：可以放到等号左边的东西叫左值。
- 右值：不可以放到等号左边的东西就叫右值。

方向 2：

- 左值：可以取地址并且有名字的东西就是左值。
- 右值：不能取地址的没有名字的东西就是右值。

示例：

`int a = b + c; `

a 是左值，有变量名，可以取地址，也可以放到等号左边, 表达式 b+c 的返回值是右值，没有名字且不能取地址，&(b+c)不能通过编译，而且也不能放到等号左边。

`int a = 4; // a是左值，4作为普通字面量是右值`

#### 扩展知识

###### 左值引用

可以理解为是对左值的引用。对于左值引用，等号右边的值必须可以取地址，如果不能取地址，则会编译失败，或者可以使用 const 引用形式，但这样就只能通过引用来读取输出，不能修改数组，因为是常量引用。

示例代码：

```cpp
int a = 5;
int &b = a; // b是左值引用
b = 4;
int &c = 10; // error，10无法取地址，无法进行引用
const int &d = 10; // ok，因为是常引用，引用常量数字，这个常量数字会存储在内存中，可以取地址
```

###### 右值引用

可以理解为是对右值的引用。即对一个临时对象或者即将销毁的对象的引用，开发者可以利用这些临时对象，却不需要复制它们。

如果使用右值引用，那表达式等号右边的值需要是右值，可以使用 std::move 函数强制把左值转换为右值。

```cpp
int a = 4;
int &&b = a; // error, a是左值
int &&c = std::move(a); // ok
```

###### 左值引用和右值引用的使用场景

- 左值引用：当你需要修改对象的值，或者需要引用一个持久对象时使用。
- 右值引用：当你需要处理一个临时对象，并且想要避免复制，或者实现移动语义时使用。

###### 纯右值

纯右值属于右值。运算表达式产生的临时变量、不和对象关联的原始字面量、非引用返回的临时变量、lambda 表达式等都是纯右值。

举例：

- 除字符串字面值外的字面值
- 返回非引用类型的函数调用
- 后置自增自减表达式 i++、i--
- 算术表达式(a+b, a*b, a&&b, a==b 等)
- 取地址表达式等(&a)

## 22. 介绍下移动语义与完美转发

#### 回答重点

移动语义和完美转发都是 C++11 引入的新特性。

###### 移动语义

一种优化资源管理的机制。常规的资源管理是拷贝别人的资源。而移动语义是转移所有权，转移了资源而不是拷贝资源，性能会更好。

移动语义通常用于那些比较大的对象，搭配移动构造函数或移动赋值运算符来使用。

示例代码：

```cpp
class A {
public:
    A(int size) : size_(size) {
        data_ = new int[size];
    }
    A(){}
    A(const A& a) {
        size_ = a.size_;
        data_ = new int[size_];
        cout << "copy " << endl;
    }
    A(A&& a) {
        this->data_ = a.data_;
        a.data_ = nullptr;
        cout << "move " << endl;
    }
    ~A() {
        if (data_ != nullptr) {
         delete[] data_;
        }
    }
    int *data_;
    int size_;
};
int main() {
    A a(10);
    A b = a;
    A c = std::move(a); // 调用移动构造函数
    return 0;
}
```

如果不使用 ```std::move```，会有很大的拷贝代价，使用移动语义可以避免很多无用的拷贝，提供程序性能，C++ 所有的 STL 都实现了移动语义，方便我们使用。例如：

```cpp
std::vector<string> vecs;
...
std::vector<string> vecm = std::move(vecs); // 免去很多拷贝
```

###### 完美转发

完美转发指可以写一个接受任意实参的函数模板，并转发到其它函数，目标函数会收到与转发函数完全相同的实参，转发函数实参是左值那目标函数实参也是左值，转发函数实参是右值那目标函数实参也是右值。

那如何实现完美转发呢？答案是使用 ` ``std::forward`，可参考以下代码：

```cpp
void PrintV(int &t) {
    cout << "lvalue" << endl;
}

void PrintV(int &&t) {
    cout << "rvalue" << endl;
}

template<typename T>
void Test(T &&t) {
    PrintV(t);
    PrintV(std::forward<T>(t));

    PrintV(std::move(t));
}

int main() {
    Test(1); // lvalue rvalue rvalue
    int a = 1;
    Test(a); // lvalue lvalue rvalue
    
    Test(std::forward<int>(a)); // lvalue rvalue rvalue
    
    Test(std::forward<int&>(a)); // lvalue lvalue rvalue
    
    Test(std::forward<int&&>(a)); // lvalue rvalue rvalue
    return 0;
}
```

- ![image-20250511160309828](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511160309828.png)

#### 扩展知识

深拷贝与浅拷贝

示例代码：

```cpp
class A {
public:
    A(int size) : size_(size) {
        data_ = new int[size];
    }
    A(){}
    A(const A& a) {
        size_ = a.size_;
        data_ = a.data_;
        cout << "copy " << endl;
    }
    ~A() {
        delete[] data_;
    }
    int *data_;
    int size_;
};
int main() {
    A a(10);
    A b = a;
    cout << "b " << b.data_ << endl;
    cout << "a " << a.data_ << endl;
    return 0;
}
```

上面代码中，两个输出的是相同的地址，a 和 b 的 data_ 指针指向了同一块内存，这就是浅拷贝，只是数据的简单赋值，那在析构时 data_ 内存会被释放两次，如何消除这种隐患呢，可以使用如下深拷贝：

```cpp
class A {
public:
    A(int size) : size_(size) {
        data_ = new int[size];
    }
    A(){}
    A(const A& a) {
        size_ = a.size_;
        data_ = new int[size_];
        cout << "copy " << endl;
    }
    ~A() {
        delete[] data_;
    }
    int *data_;
    int size_;
};
int main() {
    A a(10);
    A b = a;
    cout << "b " << b.data_ << endl;
    cout << "a " << a.data_ << endl;
    return 0;
}
```

深拷贝就是在拷贝对象时，如果被拷贝对象内部还有指针引用指向其它资源，自己需要重新开辟一块新内存存储资源，而不是简单的赋值。

## 23. 🌟C++ 中 move 有什么作用？它的原理是什么？

#### 回答重点

move 是 C++11 引入的一个新特性，用来实现移动语义。它的主要作用是将对象的资源从一个对象转移到另一个对象，而无需进行深拷贝，减少了资源内存的分配，可提高性能。

它的原理很简单，我们直接看它的源码实现：

```c
// move
template <class T>
LIBC_INLINE constexpr cpp::remove_reference_t<T> &&move(T &&t) {
  return static_cast<typename cpp::remove_reference_t<T> &&>(t);
}
```

源码取自：[https://github.com/llvm/llvm-project/blob/cceedc939a43c7c732a5888364251775bffc2dba/libc/src/__support/CPP/utility/move.h##L19](https://github.com/llvm/llvm-project/blob/cceedc939a43c7c732a5888364251775bffc2dba/libc/src/__support/CPP/utility/move.h##L19)

从源码中你可以看到，std::move 的作用只有一个，无论输入参数是左值还是右值，都强制转成右值。

#### 扩展知识

1）move 转成右值有什么好处？

这就涉及到移动语义的概念，右值可以触发移动语义，那什么是移动语义？我们可以理解为在对象转换的时候，通过右值可以触发到类的移动构造函数或者移动赋值函数。

因为触发了移动构造函数 或者 移动赋值函数，我们就默认，原对象后面已经不会再使用了（包括内部的某些内存），这样我们就可以在新对象中直接使用原对象的那部分内存，减少了数据的拷贝操作，昂贵的拷贝转为了廉价的移动，提升了程序的性能。

2）是不是 std::move 后的对象就没法使用了？

其实不是，还是取决于搭配的移动构造函数 和 移动赋值函数是如何实现的。

如果在移动构造函数 + 移动赋值函数中，还是使用了拷贝动作，那原对象还是可以使用的，见下面示例。

```c
#include <chrono>
include <functional>
#include <future>
#include <iostream>
#include <string>

class A {
public:
    A() {
        std::cout << "A() \n";
    }
    
    ~A() {
        std::cout << "~A() \n";
    }
    A(const A& a) {
        count_ = a.count_;
        std::cout << "A copy \n";
    }
    A& operator=(const A& a) {
        count_ = a.count_;
        std::cout << "A = \n";
        return *this;
    }
    
    A(A&& a) {
        count_ = a.count_;
        std::cout << "A move \n";
    }
    
    A& operator=(A&& a) {
        count_ = a.count_;
        std::cout << "A move = \n";
        return *this;
    }
    
    std::string count_;
};


int main() {
    A a;
    a.count_ = "12345";
    A b = std::move(a);
    std::cout << a.count_ << std::endl;
    std::cout << b.count_ << std::endl;
    return 0;
}
```

如果我们在移动构造函数 + 移动赋值函数中，将原对象内部内存废弃掉，新对象使用原对象内存，那原对象的内存就不可以用了，示例代码如下：

```c
#include <chrono>
#include <functional>
#include <future>
#include <iostream>
#include <string>

class A {
public:
    A() {
        std::cout << "A() \n";
    }
    
    ~A() {
        std::cout << "~A() \n";
    }
   
    A(const A& a) {
        count_ = a.count_;
        std::cout << "A copy \n";
    }
    
    A& operator=(const A& a) {
        count_ = a.count_;
        std::cout << "A = \n";
        return *this;
    }
    
    A(A&& a) {
        count_ = std::move(a.count_);
        std::cout << "A move \n";
    }
    
    A& operator=(A&& a) {
        count_ = std::move(a.count_);
        std::cout << "A move = \n";
        return *this;
    }
    
    std::string count_;
};


int main() {
    A a;
    a.count_ = "12345";
    A b = std::move(a);
    std::cout << a.count_ << std::endl;
    std::cout << b.count_ << std::endl;
    return 0;
}
```

**总结：**

- std::move 函数的作用是将参数强制转换为右值。而且，只是转换为右值，并不会对对象进行任何操作。
- 转换为右值可以触发移动语义，减少数据的拷贝操作，提升程序的性能。
- 在使用 std::move 函数后，原对象是否可以继续使用取决于移动构造函数和移动赋值函数的实现。

## 24. 介绍 C++ 中三种智能指针的使用场景？

#### 回答重点

C++ 中的智能指针主要用于管理动态分配的内存，避免内存泄漏。

C++11 标准引入了三种主要的智能指针：`std::unique_ptr`、`std::shared_ptr` 和 `std::weak_ptr`：

###### 1. `std::unique_ptr`

`std::unique_ptr` 是一种独占所有权的智能指针，意味着同一时间内只能有一个 `unique_ptr` 指向一个特定的对象。当 `unique_ptr` 被销毁时，它所指向的对象也会被销毁。

**使用场景：**

- 当你需要确保一个对象只被一个指针所拥有时。
- 当你需要自动管理资源，如文件句柄或互斥锁时。

**示例代码：**

```cpp
#include <iostream>
#include <memory>

class Test {
public:
    Test() { std::cout << "Test::Test()"; }
    ~Test() { std::cout << "Test::~Test()"; }
    void test() { std::cout << "Test::test()"; }
};

int main() {
    std::unique_ptr<Test> 
    ptr(new Test());
    ptr->test();
    // 当ptr离开作用域时，它指向的对象会被自动销毁return 0;
}
```

###### 2. `std::shared_ptr`

`std::shared_ptr` 是一种共享所有权的智能指针，多个 `shared_ptr` 可以指向同一个对象。内部使用引用计数来确保只有当最后一个指向对象的 `shared_ptr` 被销毁时，对象才会被销毁。

**使用场景：**

- 当你需要在多个所有者之间共享对象时。
- 当你需要通过复制构造函数或赋值操作符来复制智能指针时。

**示例代码：**

```cpp
#include <iostream>
#include <memory>

class Test {
public:
    Test() { std::cout << "Test::Test()"; }
    ~Test() { std::cout << "Test::~Test()"; }
    void test() { std::cout << "Test::test()"; }
};

int main() {
    std::shared_ptr<Test> ptr1(new Test());
    std::shared_ptr<Test> ptr2 = ptr1;
    ptr1->test();
    // 当ptr1和ptr2离开作用域时，它们指向的对象会被自动销毁
    return 0;
}
```

###### 3. `std::weak_ptr`

`std::weak_ptr` 是一种不拥有对象所有权的智能指针，它指向一个由 `std::shared_ptr` 管理的对象。`weak_ptr` 用于解决 `shared_ptr` 之间的循环引用问题。

是另外一种智能指针，它是对 shared_ptr 的补充，`std::weak_ptr` 是一种弱引用智能指针，用于观察 std::shared_ptr 指向的对象，而不影响引用计数。它主要用于解决循环引用问题，从而避免内存泄漏，另外如果需要追踪指向某个对象的第一个指针，则可以使用 weak_ptr。

可以考虑在对象本身中维护一个指向第一个 shared_ptr 的弱引用（std::weak_ptr）。当创建对象的第一个 shared_ptr 时，将这个 shared_ptr 赋值给对象的 weak_ptr 成员。这样，在需要时，可以通过检查对象的 weak_ptr 成员来获取指向对象的第一个 shared_ptr（如果仍然存在的话）.

**使用场景：**

- 当你需要访问但不拥有由 `shared_ptr` 管理的对象时。
- 当你需要解决 `shared_ptr` 之间的循环引用问题时。
- 注意 `weak_ptr` 肯定要和 `shared_ptr` 搭配使用。

**示例代码：**

```cpp
#include <iostream>
#include <memory>

class Test {
public:
    Test() { std::cout << "Test::Test()"; }
    ~Test() { std::cout << "Test::~Test()"; }
    void test() { std::cout << "Test::test()"; }
};

int main() {
    std::shared_ptr<Test> sharedPtr(new Test());
    std::weak_ptr<Test> weakPtr = sharedPtr;
    
    if (auto lockedSharedPtr = weakPtr.lock()) {
        lockedSharedPtr->test();
    }// 当sharedPtr离开作用域时，它指向的对象会被自动销毁
    return 0;
}
```

这三种智能指针各有其用途，选择哪一种取决于你的具体需求。

#### 扩展知识

1）智能指针方面的建议：

- 尽量使用智能指针，而非裸指针来管理内存，很多时候利用 `RAII` 机制管理内存肯定更靠谱安全的多。
- 如果没有多个所有者共享对象的需求，建议优先使用 `unique_ptr` 管理内存，它相对 `shared_ptr` 会更轻量一些。
- 在使用 `shared_ptr` 时，一定要注意是否有循环引用的问题，因为这会导致内存泄漏。
- `shared_ptr` 的引用计数是安全的，但是里面的对象不是线程安全的，这点要区别开。

2）为什么 `std::unique_ptr` 可以做到不可复制，只可移动？

因为把拷贝构造函数和赋值运算符标记为了 delete，见源码：

```c
template <typename _Tp, typename _Tp_Deleter = default_delete<_Tp> > 
class unique_ptr {
        // Disable copy from lvalue.
        unique_ptr(const unique_ptr&) = delete;
       
        template<typename _Up, typename _Up_Deleter> 
        unique_ptr(const unique_ptr<_Up, _Up_Deleter>&) = delete;
        
        unique_ptr& operator=(const unique_ptr&) = delete;
    
     template<typename _Up, typename _Up_Deleter> 
     unique_ptr& operator=(const unique_ptr<_Up, _Up_Deleter>&) = delete;
};
```

3）`shared_ptr` 的原理：

每个 std::shared_ptr 对象包含两个成员变量：一个指向被管理对象的原始指针，一个指向引用计数块的指针（control block pointer）。

引用计数块是一个单独的内存块，引用计数块允许多个 std::shared_ptr 对象共享相同的引用计数，从而实现共享所有权。

当创建一个新的 std::shared_ptr 时，引用计数初始化为 1，表示对象当前被一个 shared_ptr 管理。

1. 拷贝 std::shared_ptr：当用一个 shared_ptr 拷贝出另一个 shared_ptr 时，需要拷贝两个成员变量（被管理对象的原始指针和引用计数块的指针），并同时将引用计数值加 1。这样，多个 shared_ptr 对象可以共享相同的引用计数。
2. 析构 std::shared_ptr：当 shared_ptr 对象析构时，引用计数值减 1。然后检测引用计数是否为 0。如果引用计数为 0，说明没有其他 shared_ptr 对象指向该资源，因此需要同时删除原始对象（通过调用自定义删除器，如果有的话）。

4）智能指针的缺点

1. 性能开销，需要额外的内存来存储他们的控制块，控制块包括引用计数，以及运行时的原子操作来增加或减少引用技术，这可能导致裸指针的性能下降。
2. 循环引用问题，如果两个对象通过成员变量 `shared_ptr` 相互引用，并且没有其他指针指向这两个对象中的任何一个，那么这两个对象的内存将永远不会被释放，导致内存泄露。

```cpp
#include<iostream>
#include<memory>
class B; // 前向声明
class A {
public:
    std::shared_ptr<B> b_ptr;
    ~A() {
        std::cout << "A has been destroyed."<< std::endl;
    }
};
class B {
public:
    std::shared_ptr<A> a_ptr;
    ~B() {
        std::cout << "B has been destroyed."<< std::endl;
    }
};
int main() {
    std::shared_ptr<A> a = std::make_shared<A>();
    std::shared_ptr<B> b = std::make_shared<B>();
    a->b_ptr = b; // A 引用 B
    b->a_ptr = a; // B 引用 A
    // 由于存在循环引用，A 和 B 的析构函数将不会被调用，从而导致内存泄漏
    return 0;
}
```

1. 不一定适用于所有场景：有一些容器类，内部实现依赖于裸指针，另外在考虑某些性能关键场景下，使用裸指针可能更合适。

![image-20250511155712503](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/202505111557598.png)



## 25. 🌟C++11 中有哪些常用的新特性？

#### 回答重点

C++11 新特性几乎是面试必问的一个话题，可以主要回答以下几个特性：

- auto 类型推导
- 智能指针
- RAII lock
- std::thread
- 左值右值
- std::function 和 lambda 表达式

###### auto 类型推导

auto 可以让编译器在编译时就推导出变量的类型，看代码：

```c
auto a = 10; // 10是int型，可以自动推导出a是int

int i = 10;
auto b = i; // b是int型

auto d = 2.0; // d是double型
auto f =  { // f是啥类型？直接用auto就行
    return std::string("d");
}
```

利用 auto 可以通过=右边的类型推导出变量的类型。

什么时候使用 auto 呢？简单类型其实没必要使用 auto，某些复杂类型就有必要使用 auto，比如 lambda 表达式的类型，async 函数的类型等，例如：

```c
auto func = [&] {
    cout << "xxx";
}; // 对于func你难道不使用auto吗，反正我是不关心lambda表达式究竟是什么类型。
auto asyncfunc = std::async(std::launch::async, func);
```

###### 智能指针

C++11 新特性中主要有两种智能指针 `std::shared_ptr` 和 `std::unique_ptr`。

那什么时候使用 `std::shared_ptr`，什么时候使用 `std::unique_ptr` 呢？

- 当所有权不明晰的情况，有可能多个对象共同管理同一块内存时，要使用 `std::shared_ptr`；
- 而 `std::unique_ptr` 强调的是独占，同一时刻只能有一个对象占用这块内存，不支持多个对象共同管理同一块内存。

两类智能指针使用方式类似，拿 std::unique_ptr 举例：

```c
using namespace std;

struct A {
   ~A() {
       cout << "A delete" << endl;
   }
   void Print() {
       cout << "A" << endl;
   }
};

int main() {
    auto ptr = std::unique_ptr<A>(new A);
    auto tptr = std::make_unique<A>(); // error, c++11还不行，需要c++14
    std::unique_ptr<A> tem = ptr; // error, unique_ptr不允许移动，编译失败
    ptr->Print();
    return 0;
}
```

###### RAII lock

C++11 提供了两种锁封装，通过 RAII 方式可动态的释放锁资源，防止编码失误导致始终持有锁。

这两种封装是 `std::lock_guard` 和 `std::unique_lock`，使用方式类似，看下面的代码：

```c
#include <iostream>
#include <mutex>
#include <thread>
#include <chrono>

using namespace std;
std::mutex mutex_;

int main() {
   auto func1 = [](int k) {
       // std::lock_guard<std::mutex> lock(mutex_);
       std::unique_lock<std::mutex> lock(mutex_);
       for (int i = 0; i < k; ++i) {
           cout << i << " ";
      }
      cout << endl;
  };
   std::thread threads[5];
   for (int i = 0; i < 5; ++i) {
       threads[i] = std::thread(func1, 200);
  }
   for (auto& th : threads) {
       th.join();
  }
   return 0;
}
```

普通情况下建议使用 std::lock_guard，因为 std::lock_guard 更加轻量级，但如果用在条件变量的 wait 中环境中，必须使用 std::unique_lock。

###### std::thread

什么是多线程这里就不过多介绍，新特性关于多线程最主要的就是 std::thread 的使用，它的使用也很简单，看代码：

```c
#include <iostream>
#include <thread>

using namespace std;

int main() {
    auto func =  {
        for (int i = 0; i < 10; ++i) {
            cout << i << " ";
      }
        cout << endl;
  };
   std::thread t(func);
   if (t.joinable()) {
       t.detach();
  }
   auto func1 = [](int k) {
       for (int i = 0; i < k; ++i) {
          cout << i << " ";
      }
       cout << endl;
  };
   std::thread tt(func1, 20);
   if (tt.joinable()) { // 检查线程可否被join
       tt.join();
  }
   return 0;
}
```

这里记住，`std::thread` 在其对象生命周期结束时必须要调用 `join()` 或者 `detach()`，否则程序会 `terminate()`，这个问题在 C++20 中的 `std::jthread` 得到解决，但是 C++20 现在多数编译器还没有完全支持所有特性，先暂时了解下即可，项目中没必要着急使用。

###### 左值右值

关于左值和右值，有两种方式理解：

**概念 1：**

左值：可以放到等号左边的东西叫左值。

右值：不可以放到等号左边的东西就叫右值。

**概念 2：**

左值：可以取地址并且有名字的东西就是左值。

右值：不能取地址的没有名字的东西就是右值。

###### std::function 和 lambda 表达式

这两个可以说是很常用的特性，使用它们会让函数的调用相当方便。使用 `std::function` 可以完全替代以前那种繁琐的函数指针形式。

还可以结合 `std::bind` 一起使用，直接看一段示例代码：

```c
std::function<void(int)> f; // 这里表示function的对象f的参数是int，返回值是void
#include <functional>
#include <iostream>

struct Foo {
   Foo(int num) : num_(num) {}
   void print_add(int i) const { std::cout << num_ + i << '\n'; }
   int num_;
};

void print_num(int i) { std::cout << i << '\n'; }

struct PrintNum {
   void operator()(int i) const { std::cout << i << '\n'; }
};

int main() {
   // 存储自由函数
   std::function<void(int)> f_display = print_num;
   f_display(-9);
  
   // 存储 lambda
   std::function<void()> f_display_42 =  { print_num(42); };
   f_display_42();
   
   // 存储到 std::bind 调用的结果
   std::function<void()> f_display_31337 = std::bind(print_num, 31337);
   f_display_31337();
   
   // 存储到成员函数的调用
   std::function<void(const Foo&, int)> f_add_display = &Foo::print_add;const Foo foo(314159);
   f_add_display(foo, 1);
   f_add_display(314159, 1);
   
   // 存储到数据成员访问器的调用
   std::function<int>(Foo const&)> f_num = &Foo::num_;std::cout << "num_: " << f_num(foo) << '\n';
   
   // 存储到成员函数及对象的调用
   using std::placeholders::_1;std::function<void(int)> f_add_display2 = std::bind(&Foo::print_add, foo, _1);
   f_add_display2(2);
   
   // 存储到成员函数和对象指针的调用
   std::function<void(int)> f_add_display3 = std::bind(&Foo::print_add, &foo, _1);
   f_add_display3(3);
   
   // 存储到函数对象的调用
   std::function<void(int)> f_display_obj = PrintNum();
   f_display_obj(18);
}
```

从上面可以看到 `std::function` 的使用方法，当给 `std::function` 填入合适的参数表和返回值后，它就变成了可以容纳所有这一类调用方式的函数封装器。`std::function` 还可以用作回调函数，或者在 `C++` 里如果需要使用回调那就一定要使用 `std::function`，特别方便。

`lambda` 表达式可以说是 `C++11` 引入的最重要的特性之一，它定义了一个匿名函数，可以捕获一定范围的变量在函数内部使用，一般有如下语法形式：

```c
auto func = [capture] (params) opt -> ret { func_body; };
```

其中 func 是可以当作 lambda 表达式的名字，作为一个函数使用，capture 是捕获列表，params 是参数表，opt 是函数选项(mutable 之类)， ret 是返回值类型，func_body 是函数体。

看下面这段使用 lambda 表达式的示例：

```c
auto func1 = [](int a) -> int { return a + 1; }; 
auto func2 = [](int a) { return a + 2; }; 
cout << func1(1) << " " << func2(2) << endl;
```

`std::function` 和 `std::bind` 使得我们平时编程过程中封装函数更加的方便，而 lambda 表达式将这种方便发挥到了极致，可以在需要的时间就地定义匿名函数，不再需要定义类或者函数等，在自定义 `STL` 规则时候也非常方便，让代码更简洁，更灵活，提高开发效率。

#### 扩展知识

###### std::chrono

`chrono` 很强大，平时的打印函数耗时，休眠某段时间等，都可使用 `chrono`。

在 `C++11` 中引入了 `duration`、`time_point` 和 `clocks`，在 `C++20` 中还进一步支持了日期和时区。这里简要介绍下 `C++11` 中的这几个新特性。

duration

`std::chrono::duration` 表示一段时间，常见的单位有 s、ms 等，示例代码：

```c
// 拿休眠一段时间举例，这里表示休眠100ms
std::this_thread::sleep_for(std::chrono::milliseconds(100));
```

sleep_for 里面其实就是 `std::chrono::duration`，表示一段时间，实际是这样：

```c
typedef duration<int64_t, milli> milliseconds;
typedef duration<int64_tseconds;
```

duration 具体模板如下：

```c
template <class Rep, class Period = ratio<1> > class duration;
```

Rep 表示一种数值类型，用来表示 Period 的数量，比如 int、float、double，Period 是 ratio 类型，用来表示【用秒表示的时间单位】比如 second，常用的 `duration` 已经定义好了，在 `std::chrono::duration` 下：

- ratio<3600, 1>：hours
- ratio<60, 1>：minutes
- ratio<1, 1>：seconds
- ratio<1, 1000>：microseconds
- ratio<1, 1000000>：microseconds
- ratio<1, 1000000000>：nanosecons

ratio 的具体模板如下：

```c
template <intmax_t N, intmax_t D = 1class ratio;
```

N 代表分子，D 代表分母，所以 ratio 表示一个分数，我们可以自定义 Period，比如 ratio<2, 1> 表示单位时间是 2 秒。

time_point

表示一个具体时间点，如 2020 年 5 月 10 日 10 点 10 分 10 秒，拿获取当前时间举例：

```c
std::chrono::time_point<std::chrono::high_resolution_clock> Now() {return std::chrono::high_resolution_clock::now();
}
// std::chrono::high_resolution_clock为高精度时钟，下面会提到
```

clocks

时钟，chrono 里面提供了三种时钟：

1）steady_clock

稳定的时间间隔，表示相对时间，相对于系统开机启动的时间，无论系统时间如何被更改，后一次调用 now()肯定比前一次调用 now()的数值大，可用于计时。

2）system_clock

表示当前的系统时钟，可以用于获取当前时间：

```c
int main() {
   using std::chrono::system_clock;
   system_clock::time_point today = system_clock::now();
   std::time_t tt = system_clock::to_time_t(today);
   std::cout << "today is: " << ctime(&tt);
   return 0;
}
// today is: Sun May 10 09:48:36 2020
```

3）high_resolution_clock

`high_resolution_clock` 表示系统可用的最高精度的时钟，实际上就是 `system_clock` 或者 `steady_clock` 其中一种的定义，官方没有说明具体是哪个，不同系统可能不一样，之前看 gcc chrono 源码中 `high_resolution_clock` 是 `steady_clock` 的 typedef。

###### 条件变量

条件变量是 C++11 引入的一种同步机制，它可以阻塞一个线程或多个线程，直到有线程通知或者超时才会唤醒正在阻塞的线程，条件变量需要和锁配合使用，这里的锁就是上面介绍的 `std::unique_lock`。

这里使用条件变量实现一个 `CountDownLatch`：

```c
class CountDownLatch {
   public:
    explicit CountDownLatch(uint32_t count) : count_(count);

    void CountDown() {
        std::unique_lock<std::mutex> lock(mutex_);
        --count_;
        if (count_ == 0) {
            cv_.notify_all();
        }
    }
    
void Await(uint32_t time_ms = 0) {std::unique_lock<std::mutex> lock(mutex_);while (count_ > 0) {if (time_ms > 0) {
                cv_.wait_for(lock, std::chrono::milliseconds(time_ms));
            } else {
                cv_.wait(lock);
            }
        }
    }
    
    uint32_t GetCount() const {
        std::unique_lock<std::mutex> lock(mutex_);
     return count_;
    }
   
    private:
    std::condition_variable cv_;
    mutable std::mutex mutex_;
    uint32_t count_ = 0;
};
```

![image-20250511160409369](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511160409369.png)



## 26. 🌟C++ 中 inline 的作用？它有什么优缺点？

#### 回答重点

`inline` 的作用是建议编译器将函数调用替换为函数体，以减少函数调用的开销，和宏比较类似。

使用 `inline` 函数的目的一定是希望可以提高程序的运行效率，特别是那些频繁调用的小函数。

优点：

- 降低函数调用的开销，原理就是因为省去了调用和返回的指令开销。
- 如果函数体较小，可以提高代码执行的效率。

缺点：

- 容易导致代码膨胀，整个可执行程序体积变大，特别是当 `inline` 函数体较大且被多次调用时。
- 内联是一种建议，编译器可以选择忽略 `inline` 关键字。

#### 扩展知识

下面深入了解下 `inline` 函数。

- 内联函数的典型应用场景：

  - 内联函数不仅适用于短小的函数，例如简单的 getter 和 setter，它还适合一些占用时间较短的算法，很多算法都会在语言层面考虑内联来提升性能。
  - 需要频繁调用而且内联能显著提高性能的地方。
- 内联函数与宏的区别：宏是在预处理阶段进行文本替换，而内联函数是在编译阶段展开。内联函数有类型安全和作用域控制，宏没有这一特性。内联函数可以更好地报告调试信息，相对来说调试比较方便。
- 在优化级别较高时，即使未加 `inline` 关键字，编译器也可能自动将频繁调用的小函数设为内联。
- `inline` 的作用不仅仅是优先内联，它逐渐演变成了允许多重定义的含义。

## 27. C++ 中 explicit 的作用？

#### 回答重点

关键字 `explicit` 的主要作用是防止构造函数或转换函数在不合适的情况下被隐式调用。

例如，如果有一个只有一个参数的构造函数，加上 `explicit` 关键字后，编译器就不会自动用该构造函数进行隐式转换。这可以避免由于意外的隐式转换导致的难以调试的行为。

```cpp
class Foo {
public:
    explicit Foo(int x) : value(x) {}
private:
    int value;
};

void func(Foo f) {
    // ...
}

int main() {
    Foo foo = 10;  // 错误，必须使用 Foo foo(10) 或 Foo foo = Foo(10)
    func(10);      // 错误，必须使用 func(Foo(10))
}
```

如果没有 `explicit` 关键字，`Foo foo = 10;` 以及 `func(10);` 这样的代码是可以通过编译的，这会导致一些意想不到的隐式转换。

#### 扩展知识

1）历史背景

`explicit` 关键字在 C++98 标准中引入，用来增强类型安全，防止不经意的隐式转换。从 C++11 开始，`explicit` 可以用于 conversion operator。

2）使用场景

- 防止单参数构造函数隐式转换
- 如果一个类的构造函数接受一个参数，而你并不希望通过隐式转换来创建这个类的实例，就应该在构造函数前加 `explicit`。这也是它最主要的作用。

```cpp
class Bar {
public:
    explicit Bar(int x) : value(x) {}
private:
   int value;
};

Bar bar = 10;  // 错误，无法隐式转换
```

- 防止 conversion operator 隐式转换
- 类中有时会定义一些转换操作符，但有些转换是需要显式调用的，这时也可以使用 `explicit`。

```cpp
class Double {
public:
    explicit operator int() const {
        return static_cast<int>(value);
    }
private:
   double value;
};

Double d;
int i = d;               // 错误，无法隐式转换
int j = static_cast<int>(d);  // 正确，显式转换
```

3）复杂构造函数

对于那些带有默认参数的复杂构造函数，`explicit` 尤其重要，它们可能会被意外地调用。

```cpp
class Widget {
public:explicit Widget(int x = 0, bool flag = true) : value(x), flag(flag) {}
private:int value;bool flag;
};
```

这种情况下，如果不加 `explicit`，没有任何参数传递给构造函数也可能会进行隐式转换，引发难以察觉的错误。

## 28. 🌟C++ 中野指针和悬挂指针的区别？

#### 回答重点

两者都可能导致程序产生不可预测的行为。但它们有明显的区别：

**1）野指针：**一种未被初始化的指针，通常会指向一个随机的内存地址。这个地址不可控，使用它可能会导致程序崩溃或数据损坏。

```cpp
int *p;
std::cout<< *p << std::endl;
```

**2）悬挂指针：**一个原本合法的指针，但指向的内存已被释放或重新分配。当访问此指针指向的内存时，会导致未定义行为，因为那块内存数据可能已经不是期望的数据了。

```cpp
int main(void) {
  int * p = nullptr;
  int* p2 = new int;

  p = p2;
  delete p2;
}
```

#### 扩展知识

展开说一说，弄清楚这两个概念不难，但如何避免和处理它们才是关键，这直接关系到写出更健壮的代码。

**1）如何避免野指针**

- 初始化指针：在声明一个指针时，立即赋予它一个明确的数值，可以是一个有效的地址，也可以是 nullptr。

```cpp
int *ptr = nullptr; // 初始化
```

- 使用智能指针：C++ 中的智能指针（如 `std::unique_ptr` 和 `std::shared_ptr`）可以帮助自动管理指针的生命周期，减少手动管理的错误。

```cpp
std::unique_ptr<int> ptr(new int(10));
```

**2）如何避免悬挂指针**

- 在删除对象后，将指针设置为 nullptr，确保指针不再指向已经释放的内存。

```cpp
delete ptr;
ptr = nullptr;
```

- 尽量使用智能指针，它们会自动处理指针的生命周期，减少悬挂指针的产生。

```cpp
std::shared_ptr<int> ptr1 = std::make_shared<int>(10);
{
    std::shared_ptr<int> ptr2 = ptr1;
    // 当 ptr2 离开作用域后，资源仍然被 ptr1 管理
}
// 仍然可以使用 ptr1
```

**3）检测工具**

- 静态分析工具（如 Clang-Tidy、cppcheck）和动态分析工具（Valgrind、AddressSanitizer）可以帮助检测这些错误，确保代码质量。

![image-20250511160459848](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511160459848.png)



## 29. 🌟 什么是内存对齐？为什么要内存对齐？

#### 回答重点

内存对齐是指计算机在访问内存时，会根据一些规则来为数据指定一个合适的起始地址。

计算机的内存是以字节为基本单位进行编址，但是不同类型的数据所占据的内存空间大小是不一样的，在 C++ 语言中可以用 sizeof()来获得对应数据类型的字节数，一些计算机硬件平台要求存储在内存中的变量按照自然边界对齐，也就是说必须使**数据存储的起始地址可以整除数据实际占据内存的字节数**，这叫做内存对齐。

通常，这些地址是固定数字的整数倍。这样做，可以提高 CPU 的访问效率，尤其是在读取和写入数据时。

为什么要内存对齐？主要有以下几个原因：

**1）性能提升：**对齐的数据操作可以让 CPU 在一次内存周期内更高效地读取和写入，减少内存访问次数。通过内存布局优化技术，可以提高程序的运行效率和可靠性，这是因为可以减少 CPU 访问内存的次数，提高计算机新能，计算机在每次访问内存的时候，每次读取到的一定长度，就是操作系统的默认对齐系数，或者其系数的整数倍。

![](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/202505101800072.png)

以 32 位 Intel CPU 为例（16 和 64 位类同），一次可以对一个 32 位的数进行运算，它的数据总线的宽度是 32 位，即 CPU 字长为 32 位。

假设 long1 和 long2 变量内存分配结构如下： long1，long2 类型都为 long，long1 在内存中的位置正好与内存字边界对齐，CPU 存取这个数只需访问内存 1 次，而 long2 在内存中跨越字边界，导致 CPU 存取这个数则需访问内存 2 次。由此可以看出，字节对齐主要提高 CPU 访问内存的效率。内存对齐之后，存取 long2 ，一次访问即可。

**2）硬件限制：**某些架构要求数据必须对齐，否则可能会引发硬件异常或需要额外的处理时间。有些 CPU 可以访问任意地址上的任意数据，而有些 CPU 只能在特定地址访问数据，因此不同硬件平台具有差异性，这样的代码就不具有移植性。

**3）可移植性：**代码在不同架构上运行时，遵从内存对齐规则可以减少潜在的问题。

#### 扩展知识

内存对齐虽然看起来只是一个约定或者优化策略，其实背后还是有不少细致的讲究的：

**1）对齐要求：**不同的数据类型有不同的对齐要求。例如，在大多数 32 位系统中，int 通常要求 4 字节对齐，而 double 可能要求 8 字节对齐。编译器会根据这些对齐要求调整结构体或类的成员变量的布局。

**2）填充字节：**为了确保对齐，编译器有时会在数据成员之间插入一些“填充字节”（padding bytes），这些字节本身不保存有用的数据，只是为了使下一个成员变量满足对齐要求。例如，如果一个结构体的成员变量有 int 和 char，编译器可能会在 char 后面插入几个字节的填充，以确保下一个 int 的对齐。

**3）控制内存对齐：**在 C/C++ 中，我们可以使用编译器提供的关键字或扩展来控制数据的对齐方式，例如通过 `##pragma pack` 控制。

**4）与缓存一致性相关：**内存对齐有时候还与缓存一致性联系在一起。CPU 有自己的缓存系统，合理的内存对齐往往能使缓存更高效地工作，减少 cache miss 的概率。

结构体内存对齐

例子 1：假设 32 位系统，long 占 4 个字节。

```cpp
struct AlignA
{
    char a;
    long b;
    int c;
}
```

该结构体，占用内存为 4+ 4+ 4 =12 个字节，这就是内存对齐的原因。

![](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/202505101759489.png)

例子 2

```cpp
struct AlignB
{
    int b;
    char c[10];
    double a;
}
```

![](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/202505101759695.png)

成员变量布局调整

通过上面了解了结构体的内存对齐，是不是就可以想到，如果修改了结构体内变量的位置，就可以减少结构体的大小了。

## 30. C++ 中静态多态，动态多态是什么？

静态多态也叫编译时多态，它是在编译阶段就确定要调用哪个函数。

实现方式主要有函数重载和模板，函数重载就是在一个类或者同一个作用域里有好几个同名函数，但它们的参数列表不同，编译器会根据你调用函数时传的实参类型和数量，在编译的时候就选好要调用哪个函数。

模板则是可以创建通用的函数或者类，根据不同的模板参数，编译器在编译时生成不同的代码，静态多态的优点是速度快，因为编译时就确定了调用，没有额外的运行时开销。

动态多态也叫运行时多态，它是在程序运行的时候才确定要调用哪个函数。主要通过虚函数和继承来实现。在基类里把函数声明成虚函数，派生类可以重写这个虚函数。

当用基类的指针或者引用去调用这个虚函数时，程序在运行时会根据指针或者引用实际指向的对象类型，来决定调用基类的函数还是派生类重写后的函数。动态多态的好处是灵活性高，能让代码更有扩展性，但因为要在运行时做判断，会有一些额外的开销。

打个比方，静态多态就像你出门前就根据天气决定穿什么衣服；动态多态就像你出门了，到地方才根据实际情况换衣服。

![image-20250511160518787](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511160518787.png)


## C++ 面试真题（31-40）

## 31. 🌟C++ 中虚函数的原理？

#### 回答重点

虚函数是 C++ 中实现多态的一个关键机制。简单来说，虚函数允许你在基类里通过 `virtual` 声明一个函数，然后在派生类里对其进行重新定义。

通过使用虚函数，C++ 可以根据对象的实际类型（而不是引用或指针的静态类型）调用派生类的函数实现。实现虚函数的关键在于虚函数表（vtable）和虚函数表指针（vptr）。

每个含有虚函数的类都有一张虚函数表，表中存有该类的虚函数的地址。每个对象都有一个虚函数表指针，指向这个类的虚函数表。当调用虚函数时，程序会通过对象的虚函数表指针找到相应的虚函数地址，然后进行函数调用。

#### 扩展知识

**1）虚函数的实现原理：**

- **虚函数表（vtable）：**是一个存储虚函数地址的数组。每个包含虚函数的类会有一个虚函数表。表里存有该类或者基类中重写虚函数的实际地址。
- **虚函数表指针（vptr）：**每个对象在内存布局中会有一个指向虚函数表的指针。编译器会自动管理这个指针的初始化和赋值。

**2）多态的实现：**

虚函数是实现多态的一种手段，允许程序在运行时决定调用哪个类的函数，实现动态绑定。当基类指针或引用指向派生类对象时，调用虚函数会根据实际对象类型选择合适的函数实现，下面是示例代码：

```cpp
class Base {
public:virtual void show() {
        cout << "Base class show" << endl;
    }
};

class Derived : public Base {
public:
    void show() override {
        cout << "Derived class show" << endl;
    }
};

// 用例
Base *b;
Derived d;
b = &d;
b->show();  // 将会调用 Derived 的 show 方法
```

**3）注意点：**

- 虚函数的调用比普通函数多了一个 vtable 查找过程，运行时略有开销。
- 析构函数如果需要在派生类中被正确的调用，应该声明为虚函数。

**4）虚函数和纯虚函数：**

- 如果类中包含纯虚函数，那么这个类就是一个抽象类，不能实例化，只能被继承。

```cpp
class Abstract {
public:virtual void pure_virtual_func() = 0;  // 纯虚函数
};
```

**5）常见误区：**

- 静态绑定的成员函数（static 关键字）不能是虚函数。

## 32. 🌟C++ 中构造函数可以是虚函数吗？

#### 回答重点

构造函数不能是虚函数。

虚函数的机制依赖于虚函数表，而虚表对象的建立需要在调用构造函数之后才能完成。因为构造函数是用来初始化对象的，而在对象的初始化阶段虚表对象还没有被建立，如果构造函数是虚函数，就会导致对象初始化和多态机制的矛盾，因此，构造函数不能是虚函数。

#### 扩展知识

**1） 析构函数可以是虚函数**

虽然构造函数不能是虚函数，但是析构函数应当是虚函数，特别是在基类中。这样做的目的是为了确保在删除一个指向派生类对象的基类指针时，能正确调用派生类对象的析构函数，从而避免资源泄露。

**2） 其他特殊成员函数也不是虚函数**

除了构造函数外，静态成员函数和友元函数也不能是虚函数。静态成员函数与类而不是与某个对象相关联，而友元函数则不属于类的成员函数，它们不具备多态性所需的对象上下文。

**3） 解决方案**

如果需要在对象创建时实现多态性，可以考虑工厂模式等设计模式来间接实现多态性。这些设计模式可以通过一些间接的手段，在对象创建过程中提供多态行为。

## 33. 🌟C++ 中析构函数一定要是虚函数吗？

#### 回答重点

C++ 中析构函数并不一定要是虚函数，但在多态条件下，我是建议一定将其声明为虚函数。

如果一个类可能会被继承，并且你需要在删除指向派生类对象的基类指针时确保正确调用派生类的析构函数，那么基类的析构函数必须是虚函数。如果没有这样做，可能会导致资源泄漏或者未能正确释放派生类对象的资源。

#### 扩展知识

1）虚函数的机制及其应用场景

当一个类的成员函数被声明为虚函数时，C++ 会为该类生成一个虚函数表，这个表存储指向虚函数的指针。在运行时，基于当前对象的实际类型，虚函数表指针用于动态绑定，调用正确的函数版本。

场景：假设有一个基类 `Base` 和一个派生类 `Derived`：

```cpp
class Base {
public:
    virtual ~Base() { std::cout << "Base destructor" << std::endl; }
};

class Derived : public Base {
public:
    ~Derived() { std::cout << "Derived destructor" << std::endl; }
};
```

如果基类的析构函数不是虚函数，那么以下代码可能会产生问题：

```cpp
Base *obj = new Derived();
delete obj;  // 只调用了 Base 的析构函数，可能导致内存泄漏或未释放资源
```

**2）默认情况下析构函数的行为**

如果基类的析构函数不是虚函数，当你通过基类指针删除派生类对象时，只会调用基类的析构函数，而不会调用派生类的析构函数。这种情况下，派生类中分配的资源可能不会及时释放，导致资源泄漏。

**3）什么时候不需要虚析构函数**

如果一个类不设计为基类，或者不会通过基类指针删除派生类对象，那么就不需要将析构函数声明为虚函数。

###### 深入理解

首先我们先来理解一下什么是多态，多态性允许我们通过基类指针或引用来操作派生类对象。

这使得我们可以编写更通用、可扩展的代码，因为我们可以将派生类对象视为基类对象，并使用相同的接口处理它们。多态性主要通过虚函数实现。

将析构函数设计为虚函数主要是保证多态性，当我们使用基类指针或引用操作派生类对象时，如果基类的析构函数不是虚函数，此时则只会调用基类的析构函数，这样则有可能会导致未定义行为或者内存泄露。

但是当我们设置成虚函数时，并 override 之后，销毁派生类对象时，将自动调用相应的派生类析构函数，已实现派生类资源的释放。

```cpp
##include<iostream>
class Base {
public:
    Base() {
        std::cout << "Base constructor"<< std::endl;
    }
    virtual ~Base() {
        std::cout << "Base destructor"<< std::endl;
    }
};
class Derived : public Base {
public:
    Derived() {
        std::cout << "Derived constructor"<< std::endl;
    }
    ~Derived() {
        std::cout << "Derived destructor"<< std::endl;
    }
};
int main() {
    Base* basePtr = new Derived(); // 使用基类指针操作派生类对象
    delete basePtr; // 销毁对象时调用正确的派生类析构函数
    return 0;
}
```

## 34. 🌟 什么是 C++ 的函数重载？它的优点是什么？和重写有什么区别？

#### 回答重点

在 C++ 中，函数重载是指在同一个作用域内允许存在多个同名函数，它们的参数个数不同或者参数类型不同，注意函数的返回类型不同不能算作重载。

函数重载的优点是：

1. 增强了代码的可读性。使用同名的函数，而不用为不同的功能选择完全不同的函数名，程序员可以更直观地理解代码。
2. 改善了程序的可维护性。函数重载让我们可以定义一个通用接口，让同名函数实现不同的功能，减轻了函数命名的负担。

函数重载和函数重写（覆盖）的区别：

1. 函数重载可以发生在同一个类中，而函数重写发生在继承关系的子类中。
2. 函数重载要求参数列表必须不同，而函数重写要求方法签名（包括参数列表和返回类型）必须与父类方法一致。
3. 函数重载在编译时决定调用哪个函数（静态绑定），而函数重写在运行时决定调用哪个函数（动态绑定）。

#### 扩展知识

下面是函数重载的实例代码：

```c
##include <iostream>  
##include <string>  

// 重载print函数以打印整数  
void print(int i) {  
    std::cout << "Printing int: " << i << std::endl;  
}  
  
// 重载print函数以打印浮点数
void print(double f) {  
    std::cout << "Printing float: " << f << std::endl;  
}  
  
// 重载print函数以打印字符串  
void print(const std::string& s) {  
    std::cout << "Printing string: " << s << std::endl;  
}  
  
// 重载print函数以打印字符  
void print(char c) {  
    std::cout << "Printing char: " << c << std::endl;  
}  
  
int main() {    
    print(5);          // 调用打印整数的print  
    print(500.263);    // 调用打印浮点数的print  
    print("Hello");    // 调用打印字符串的print  
    print('A');        // 调用打印字符的print  
    return 0;  
}
```

函数重载在实际应用中非常广泛，比如标准库中的 `std::cout` 就是很多重载的运算符 `<<` 实现的，使我们可以打印不同类型的变量。而且，C++ STL（标准模板库）中的许多算法和容器类方法如 `std::sort` 和 `std::vector` 也采用了重载函数，以此来处理不同类型的输入。

除了函数重载和重写外，C++ 还支持运算符重载，允许对用户自定义的类型重载内置运算符，从而使自定义类型的使用和内置类型一样直观便捷。这个特性在实现复杂数据结构（如矩阵、复数等）时特别有用，使代码更易读、更自然。

<table>
    <tr>
    <td> 特性 <br/></td><td> 函数重载（Overload） <br/></td><td> 函数重写/覆盖（Override） <br/></td></tr>
    <tr>
    <td> 作用域 <br/></td><td>同一个类或作用域内<br/></td><td>继承关系的子类中重写父类的虚函数<br/></td></tr>
    <tr>
    <td> 函数签名要求 <br/></td><td>函数名相同， 参数列表不同 （参数类型或个数不同）<br/></td><td>函数名、参数列表、返回类型 必须完全相同 <br/></td></tr>
    <tr>
    <td> 返回类型 <br/></td><td>可以不同（但仅返回类型不同不构成重载）<br/></td><td>必须相同（协变返回类型除外）<br/></td></tr>
    <tr>
    <td> 绑定方式 <br/></td><td> 编译时 决定调用哪个函数（静态绑定）<br/></td><td> 运行时 决定调用哪个函数（动态绑定，需虚函数）<br/></td></tr>
    <tr>
    <td> 用途 <br/></td><td>提供同一功能的多种实现方式，适应不同参数类型<br/></td><td>子类修改或扩展父类的行为，实现多态<br/></td></tr>
    <tr>
    <td> 示例 <br/></td><td>```void print(int); void print(double);```<br/><br/></td><td>```virtual void draw() = 0; 在子类中重写 draw()```<br/><br/></td></tr>
    </table>




## 35. 🌟C++ 中 using 和 typedef 的区别？

#### 回答重点

`using` 在 C++11 中引入，`using` 和 `typedef` 都可以用来为已有的类型定义一个新的名称。最主要的区别在于，`using` 可以用来定义模板别名，而 `typedef` 不能。

1）`typedef` 主要用于给类型定义别名，但是它不能用于模板别名。

```cpp
typedef unsigned long ulong;
typedef int (*FuncPtr)(double);
```

2）`using` 可以取代 `typedef` 的功能，语法相对简洁。

```cpp
using ulong = unsigned long;
using FuncPtr = int (*)(double);
```

3）对于模板别名，`using` 显得非常强大且直观。

```cpp
template<typename T>
using Vec = std::vector<T>;
```

总之，更推荐使用 `using`，尤其是当你处理模板时。

#### 扩展知识

**1）模板别名（Template Aliases）：**`using` 在处理模板时，如定义容器模板别名，非常方便。假如我们需要一个模板类 `std::vector` 的别名：

```cpp
template<typename T>
using Vec = std::vector<T>;
Vec<int> vecInt; // 相当于 std::vector<int> vecInt;
```

**2）作用范围：**`using` 还可以用于命名空间引入，`typedef` 没有此功能。

```cpp
namespace LongNamespaceName {
    int value;
}

using LNN = LongNamespaceName;
LNN::value = 42; // 相当于 LongNamespaceName::value
```

**3）可读性与调试：**`using` 相对 `typedef` 更易读。

```cpp
typedef void (*Func)(int, double);
using FuncAlias = void(*)(int, double);
```

在这个例子中，`using` 显然定义和解释都更加直观。

**4）现代 C++ 代码规范：**在 C++11 之后，许多代码规范建议优先使用 `using` 而不是 `typedef`。这证明了在实际应用和代码维护中，`using` 更具有优势。

![image-20250511161633063](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511161633063.png)

## 36. 🌟C++ 中 map 和 unordered_map 的区别？分别在什么场景下使用？

#### 回答重点

两者都是常用的关联容器。但有一些区别：

**1）底层实现：**

- `map`：基于有序的红黑树（具体实现依赖于标准库）。
- `unordered_map`：基于哈希表。

**2）时间复杂度：**

- `map`：插入、删除、查找的时间复杂度为 O(log n)。
- `unordered_map`：插入、删除、查找的时间复杂度为 O(1)（摊销）。

**3）元素顺序：**

- `map`：元素按键值有序排列。
- `unordered_map`：元素无序排列。

**4）内存使用：**

- `map`：由于底层是红黑树，内存使用较少。
- `unordered_map`：需要额外的空间存储哈希表，但在处理大量数据时，可能具有更好的表现。

###### 场景选择

1）`map`：当需要按键值有序访问元素时，适合使用 `map`，例如按顺序遍历键值对。
2）`unordered_map`：当主要关注查找速度、不关心元素顺序时，使用 `unordered_map` 会更高效，例如需要高效的键值存储和快速查找的场景。

#### 扩展知识

**1）迭代器稳定性：**在 `map` 中，由于基于红黑树，其迭代器在插入和删除元素时通常依然有效（除了指向被删除元素的迭代器），但 `unordered_map` 中，插入和删除操作可能会使所有迭代器失效。

**2）复杂数据类型的键：**如果键是复杂数据类型（需要自定义比较函数），可以在 `map` 中利用自定义键比较器的排序规则：

```cpp
struct MyKey {
 int id;
 std::string name;
 bool operator<(const MyKey& other) const {
     return id < other.id; // 按id排序
 }
};
std::map<MyKey, int>m;
```

3）哈希函数的定制：在 `unordered_map` 中，如果键类型是用户自定义类型，需要自行提供哈希函数和比较器：

```cpp
struct MyKey {
 int id;
 std::string name;
};
struct HashFunction {
 std::size_t operator()(const MyKey& k) const {
     return std::hash<int>()(k.id) ^ std::hash<std::string>()(k.name);
 }
};
struct KeyEqual {
 bool operator()(const MyKey& lhs, const MyKey& rhs) const {
     return lhs.id == rhs.id && lhs.name == rhs.name;
 }
};
std::unordered_map<MyKey, int, HashFunction, KeyEqual> um;
```

![image-20250511161831525](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511161831525.png)

## 37. 什么是 C++ 中的 RAII？它的使用场景？

#### 回答重点

`RAII`，全称是 "Resource Acquisition Is Initialization"（资源获取即初始化）。

它的核心思想是将资源的获取与对象的生命周期绑定，通过构造函数获取资源（如内存、文件句柄、网络连接等），通过析构函数释放资源。这样，即使程序在执行过程中抛出异常或多路径返回，也能确保资源最终得到正确释放，特别是可以避免内存泄漏。

#### 扩展知识

**1）使用场景：**

- **内存管理：**标准库中的 `std::unique_ptr` 和 `std::shared_ptr` 是 RAII 的经典实现，用于智能管理动态内存。
- **文件操作：**`std::fstream` 类在打开文件时获取资源，在析构函数中关闭文件。
- **互斥锁：**`std::lock_guard` 和 `std::unique_lock` 用于在多线程编程中自动管理互斥锁的锁定和释放。

**2）示例代码：**

```cpp
##include <iostream>
##include <fstream>

class FileHandler {
public:
    FileHandler(const std::string& filename) : file(filename) { // 资源获取
        if (!file.is_open()) {
           throw std::runtime_error("Unable to open file");
        }
    }
    
    ~FileHandler() {
        file.close(); // 资源释放
    }
    
    void write(const std::string& data) {
        if (file.is_open()) {
            file << data << std::endl;
        }
    }

private:
    std::ofstream file;
};

int main() {
    try {
        FileHandler fh("example.txt");
        fh.write("Hello, RAII!");
    } catch (const std::exception& e) {
        std::cerr << e.what() << std::endl;
    }return 0;
}
```

在这个示例中，`FileHandler` 类在构造函数中打开文件，在析构函数中关闭文件。即使 `main` 函数中发生异常或提前返回，析构函数也会自动调用，确保文件被正确关闭。

**3）RAII 的好处：**

- **异常安全：**使用 RAII 能够确保在异常发生时自动释放资源，避免资源泄漏。
- **简化资源管理：**将资源的获取和释放逻辑封装在类内，使代码更加简洁且方便维护。

**4）与智能指针的结合：**

```cpp
std::unique_ptr<int> ptr(new int(5));
```

**5）扩展应用：**

- 锁管理：通过 `std::lock_guard` 对锁进行管理，确保锁在作用范围内被正确释放。

```cpp
std::mutex mtx;
{
   std::lock_guard<std::mutex> lock(mtx);
   // 临界区代码
} // mtx 在此处自动释放
```

![image-20250511161717071](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511161717071.png)


## 38. C++ 的 function、bind、lambda 都在什么场景下会用到？

#### 回答重点

三者都用于处理函数和可调用对象：

1）`std::function`：用于存储和调用任意可调用对象（函数指针、Lambda、函数对象等）。常用场景包括回调函数、事件处理、作为函数参数和返回值。

2）`std::bind`：用于绑定函数参数，生成函数对象，特别是当函数参数不完全时。常见于将已有函数适配为接口要求的回调、将成员函数与对象绑定。

3）Lambda 表达式：用于定义匿名函数，通常在短期和局部使用函数时比如一次性回调函数、算法库中的自定义操作等。

#### 扩展知识

###### std::function

`std::function` 在 C++11 中引入，它是一个类模板，用于封装任何形式的可调用对象。使用 `std::function` 可以很方便地存储各种不同类型的函数，以便后面调用。

**常见使用场景：**

1）回调函数：在图形用户界面程序或网络编程中，经常需要定义回调函数。

2）事件处理：在观察者模式中，可以用 `std::function` 存储和调用事件处理函数。

3）作为函数参数和返回值：方便传递函数或存储函数以在其他地方调用。

示例：

```cpp
##include <functional>
##include <iostream>
##include <vector>

void exampleFunction(int num) {
    std::cout << "Number: " << num << std::endl;
}

int main() {
    std::function<void(int)> 
    func = exampleFunction;func(42);
    return 0;
}
```

###### std::bind

`std::bind` 是一个函数模板，用于从一个可调用对象（如函数或成员函数）和其部分参数创建新的函数对象。这在处理不完全的函数参数或需要绑定特定对象的时候特别有用。

**常见使用场景：**

1）适配接口：当接口要求的函数签名与现有函数不匹配时，可以通过 `std::bind` 进行参数适配。

2）绑定成员函数：通过 `std::bind` 可以绑定类的成员函数与具体的实例对象，从而创建可以调用的对象。

示例：

```cpp
##include <functional>
##include <iostream>

void exampleFunction(int a, int b) {
    std::cout << "Sum: " << a + b << std::endl;
}

int main() {
    auto boundFunction = std::bind(exampleFunction, 10, std::placeholders::_1);
    boundFunction(32); // Output: Sum: 42
    return 0;
}
```

###### Lambda 表达式

Lambda 表达式是一种匿名函数，它可以在定义的地方直接使用，通常用于简单的计算。如果某个函数逻辑仅在某个特定范围内有用，使用 Lambda 表达式可以使代码更简洁。

**常见使用场景：**

1）一次性回调函数：与算法和容器一起使用，以简化代码。

2）自定义操作：在标准库算法（如 `std::for_each`, `std::transform` 等）中，使用 Lambda 表达式进行自定义操作。

**示例：**

```cpp
##include <algorithm>
##include <iostream>
##include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    std::for_each(vec.begin(), vec.end(), [](int &n) { n *= 2; });

    for (int n : vec) {
        std::cout << n << " ";
    }
    std::cout << std::endl;
    return 0;
}
```

## 39. 🌟C++ 中堆内存和栈内存的区别？

#### 回答重点

堆内存和栈内存的区别主要体现在分配方式、管理方式、生命周期和性能等方面：

**1）分配方式：**

- 栈内存：由编译器在程序运行时自动分配和释放。典型的例子是局部变量的分配。
- 堆内存：需要程序员手动分配和释放，使用 `new` 和 `delete` 操作符。在 C++11 之后，也可以使用智能指针来管理堆内存。

**2）管理方式：**

- 栈内存：由编译器自动管理，程序员无需担心内存泄漏，生命周期由作用域决定。
- 堆内存：由程序员手动管理，如果没有正确释放内存，会导致内存泄漏。

**3）生命周期：**

- 栈内存：变量在离开作用域之后自动销毁。
- 堆内存：只要不手动释放，内存会持续存在，直到程序终止。

**4）性能：**

- 栈内存：内存分配和释放速度极快，性能上优于堆内存。
- 堆内存：涉及到复杂的内存管理和分配机制，性能上较慢。

#### 扩展知识

**1）内存分配函数：**

- 除了 `new` 和 `delete`，堆内存还可以使用 `malloc` 和 `free` 来管理。区别在于 `new` 会调用构造函数，而 `malloc` 只是纯粹的内存分配。

**2）内存溢出和内存泄漏：**

- 内存溢出：栈空间是有限的，如果递归过深或者分配的局部变量太大，可能导致栈溢出。
- 内存泄漏：堆内存如果没有正确释放，会导致内存泄漏，尤其在长时间运行的程序中，会影响系统性能。

**3）智能指针：**

- C++11 引入了智能指针 `std::unique_ptr` 和 `std::shared_ptr`，可以自动管理堆内存，大大降低了内存泄漏的风险。

**4）虚拟内存：**

- 虚拟内存机制，使物理内存和逻辑内存独立，程序可以看到的是一个巨大的连续地址空间，但实际上可能是分散的物理内存和硬盘上的交换空间。

**5）栈与堆的容量：**

- 栈的容量往往较小，通常为几 MB，主要用于局部变量和函数调用管理。
- 堆的容量通常较大，依赖于系统可用内存，适合动态分配大量内存。

###### C++ 堆内存与栈内存对比表

![image-20250511161750399](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511161750399.png)



## 40. 什么是 C++ 的回调函数？为什么需要回调函数？


#### 回答重点

回调函数是一种通过函数指针或者函数对象（例如 `std::function` 或 lambda 表达式）将一个函数作为参数传递给另一个函数的机制。

实际上，就是把函数的调用权从一个地方转移到另一个地方，这个调用会在未来某个时刻进行，而不是立即执行。之所以称为“回调”，可以理解为某种倒叙执行：先安排好函数的调用，不立即执行，等到合适的时机再“回头”执行。

需要回调函数的主要原因包括：

**1）异步编程：**在异步操作中，比如网络请求、文件读取、事件处理等，可以在操作完成后调用回调函数，而主程序可以继续执行其它任务，避免等待操作完成。

**2）解耦代码：**回调函数有助于将代码模块化和解耦，允许我们创建更灵活和可复用的代码。例如，一个通用的排序算法可以接受一个比较函数，允许用户自定义排序逻辑。

**3）事件驱动编程：**在 GUI 或者其他事件驱动程序中，回调函数经常用于处理用户输入事件，如点击、鼠标移动、键盘输入等。

#### 扩展知识

回调函数的实际应用：

**1）使用函数指针作为回调函数：**

在 C 风格接口中，最常见的回调函数形式就是使用函数指针。例如：

```cpp
##include <iostream>

// 定义一个函数指针类型
typedef void (*CallbackFunc)(int);

void RegisterCallback(CallbackFunc cb) {
 // 模拟某些操作
 std::cout << "Registering callback...\n";
 cb(42); // 调用回调函数
}

void MyCallback(int value) {
 std::cout << "Callback called with value: " << value << std::endl;
}

int main() {
 RegisterCallback(MyCallback); // 传递回调函数
 return 0;
}
```

**2）使用 C++11 之后的 **std::function 和 lambda 表达式：

```cpp
##include <iostream>
##include <functional>

void RegisterCallback(
 std::function<void(int)> cb) {
 std::cout << "Registering callback...\n";
 cb(42); // 调用回调函数
}

int main() {
 auto myCallback = [](int value) {
     std::cout << "Callback called with value: " << value << std::endl;
 };
 RegisterCallback(myCallback); // 传递 lambda 回调函数
 return 0;
}
```

**3）GUI 编程中的回调：**

在图形用户界面编程中，回调函数常用于处理用户事件。例如，在一个按钮点击事件中调用用户提供的回调函数。

例如，使用一个假设的 GUI 库：

```cpp
class Button {
 public:
 void setOnClick(std::function<void()> cb) {
     onClick = cb;
 }

void simulateClick() {
    if (onClick) {
        onClick();
     }
 }
 
private:
 std::function<void()> onClick;
};

int main() {
 Button button;
 button.setOnClick([]() {
     std::cout << "Button clicked!" << std::endl;
 });
 button.simulateClick(); // 模拟一次点击事件
 return 0;
}
```


## C++ 面试真题（41-50）

## 41. 🌟C++ 中为什么要使用 nullptr 而不是 NULL？

#### 回答重点

主要原因是 `nullptr` 有明确的类型，它是 `std::nullptr_t` 类型，可以避免代码中出现类型不一致的问题。

#### 扩展知识

**1）类型安全：**`NULL` 通常被定义为数字 `0`（在 C++ 代码中一般是 `##define NULL 0`），它实际上是整型值。这就可能会带来类型不一致的问题，比如传递参数时，编译器无法准确判断是整数 `0` 还是空指针。而 `nullptr` 则是 `std::nullptr_t` 类型的，能够明确表示空指针，使编译器更容易理解代码。

**2）代码可读性：**使用 `nullptr` 使得代码更具有可读性和可维护性。它明确传达了变量是用作指针而非整数值，例如：

```cpp
void process(int x) {
    std::cout << "Integer: " << x << std::endl;
}

void process(voidptr) {
    std::cout << "Pointer: " << ptr << std::endl;
}

int main() {
    process(NULL);     // int 还是指针？
    process(nullptr);  // 指针
    return 0;
}
```

在上面的代码中，可以看出 `nullptr` 能让编译器和程序员清楚地知道调用哪个函数。

**3）避免潜在的错误：**在函数重载和模板中使用 `NULL` 可能导致编译器选择错误的重载版本。另外，模板编程中特别是涉及类型推断时，`NULL` 会带来一些不期望的效果。

```cpp
template<typename T>
void foo(T x) {
    std::cout << typeid(x).name() << std::endl;
}

int main() {
    foo(0);         // 0 是int型
    foo(NULL);      // 你希望是int还是指针呢
    foo(nullptr);   // std::nullptr_t
    return 0;
}
```

在上面的代码中，使用 `nullptr` 可以让我们精确控制模板的类型。

###### **C++ 中** `nullptr` **与** `NULL` **对比表**

![image-20250511162201399](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511162201399.png)



## 42. 🌟C++ 中什么是深拷贝？什么是浅拷贝？

#### 回答重点

**1）浅拷贝：**浅拷贝只是简单地复制对象的值，而不复制对象所拥有的资源或内存。也就是说，两个对象共享同一个资源或内存。当一个对象修改了该资源或内存，另一个对象也会受到影响。这种情况通常发生在默认的拷贝构造函数或赋值操作中。

**2）深拷贝：**深拷贝不仅复制对象的值，还会新分配内存并复制对象所拥有的资源。这样两个对象之间就不会共享同一个资源或内存，修改其中一个对象的资源或内存不会影响到另一个对象。

## 43. C++ 中友元类和友元函数有什么作用？

#### 回答重点

两者主要用于提供访问私有成员和保护成员的权限。

友元关系是一种单向的访问权限，并不会破坏封装性，同时也不会牵涉到类之间的继承关系。友元的使用在以下情况下特别有用：

**1） 友元函数：**允许一个函数访问某个类的私有成员和保护成员。

```cpp
class MyClass {
 private:
 int privateMember;

 public:
 MyClass() : privateMember(0) {}
 //声明友元函数friend void 
 friendFunction(MyClass &obj);
};

void friendFunction(MyClass &obj) {
//访问 privateMember
 obj.privateMember = 10;
}
```

**2） 友元类：**允许另一个类访问某个类的私有成员和保护成员。

```cpp
class B; //前向声明

class A {
 private:
 int privateMember;
 
 public:
 A() : privateMember(0) {}
 //声明B为友元类
 friend class B;
};

class B {
 public:
 void accessA(A &obj) {
     //访问 A 的 privateMember
     obj.privateMember = 20;
 }
};
```

#### 扩展知识

下面进一步讨论下它们的作用场景和设计考量：

**1） 封装与开放：**

- 封装是面向对象编程的基本原则之一，它将数据和操作数据的方法绑定到一起，防止外部代码直接访问对象的内部状态。友元的引入让类在需要的时候能够部分地开放它的内部状态，通常不会滥用。
- 友元函数和友元类提供了一种在不破坏封装性的条件下，安全访问私有成员的方式。

**2） 友元的替代方案：**

- 如果友元机制的使用本质上意味着违反封装性或设计初衷，那么可能需要重新考量类的设计。
- 你可以选择通过公开接口提供访问权限（如 getter/setter 方法），或利用继承、多态等其他 OOP 特性来实现同样的目的。

**3） 访问控制复杂度：**

- 使用友元可能会增加代码的复杂度，因为它打破了类的封装性，代码的维护变得相对困难。所以，在维护代码时，需要非常小心，确保友元使用的合理性和必要性。

友元是一种方便但需要慎用的工具，合理使用能够简化代码，但滥用则会破坏类的封装性，增加代码维护的难度。建议在实际编程中能够权衡利弊，合理利用这一机制。

## 44. C++ 如何调用 C 语言的库？

#### 回答重点

可以使用 `extern "C"` 来告诉编译器按照 C 语言的链接方式处理某些代码：

1）在 C++ 代码中包含 C 语言头文件时，用 `extern "C"` 进行声明，比如：

```cpp
extern "C" {
    ##include "your_c_library.h"
}
```

2）需要在链接阶段确保 C++ 项目和 C 语言库都被正确链接。可通过编写合适的 CMakeLists.txt 或 Makefile 来实现。

3）也可以不使用 extern "C"，源文件后缀名改为.c 也行。

#### 扩展知识

说到 `extern "C"`，得从 C 和 C++ 的兼容性说起。C++ 是 C 的增强版本，但它们的编译方式还是有些差异的。C++ 支持函数的重载，而 C 语言不支持。C++ 编译器会对函数进行“名字修饰”（Name Mangling）。

`extern "C"` 的作用是让编译器按 C 方式编译，避免函数名被修饰，保证 C 语言库里的函数能被正确调用。

举个例子：
假设有一个简单的 C 库 `math_library.c`：

```c
// math_library.c
int add(int a, int b) {
    return a + b;
}
```

你先编写一个头文件 `math_library.h`：

```c
// math_library.h
##ifndef MATH_LIBRARY_H
##define MATH_LIBRARY_H
int add(int a, int b);
##endif
```

然后在你的 C++ 项目中这么用：

```cpp
// main.cpp
##include <iostream>
extern "C" {
    ##include "math_library.h"
}

int main() {
    int result = add(3, 4);
    std::cout << "Result: " << result << std::endl;
    return 0;
}
```

最后，确保编译和链接。可以使用以下命令：

```shell
g++ -o main main.cpp math_library.c
```

另外，有几点需要注意：

1）如果你的 C 库里有 C++ 不支持的特性，比如变量长度数组（VLA），需要仔细考虑兼容性。

2）如果 C 库包含了结构体，尤其是那些带有复杂数据类型或指针的结构体，要确保它们在 C++ 中能够正确处理。

3）最好是 C 和 C++ 不要混用，如果要混用，建议做一个封装层，对 C 做一层 C++ 的封装，然后上层的业务代码还是统一使用 C++。

## 45. 指针和引用的区别

#### 回答重点

1. 引用必须在声明时初始化，指针可以不需要初始化

```cpp
// 引用示例
int a = 10;
int& ref_a = a; // 引用初始化
// int& ref_b; // 错误：引用必须在声明时初始化
// 指针示例
int b = 20;
int* ptr_b = &b; // 指针初始化
int* ptr_c = nullptr; // 指针可以为空
```

1. 指针是一个变量，存储的是一个地址，引用跟原来的变量实质上是同一个东西，是原变量的别名，引用本身并不存储地址，而是在底层通过指针来实现对原变量的访问
2. 引用被创建之后，就不可以进行更改，指针可以更改

```cpp
##include<iostream>
int main() {
    int a = 10;
    int b = 20;
    // 引用
    int& ref_a = a; // ref_a 是 a 的引用
    // ref_a = b; // 错误：引用不能被重新绑定
    // 指针
    int* ptr_a = &a; // ptr_a 是一个指针，指向 a
    ptr_a = &b; // 指针可以被重新赋值，现在指向 b
    std::cout << "a: " << a << ", b: " << b << std::endl;
    // 如果取消注释 ref_a = b; 这行代码，将会导致编译错误
    // 输出 "a: 10, b: 20"，因为引用不能被重新绑定
    *ptr_a = 30; // 通过指针修改 b 的值
    std::cout << "a: " << a << ", b: " << b << std::endl; // 输出 "a: 10, b: 30"
    return 0;
}
```

1. 不存在指向空值的引用，必须有具体实体；但是存在指向空值的指针。

## 46. public 继承、protected 继承、private 继承的区别

#### 回答重点

重点说明三种继承方式的区别：

<table>
<tr>
<td><br/></td><td>public继承<br/></td><td>protected继承<br/></td><td>private继承<br/></td></tr>
<tr>
<td>基类public成员<br/></td><td>子类中仍然是public<br/></td><td>子类中变为protected<br/></td><td>子类中变为private<br/></td></tr>
<tr>
<td>基类protected成员<br/></td><td>子类中仍然是protected<br/></td><td>子类中仍然是protected<br/></td><td>子类中变为private<br/></td></tr>
<tr>
<td>基类private成员<br/></td><td>子类不可访问<br/></td><td>子类不可访问<br/></td><td>子类不可访问<br/></td></tr>
<tr>
<td>使用场景<br/></td><td>最常用的is-a场景，允许外部访问基类的public接口。<br/></td><td>避免基类的成员不被外部访问，子类的子类还有机会可以访问。<br/></td><td>外部无法访问基类的任何成员。<br/></td></tr>
</table>


## 47. 什么是静态数据成员和静态成员函数？

在 C++ 里，静态数据成员和静态成员函数是类的特殊成员，下面分别介绍它们：

#### 静态数据成员

静态数据成员是用 `static` 关键字修饰的类的数据成员。它不属于类的某个具体对象，而是被类的所有对象共享，在内存中只有一份拷贝。

所有对象都能访问和修改同一个静态数据成员，可用于记录类相关的公共信息。

必须在类外进行初始化，且初始化时不使用 `static` 关键字。

```cpp
##include <iostream>
class MyClass {
public:
    static int staticData; // 声明静态数据成员
};

// 在类外初始化静态数据成员
int MyClass::staticData = 10;

int main() {
    MyClass obj1, obj2;
    std::cout << "obj1的静态数据成员值: " << obj1.staticData << std::endl;
    std::cout << "obj2的静态数据成员值: " << obj2.staticData << std::endl;

    // 修改静态数据成员的值
    obj1.staticData = 20;
    std::cout << "修改后obj2的静态数据成员值: " << obj2.staticData << std::endl;

    return 0;
}
```

在这个例子中，`staticData` 是 `MyClass` 类的静态数据成员，`obj1` 和 `obj2` 共享这一成员，修改 `obj1` 的 `staticData` 后，`obj2` 的 `staticData` 也会改变。

#### 静态成员函数

静态成员函数是用 `static` 关键字修饰的类的成员函数，它不依赖于类的具体对象，可直接通过类名调用。

没有 `this` 指针，因为它不与特定对象关联，不能访问非静态数据成员和非静态成员函数。

主要用于处理类的静态数据成员，提供与类相关的通用功能。

```cpp
##include <iostream>
class MyClass {
private:
    static int staticData;
public:
    static void setStaticData(int value) {
        staticData = value;
    }
    static int getStaticData() {
        return staticData;
    }
};

// 在类外初始化静态数据成员
int MyClass::staticData = 0;

int main() {
    // 直接通过类名调用静态成员函数
    MyClass::setStaticData(30);
    std::cout << "静态数据成员的值: " << MyClass::getStaticData() << std::endl;

    return 0;
}
```

![image-20250511162217426](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511162217426.png)



## 48. 静态成员变量为什么要在类外初始化？

#### 回答重点

1. **存储空间分配**：

   - 因为静态变量不属于任何特定对象，而是类的共享成员，需要在全局范围内分配存储空间。静态变量的存储空间在程序启动时分配，生命周期贯穿整个程序运行。类内声明仅告知编译器该变量的存在，而类外定义则实际分配内存。
2. **避免重复定义**：

   - 如果在类内初始化静态变量，可能会导致多个编译单元中包含该变量的定义，导致链接错误。类外定义确保静态变量只在一个编译单元中定义，避免重复。

#### 扩展知识

1. **静态成员函数**：

   - 静态成员函数只能访问静态成员变量，不能访问非静态成员变量，因为它们没有 `this` 指针。
2. **静态常量成员**：

   - 静态常量成员（如 `static const int`）可以在类内直接初始化，因为它们在编译时已知，并且不需要额外的存储空间。
3. **内联静态成员**：

   - C++17 引入了内联静态成员，可以在类内直接初始化静态变量，编译器会自动处理存储和链接问题。

```cpp
class MyClass {
public:
    static int staticVar; // 类内声明
    static const int staticConstVar = 10; // 静态常量整型成员，类内初始化
};

int MyClass::staticVar = 0; // 类外定义和初始化

int main() {
    MyClass::staticVar = 5; // 访问静态变量
    return 0;
}
```






## 49. 🌟C++ 动态库和静态库的区别？

#### 回答重点

动态库（也称为共享库）和静态库是常用的两种库形式，它们有以下主要区别：

1）动态库在运行时加载，提供共享的库文件，如 `.dll`（Windows）或 `.so`（Linux），而静态库在编译时被直接合并到可执行文件中，通常是 `.lib`（Windows）或 `.a`（Linux）。

2）动态库在内存中可以被多个程序共享，因此节省了内存和磁盘空间，但会引入一些加载开销。对于静态库，每个使用它的程序都会有一份拷贝，二进制文件较大，但启动速度通常较快。

3）**重点：**动态库可以在库升级时**无需重新编译**依赖该库的程序，只需要更新库文件即可。而静态库由于依赖程序直接包含了库的代码，所以**需要重新编译**依赖程序进行更新。

4）在调试和部署方面，动态库更复杂，因为它们需要在运行时找到所需的库文件。如果库文件不存在或版本不匹配，程序可能无法正常运行。静态库则不会有这种问题，因为所有依赖都已经编译进了可执行文件。

#### 扩展知识

除了上面提到的主要区别，动态库和静态库在开发过程中还有一些其他注意事项和优化技巧，可以进一步了解：

**1）符号解析：**静态链接在编译时就解析了符号，这意味着所有函数和变量的引用都在编译期解决，而动态链接在运行时解析符号，这会略微增加加载时间。

**2）版本控制：**动态库通常可以使用版本号来管理不同版本的库同时存在，例如在 Linux 系统中，库文件名中可以包含版本号，如 `libxyz.so.1.2.3`。这可以允许旧版程序继续使用旧版本的库，而新版程序使用新版本的库。

**3）编译选项：**在创建动态库时，通常需要使用如 `-fPIC`（Position Independent Code，位置无关代码）选项来生成可以在任意内存地址运行的代码。而静态库通常不需要考虑这一点，因为它们最终会被编译进可执行文件。

**4）链接顺序：**在链接静态库时，链接顺序可能会影响到最终的可执行文件，尤其是在处理有依赖关系的多个库时。动态库则相对宽松，因为它们的依赖关系可以在运行时解决。

**5）打包和分发：**动态库通常需要一并打包分发，确保目标系统中存在正确版本的库。常见的方法包括使用容器（如 Docker）或者打包系统（如 Windows 安装包及 Linux 的 `.deb` 和 `rpm` 包）。

![image-20250511162229101](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511162229101.png)



## 50. 🌟 介绍下 C++ 程序从编写到可执行的整个过程？

#### 回答重点

总共分 5 步：

1）编写代码：编写 C++ 源代码，保存为 `.cpp` 、`.cc` `.h` 文件。

2）预处理：预处理器根据源代码中的预处理指令（如 `##include` 替换、`##define` 替换等）对代码进行处理，生成纯净的源代码。

3）编译：编译器（如 `g++` 或 `clang++`）将预处理后的源代码翻译成汇编代码。

4）汇编：汇编器（如 `as`）将汇编代码转换成机器码，生成目标文件（`.o` 文件）。

5）链接：链接器（如 `ld`）将多个目标文件和库文件链接在一起，生成最终的可执行文件。

#### 扩展知识

下面深入理解各个步骤：

**1）编写代码**

开发者主要参与的就是这个步骤，通过 C++ 编写逻辑业务代码，这部分代码是高级语言编写的，人类易读易写。

**2）预处理**

预处理器主要做以下工作：

- 处理头文件：如 `##include` 指令会替换为头文件内容。
- 宏替换：如 `##define` 指令用相应的代码替换宏。
- 条件编译：根据条件指令（如 `##ifdef`、`##ifndef` 等）决定是否编译部分代码。
- 去除注释：清理掉所有注释。
- 添加行号：添加行号和文件名标识，方便编译器产生警告和调试信息

预处理的输出仍是 C++ 代码，但没有了预处理指令。

**3）编译**

编译器将预处理后的代码转化为汇编代码，主要做以下工作：

- 词法分析：语法扫描，利用有限状态机的算法将源码中的字符串分割成一系列记号，如加减乘除数字括号等。
- 语法分析：检查代码语法，将源代码转为语法树。
- 语义分析：检查变量类型、函数调用是否合法等，比如浮点型整数赋值给指针，编译器就会报错。
- 优化：对中间代码进行优化，提高代码运行效率，比如 3+4=7。
- 生成汇编：根据优化后的中间代码生成汇编代码。

汇编代码是与硬件无关的低级代码。

**4）汇编**

汇编器将汇编代码转化为与目标机器相关的机器码，生成目标文件。每个 `.cpp` 源文件（模块）通常会生成一个对应的 `.o` 目标文件。这些文件包含机器指令以及一些符号表信息，用于后续链接。

**5）链接**

链接器将所有目标文件和所需的库文件链接起来，生成最终的可执行文件。主要做以下工作：

- 符号解析：找到所有外部符号（如函数、变量）在目标文件和库文件中的定义。
- 地址分配：将目标代码放置在内存中的适当位置。
- 重定位：调整程序内所有的地址引用，使它们指向正确的内存位置。

![](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511162254200.png)


## C++ 面试真题（51-60）

## 51. 🌟 谈一谈你对面向对象的理解

#### **回答重点**

四大核心特性（3 + 1）：

1. **封装**：将数据（属性）和操作数据的方法（函数）绑定到类中，通过访问控制（`public`/`private`/`protected`）隐藏实现细节。可提高安全性（防止外部直接修改数据），简化接口使用。
2. **继承：**通过派生类（子类）复用基类（父类）的代码，来支持层次化设计。
3. **多态：**同一接口在不同上下文中表现出不同行为，分为编译时多态（函数重载、模板）和运行时多态（虚函数）。运行时多态主要是通过虚函数表和虚函数指针，实现动态绑定。
4. **抽象：**仅暴露必要接口，隐藏复杂实现细节。主要通过纯虚函数定义抽象类（接口）。

#### **扩展知识**

- **菱形继承**：C++ 支持一个类继承多个基类，但需注意菱形继承问题（通过虚继承解决）：

  ```cpp
  class A {};
  class B : virtual public A {};
  class C : virtual public A {};
  class D : public B, public C {}; // 虚继承避免 A 的重复拷贝
  ```

- **虚函数与多态成本**：虚函数通过虚函数表实现，会带来额外内存和间接调用开销，但提供了灵活的运行时多态。

![image-20250511162010139](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511162010139.png)


## 52. 🌟介绍下C++常用的容器以及特点？

如下表：

<table>
<tr>
<td>容器<br/></td><td>特点<br/></td><td>时间复杂度<br/></td><td>适用场景<br/></td></tr>
<tr>
<td>vector<br/></td><td>动态数组，随机访问快<br/></td><td>尾部操作 O(1)，中间 O(n)<br/></td><td>需要随机访问，尾部操作多<br/></td></tr>
<tr>
<td>list<br/></td><td>双向链表，插入/删除快<br/></td><td>插入/删除 O(1)，访问 O(n)<br/></td><td>频繁中间插入/删除<br/></td></tr>
<tr>
<td>deque<br/></td><td>双端队列，两端操作高效<br/></td><td>头尾操作 O(1)<br/></td><td>需要两端操作和随机访问<br/></td></tr>
<tr>
<td>set/map<br/></td><td>红黑树实现，有序<br/></td><td>操作 O(log n)<br/></td><td>需要有序且快速查找<br/></td></tr>
<tr>
<td>unordered_set/map<br/></td><td>哈希表实现，无序，查找快<br/></td><td>平均 O(1)，最坏 O(n)<br/></td><td>需要快速查找，不关心顺序<br/></td></tr>
<tr>
<td>stack/queue<br/></td><td>适配器，限制操作<br/></td><td>依赖底层容器<br/></td><td>需要特定数据结构（LIFO/FIFO）<br/></td></tr>
</table>



## 53. 🌟详细介绍C++中的`constexpr`

#### 回答重点

`constexpr`是C++11引入的关键字，用于声明常量表达式，在编译时就能确定值的表达式，两个用途：

- 编译时计算：编译时就可以计算出表达式的值，提高运行时性能

- 常量表达式上下文：可用于哪些需要常量的表达式，比如数组大小、模板参数等。

#### 扩展知识

1）`constexpr`变量：声明时必须初始化，且初始值必须是常量表达式。

```cpp
constexpr int size = 10; // size 是编译时常量
int arr[size];
```

2）`constexpr` 函数：在编译期求值的函数，它的参数必须是常量表达式。

```cpp
constexpr int factorial(int n) {
    return n <= 1 ? 1 : n * factorial(n - 1);
}
constexpr int val = factorial(5); // val 在编译时计算为 120
```

3）`constexpr` 与 `const` 的区别：

- `const`：运行时不可修改，但不保证编译时求值
- `constexpr`：编译时求值

<table>
    <tr>
    <td> 对比项 <br/></td><td>`constexpr`<br/></td><td>`const`<br/></td></tr>
    <tr>
    <td> 求值时机 <br/></td><td>编译时<br/></td><td>运行时（可能编译时，但不强制）<br/></td></tr>
    <tr>
    <td> 初始化要求 <br/></td><td>必须用常量表达式初始化<br/></td><td>可用运行时表达式初始化<br/></td></tr>
    <tr>
    <td> 函数支持 <br/></td><td>可修饰函数（编译时执行）<br/></td><td>仅修饰变量/成员函数（不改变求值时机）<br/></td></tr>
    <tr>
    <td> 优化潜力 <br/></td><td>更高（编译期计算消除运行时开销）<br/></td><td>无额外优化<br/></td></tr>
    <tr>
    <td> 适用版本 <br/></td><td>C++11+<br/></td><td>所有版本<br/></td></tr>
    </table>



## 54. 🌟 请介绍 C++ 多态的实现原理？

#### 回答重点

整体流程如下：

回答多态的实现原理，主要可以围绕在 `虚函数`、`虚函数表` 和 `虚函数表指针` 方向上。

多态通过虚函数实现。通过虚函数，子类可以重写父类的方法，当通过基类指针或引用调用时，会根据对象的实际类型调用对应的函数实现。

而这更深层次的原理，是通过虚表（vtable）和虚表指针（vptr）机制实现的。虚表是一个函数指针数组，包含了该类所有虚函数的地址，而虚表指针存储在对象实例中，指向属于该对象的虚表。

#### 扩展知识

可以从以下几个方面，更全面的了解多态：

**1）虚函数和重写：**在基类中使用关键字 `virtual` 声明虚函数后，在子类中可以重写这个函数。

```cpp
class Base {
public:
 virtual void show() {
     std::cout << "Base show" << std::endl;
 }
};

class Derived : public Base {
public:
 void show() override {
     std::cout << "Derived show" << std::endl;
 }
};
```

**2）虚表（vtable）：**每个包含虚函数的类都会有一个虚表（vtable），这个虚表在编译时生成。它包含了该类所有虚函数的指针。对于每个类（而不是每个对象），编译器会创建一个唯一的虚表。

**3）虚表指针（vptr）：**每个包含虚函数的对象实例会有一个隐藏的虚表指针（vptr），它在对象创建时自动初始化，指向该类的虚表。不同类型的对象，其虚表指针会指向不同的虚表。例如，上述示例中，`Base` 和 `Derived` 对象的虚表指针分别指向它们各自的虚表。

实际如图所示：

**4）多态的调用机制：**当通过基类指针或引用调用虚函数时，程序会通过该指针或引用找到对应的对象，然后通过虚表指针找到正确的虚表中的函数地址，最终调用适当的函数实现，这样程序能够在运行时决定调用哪一个函数实现。

**5）实际示例：**

```cpp
void demonstratePolymorphism(Base &obj) {
 obj.show();  // 依赖于实际对象的类型
}

int main() {
 Base b;
 Derived d;
 demonstratePolymorphism(b);  // 输出 "Base show"
 demonstratePolymorphism(d);  // 输出 "Derived show"

 return 0;
}
```

**6）注意事项：**

- 使用多态会有一定的内存和性能开销，因为每个类需要维护虚表，每个对象也需要存储虚表指针。
- 虚函数调用通常比普通函数调用更慢一点，因为多了一次指针间接寻址。

## 55. 🌟 请介绍 C++ 中 unique_ptr 的原理？

#### 回答重点

`unique_ptr` 是 C++11 引入的智能指针，它利用 `RAII` 模式，自动管理动态分配的资源。主要特点是它的所有权是独占的，也就是说，在任意时刻，某块内存只能由一个 `unique_ptr` 实例拥有，这样可以确保资源不会被多次释放。

基本原理如下：

1）`unique_ptr` 一旦被创建，它会负责管理内存并在适当的时候释放资源。

2）`unique_ptr` 不允许复制，但可以通过 `std::move` 进行所有权转移，从而避免双重释放问题。

3）`unique_ptr` 采用 `RAII` 模式，即在对象的生命周期内自动管理资源的获取和释放。

它为什么可以做到独占？

可以直接看这段[源码](https://gcc.gnu.org/onlinedocs/libstdc++/libstdc++-html-USERS-4.4/a01404.html)：

```c
unique_ptr(const unique_ptr&) = delete;

template<typename _Up, typename _Up_Deleter> 
unique_ptr(const unique_ptr<_Up, _Up_Deleter>&) = delete;

unique_ptr& operator=(const unique_ptr&) = delete;

template<typename _Up, typename _Up_Deleter> 
unique_ptr& operator=(const unique_ptr<_Up, _Up_Deleter>&) = delete;
```

因为它禁用了拷贝构造函数。

#### 扩展知识

进一步探讨 `unique_ptr` 的使用和内部机制。

**1）创建** `unique_ptr`：

```cpp
std::unique_ptr<int> ptr1(new int(10));
auto ptr2 = std::make_unique<int>(20);
```

**2）所有权转移：**使用 `std::move` 转移 `unique_ptr` 的所有权。

```cpp
std::unique_ptr<int> ptr3 = std::move(ptr1);
```

**3）不可复制：**尝试复制 `unique_ptr` 会导致编译错误。

```cpp
// std::unique_ptr<int> ptr4 = ptr3; // 编译错误
```

**4）析构：**当 `unique_ptr` 离开作用域时，其析构函数会自动调用 `delete`，释放内存。

```cpp
void example() {std::unique_ptr<int> ptr(new int(30));// 离开作用域时， ptr 会自动释放内存
}
```

5）自定义删除器：可以为 `unique_ptr` 指定一个自定义删除器。

```cpp
std::unique_ptr<FILE, decltype(&fclose)> file_ptr(fopen("file.txt", "r"), &fclose);
```

6）优化性能：通过使用 `std::make_unique` 减少内存分配和构造步骤，提高性能和安全性。

```cpp
auto arr = std::make_unique<int[]>(5);
```

因为这些特性， `unique_ptr` 非常适合用来管理动态资源，防止常见的内存泄漏和其他资源管理问题。

而且，使用 `unique_ptr` 还可以使代码更加清晰简洁，减少手动资源管理的负担。

建议在 C++ 开发中，尽量避免使用裸指针，考虑使用智能指针完全替代裸指针。

<table>
    <tr>
    <td> 对比项 <br/></td><td>`unique_ptr`<br/></td><td>`shared_ptr`<br/></td><td>裸指针<br/></td></tr>
    <tr>
    <td> 所有权 <br/></td><td>独占（不可复制）<br/></td><td>共享（引用计数）<br/></td><td>无明确所有权<br/></td></tr>
    <tr>
    <td> 开销 <br/></td><td>无额外内存开销（仅存指针）<br/></td><td>含引用计数和原子操作开销<br/></td><td>无开销<br/></td></tr>
    <tr>
    <td> 安全性 <br/></td><td>自动释放，防内存泄漏<br/></td><td>自动释放，循环引用需注意<br/></td><td>需手动管理，易泄漏<br/></td></tr>
    <tr>
    <td> 拷贝语义 <br/></td><td>仅移动（`std::move`）<br/></td><td>允许拷贝<br/></td><td>可随意拷贝<br/></td></tr>
    <tr>
    <td> 推荐场景 <br/></td><td>明确独占所有权的资源<br/></td><td>需共享所有权的资源<br/></td><td>不推荐使用<br/></td></tr>
    </table>



## 56. 🌟 请介绍 C++ 中 shared_ptr 的原理？shared_ptr 线程安全吗？

#### 回答重点

**1）原理：**`shared_ptr` 原理的回答重点在于 `引用计数`，它在底层实现上，通过维护一个引用计数，来管理内存对象的生命周期。

当新构造一个对象时，引用计数初始化为 1，拷贝对象时，引用计数加 1，对象作用域结束析构时，引用计数减 1，当最后一个对象被销毁时，引用计数会减为 0，所持有的资源会被释放。

**2）线程安全性：**`shared_ptr` 保证多个线程能够安全地增加或减少其引用计数。但是，如果多个线程同时读写同一个 `shared_ptr` 或者操作其管理的对象，那么需要额外进行同步机制，比如使用互斥锁（`mutex`）来保护这些操作。

也就是说，`shared_ptr` 的引用计数是线程安全的，但是它管理的对象是否线程安全，不归 `shared_ptr` 来管，取决于相关的对象是否有做同步处理。

#### 扩展知识

**1）引用计数机制：**`shared_ptr` 内部通常会包含两部分指针：一是指向实际管理的资源，二是指向一个控制块（control block）。控制块中包含引用计数，当 `shared_ptr` 被拷贝时，引用计数增加；当 `shared_ptr` 被销毁时，引用计数减少；当引用计数变为零时，资源才会被释放。

**2）线程安全：**具体来说，`shared_ptr` 的引用计数操作是线程安全的。这是因为标准库对引用计数的增减操作进行了原子化处理。但是在其他场景下，比如多个线程同时修改 `shared_ptr` 对象自身，依然需要使用锁来保护。

**3）循环引用问题：**需要注意的是，`shared_ptr` 可能会导致循环引用（circular reference）的情况。比如 A 持有 B 的 `shared_ptr`，B 又持有 A 的 `shared_ptr`，这会导致引用计数永远不会为零，资源无法正确释放。解决这种问题，可以引入 `weak_ptr`。`weak_ptr` 只会弱引用资源，不会影响引用计数。

**4）性能开销：**由于每次创建或销毁 `shared_ptr` 都会涉及到控制块的分配和释放操作，所以在一些性能敏感的场景（比如频繁创建和销毁对象）下，需要估算好这些操作的开销，在智能指针的选择上面，可优先选择 `unique_ptr`，次之选择 `shared_ptr`。

**5）手写一个 shared_ptr**

```
#include <iostream>
include <mutex>

// 引用计数控制块
template <typename T>
class ControlBlock {
public:
    ControlBlock(T* ptr) : ptr_(ptr), ref_count_(1), weak_count_(0) {}
    
    void add_ref() {
        std::lock_guard<std::mutex> lock(mutex_);
        ++ref_count_;
    }
    
    void release_ref() {
        bool should_delete = false;
        {
            std::lock_guard<std::mutex> lock(mutex_);
            if (--ref_count_ == 0) {
                should_delete = true;
                if (weak_count_ == 0) {
                    delete ptr_;
                    ptr_ = nullptr;
                }
            }
        }
        if (should_delete && weak_count_ == 0) {
            delete this;
        }
    }
    
    void add_weak_ref() {
        std::lock_guard<std::mutex> lock(mutex_);
        ++weak_count_;
    }
    
    void release_weak_ref() {
        bool should_delete = false;
        {
            std::lock_guard<std::mutex> lock(mutex_);
            if (--weak_count_ == 0 && ref_count_ == 0) {
                should_delete = true;
            }
        }
        if (should_delete) {
            delete this;
        }
    }
    
    T* get() const { return ptr_; }
    int use_count() const { return ref_count_; }
    
private:
    T* ptr_;
    int ref_count_;
    int weak_count_;
    mutable std::mutex mutex_;
};

// shared_ptr 主类
template <typename T>
class SharedPtr {
public:
    // 构造函数
    SharedPtr() : ctrl_block_(nullptr) {}
    
    explicit SharedPtr(T* ptr) : ctrl_block_(new ControlBlock<T>(ptr)) {}
    
    // 拷贝构造函数
    SharedPtr(const SharedPtr& other) : ctrl_block_(other.ctrl_block_) {
        if (ctrl_block_) {
            ctrl_block_->add_ref();
        }
    }
    
    // 移动构造函数
    SharedPtr(SharedPtr&& other) noexcept : ctrl_block_(other.ctrl_block_) {
        other.ctrl_block_ = nullptr;
    }
    
    // 析构函数
    ~SharedPtr() {
        if (ctrl_block_) {
            ctrl_block_->release_ref();
        }
    }
    
    // 赋值运算符
    SharedPtr& operator=(const SharedPtr& other) {
        if (this != &other) {
            if (ctrl_block_) {
                ctrl_block_->release_ref();
            }
            ctrl_block_ = other.ctrl_block_;
            if (ctrl_block_) {
                ctrl_block_->add_ref();
            }
        }
        return *this;
    }
    
    SharedPtr& operator=(SharedPtr&& other) noexcept {
        if (this != &other) {
            if (ctrl_block_) {
                ctrl_block_->release_ref();
            }
            ctrl_block_ = other.ctrl_block_;
            other.ctrl_block_ = nullptr;
        }
        return *this;
    }
    
    // 访问指针
    T* get() const { return ctrl_block_ ? ctrl_block_->get() : nullptr; }
    T& operator*() const { return *get(); }
    T* operator->() const { return get(); }
    
    // 引用计数
    int use_count() const { return ctrl_block_ ? ctrl_block_->use_count() : 0; }
    
    // 重置指针
    void reset(T* ptr = nullptr) {
        if (ctrl_block_) {
            ctrl_block_->release_ref();
        }
        ctrl_block_ = ptr ? new ControlBlock<T>(ptr) : nullptr;
    }
    
    // 交换
    void swap(SharedPtr& other) {
        std::swap(ctrl_block_, other.ctrl_block_);
    }
    
    // 转换为bool
    explicit operator bool() const { return get() != nullptr; }
    
private:
    ControlBlock<T>* ctrl_block_;
    
    template <typename U>
    friend class WeakPtr;
};

// weak_ptr 实现
template <typename T>
class WeakPtr {
public:
    WeakPtr() : ctrl_block_(nullptr) {}
    
    WeakPtr(const SharedPtr<T>& shared) : ctrl_block_(shared.ctrl_block_) {
        if (ctrl_block_) {
            ctrl_block_->add_weak_ref();
        }
    }
    
    WeakPtr(const WeakPtr& other) : ctrl_block_(other.ctrl_block_) {
        if (ctrl_block_) {
            ctrl_block_->add_weak_ref();
        }
    }
    
    ~WeakPtr() {
        if (ctrl_block_) {
            ctrl_block_->release_weak_ref();
        }
    }
    
    WeakPtr& operator=(const WeakPtr& other) {
        if (this != &other) {
            if (ctrl_block_) {
                ctrl_block_->release_weak_ref();
            }
            ctrl_block_ = other.ctrl_block_;
            if (ctrl_block_) {
                ctrl_block_->add_weak_ref();
            }
        }
        return *this;
    }
    
    SharedPtr<T> lock() const {
        SharedPtr<T> shared;
        if (ctrl_block_ && ctrl_block_->use_count() > 0) {
            shared.ctrl_block_ = ctrl_block_;
            ctrl_block_->add_ref();
        }
        return shared;
    }
    
    int use_count() const { return ctrl_block_ ? ctrl_block_->use_count() : 0; }
    
private:
    ControlBlock<T>* ctrl_block_;
};

class MyClass {
public:
    MyClass() { std::cout << "MyClass constructed\n"; }
    ~MyClass() { std::cout << "MyClass destroyed\n"; }
    void foo() { std::cout << "MyClass::foo()\n"; }
};

int main() {
    // 创建shared_ptr
    SharedPtr<MyClass> ptr1(new MyClass());
    
    // 拷贝构造
    SharedPtr<MyClass> ptr2 = ptr1;
    std::cout << "Use count: " << ptr1.use_count() << std::endl;  // 2
    
    // 通过weak_ptr观察
    WeakPtr<MyClass> weak = ptr1;
    std::cout << "Use count via weak: " << weak.use_count() << std::endl;  // 2
    
    // 重置ptr1
    ptr1.reset();
    std::cout << "Use count after reset: " << ptr2.use_count() << std::endl;  // 1
    
    // 从weak_ptr获取shared_ptr
    if (auto shared = weak.lock()) {
        shared->foo();
    } else {
        std::cout << "Object already destroyed\n";
    }
    
    // ptr2离开作用域，对象被销毁
    return 0;
}
```

## 57. 请介绍 C++ 中 weak_ptr 的原理？

#### 回答重点

`std::weak_ptr` 是 C++11 引入的新特性，它主要搭配 `std::shared_ptr` 一起使用，它用来监视 `std::shared_ptr` 的生命周期，不会影响内部的引用计数，主要用于打破 `std::shared_ptr` 之间的循环引用问题。例如在对象 A 和对象 B 相互持有对方的 `shared_ptr` 时，会造成无法释放内存的情况，`weak_ptr` 可以帮助解决这个问题。

它提供了一种观察资源但不拥有资源的手段，我们可以通过它来检查资源是否依然存在，并且在需要时将其转变为一个 `shared_ptr` 进行使用。

#### 扩展知识

1）循环引用：下面是 `shared_ptr` 循环引用的示例：

```cpp
#include <iostream>
#include <memory>
using namespace std;
struct A;
struct B;

struct A {
   std::shared_ptr<B> bptr;
   ~A() {
       cout << "A delete" << endl;
   }
   void Print() {
       cout << "A" << endl;
   }
};

struct B {
   std::shared_ptr<A> aptr; // 这里产生了循环引用
   ~B() {
       cout << "B delete" << endl;
   }
   void PrintA() {
       aptr->Print();
   }
};

int main() {
   auto aaptr = std::make_shared<A>();
   auto bbptr = std::make_shared<B>();
   aaptr->bptr = bbptr;
   bbptr->aptr = aaptr;
   bbptr->PrintA();
   return 0;
}

输出：
A
```

从上面示例可以看到，A 持有了 B，B 又持有了 A，导致两者的引用计数都无法减为 0，两者对象都没办法析构，出现了内存泄漏。

2）解决循环引用：

使用 `weak_ptr` 就可以解决上面的循环引用问题，看示例代码：

```cpp
#include <iostream>
#include <memory>
using namespace std;
struct A;
struct B;

struct A {
   std::shared_ptr<B> bptr;
   ~A() {cout << "A delete" << endl;
   }void Print() {cout << "A" << endl;
   }
};

struct B {std::weak_ptr<A> aptr; // 这里改成weak_ptr
   ~B() {
       cout << "B delete" << endl;
   }
   void PrintA() {
       if (!aptr.expired()) { // 监视shared_ptr的生命周期
       auto ptr = aptr.lock();
           ptr->Print();
      }
   }
};

int main() {
   auto aaptr = std::make_shared<A>();
   auto bbptr = std::make_shared<B>();
   aaptr->bptr = bbptr;
   bbptr->aptr = aaptr;
   bbptr->PrintA();
   return 0;
}

输出：
A
A delete
B delete
```

因为 `weak_ptr` 不持有引用计数，不管理资源，所以这里不会出现循环引用问题，引用计数会减为 0，两者对象都会正常析构。

**3）基本原理：**`weak_ptr` 基于 `shared_ptr` 实现。`weak_ptr` 本身不管理资源，而是与 `shared_ptr` 共享内部控制块（control block）。这个控制块包含了实际资源指针、引用计数（`shared_count` 和 `weak_count`）。当所有的 `shared_ptr` 对象销毁时，资源被释放，但控制块直到所有 `weak_ptr` 也销毁时才会被释放。

**4）成员函数：**

- `lock()`：将 `weak_ptr` 转换为 `shared_ptr`，若资源已被释放则返回一个空的 `shared_ptr`。
- `expired()`：检查 `weak_ptr` 指向的资源是否已被释放。

**5）线程安全：**控制块是线程安全的，因此 `shared_ptr` 和 `weak_ptr` 本身是线程安全的。多个线程可以同时使用 `shared_ptr` 和 `weak_ptr` 而不需要额外的同步机制。

<table>
    <tr>
    <td>特性<br/></td><td>栈内存 (Stack)<br/></td><td>堆内存 (Heap)<br/></td></tr>
    <tr>
    <td> 管理方式 <br/></td><td>编译器自动分配/释放，生命周期与作用域绑定<br/></td><td>程序员手动管理（或通过智能指针），生命周期动态控制<br/></td></tr>
    <tr>
    <td> 存储位置 <br/></td><td>函数调用栈帧内<br/></td><td>进程的全局堆区<br/></td></tr>
    <tr>
    <td> std::string   应用 <br/></td><td>- 对象本身始终在栈上<br/>- 短字符串优化（SSO）时，数据直接嵌入对象（栈存储）<br/></td><td>- 长字符串时，对象内的指针指向堆内存<br/>- 数据动态分配，容量可扩展<br/><br/></td></tr>
    <tr>
    <td> 典型场景 <br/></td><td>`std::string s = "Hi";` （SSO 生效）<br/></td><td>`std::string s = "Very long string...";` （SSO 不适用）<br/></td></tr>
    <tr>
    <td> 访问速度 <br/></td><td>快（CPU 缓存友好）<br/></td><td>稍慢（需间接寻址）<br/></td></tr>
    <tr>
    <td> 线程安全 <br/></td><td>栈变量线程私有（安全）<br/></td><td>需同步机制（多线程修改同一堆数据会竞争）<br/></td></tr>
    </table>



## 58. STL 容器的六个组件是什么？

主要是：容器，算法，迭代器，仿函数， 适配器，空间配置器。

![image-20250511162929250](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511162929250.png)


#### **容器**

用于存储和管理数据，提供多种数据结构（如动态数组、链表、树、哈希表等）。

**分类**：

```cpp
std::vector<int> vec = {1, 2, 3};  _// 动态数组_
std::map<std::string, int> m = {{"Alice", 25}};  _// 键值对_
```

#### **算法**

**作用**：对容器中的数据进行通用操作（如排序、查找、遍历、修改等），通过迭代器与容器解耦。

**分类**：

- **非修改序列算法**：`find`、`count`、`for_each`。
- **修改序列算法**：`copy`、`replace`、`remove`。
- **排序与搜索算法**：`sort`、`binary_search`。
- **数值算法**：`accumulate`（求和）、`inner_product`（点积）。

```cpp
std::sort(vec.begin(), vec.end());  _// 排序_
auto it = std::find(vec.begin(), vec.end(), 2);  _// 查找元素_
```

#### **迭代器**

提供访问容器元素的统一接口，充当容器与算法之间的桥梁（类似指针的抽象）。

![](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511162737342.png)


```cpp
for (auto it = vec.begin(); it != vec.end(); ++it) {
    std::cout << *it << " ";  _// 通过迭代器访问元素_
}
```

#### **仿函数**

行为类似函数的对象（重载了 `operator()` 的类），用于定制算法的操作逻辑。它比普通函数更灵活，可携带状态（通过成员变量）。

**分类**：

- **算术仿函数**：`plus`、`minus`、`modulus`（定义在 `<functional>` 中）。
- **关系仿函数**：`less`、`greater`、`equal_to`。
- **逻辑仿函数**：`logical_and`、`logical_not`。

```cpp
std::sort(vec.begin(), vec.end(), std::greater<int>());  _// 降序排序_
```

#### **适配器**

- **作用**：对现有组件进行封装，改变其接口或行为，提供更特定的功能。
- **常见适配器**：

  - **容器适配器**：`stack`（基于 `deque`/`list`）、`queue`（基于 `deque`）、`priority_queue`（基于 `vector`）。
  - **迭代器适配器**：`reverse_iterator`（反向遍历）、`insert_iterator`（插入元素）。
  - **函数适配器**：`bind`（参数绑定）、`not1`（逻辑取反）。

```cpp
std::stack<int> s;  _// 默认基于 deque_
s.push(10);         _// 适配器隐藏了底层容器的细节_
```

#### **空间配置器**

- **作用**：管理容器的内存分配与释放，实现内存分配的灵活控制（如内存池优化）。
- **默认行为**：STL 容器默认使用 `std::allocator`（调用 `new` 和 `delete`）。
- **自定义场景**：需要优化内存碎片或性能时，可替换为自定义分配器。

```cpp
std::vector<int, MyAllocator<int>> vec;  _// 使用自定义分配器_
```

#### **六大组件的关系**

1. **容器**通过**空间配置器**管理内存。
2. **算法**通过**迭代器**访问容器数据。
3. **仿函数**和**适配器**增强算法和容器的灵活性。
4. 组件间高度解耦，用户可独立扩展某一部分（如自定义容器或分配器）

## 59. 🌟C++ 有哪些进程间通信的方式？

#### 回答重点

C++ 支持多种进程间通信（IPC）方式：

1）管道（Pipes）

2）消息队列（Message Queues）

3）共享内存（Shared Memory）

4）信号（Signals）

5）套接字（Sockets）

6）文件（Files）

#### 扩展知识

这些 `IPC` 通信方式各有优缺点，

**1）不同场景选择不同方式**

**管道（Pipes）**

- **匿名管道：**通常用于具有父子关系的进程间通信，它们是单向的，也就是数据只能沿一个方向流动，如果需要双向通信，需要使用两个匿名管道。
- **命名管道（Named Pipe）：**命名管道通过在文件系统中创建一个特殊文件来实现。它可以用于没有亲缘关系的进程间通信，并且是双向的。

**消息队列（Message Queues）** 进程以消息的形式进行通信。消息队列具有以下特点：

- 消息队列中的消息具有特定的标识，可以优先级排序。
- 不需同步，消息独立存在，不会覆盖。
- Posix 中可以使用 `mq_open`、`mq_send`、`mq_receive`、`mq_close`、`mq_unlink` 进行操作。

**共享内存（Shared Memory）** 共享内存是最快的进程间通信方式，因为数据不需要从一个缓冲区拷贝到另一个缓冲区。特点是：

- 多个进程可以同时访问同一个内存段。
- 在 Linux 上，使用 `shmget`, `shmat`, `shmdt`, `shmctl` 等系统调用进行操作。
- 需要同步机制（如信号量）来防止竞争条件。

**信号（Signals） **信号是一种最古老的进程间通信方式。信号是一种比较简单的通知机制：

- 用于通知进程发生了某个事件。
- 由于信号携带的信息量很少，通常用于简单的通知和控制。

**套接字（Sockets） **套接字不仅支持进程间通信，而且可以用于网络通信。分为：

- 本地域套接字（UNIX Domain Sockets）：用于同一主机上的进程间通信。
- 网络套接字（TCP/UDP）：用于不同主机间的进程通信，应用广泛但相对速度较慢。

**文件（Files）** 尽管效率不是很高，但使用文件进行进程间通信比较简单：

- 通过文件写入和读取来传递信息。
- 避免文件竞争通常使用文件锁（如 `flock`）。

**2）不同平台建议选择不同的****IPC****方式**

- Windows 平台建议使用 `命名管道` 做 IPC，实际使用起来比较稳定。[参考文档](https://learn.microsoft.com/en-us/windows/win32/ipc/named-pipes)
- Linux 平台建议考虑直接使用 `zmq` 组件中的 IPC 通信方式，性能高，使用起来也稳定。[参考文档](http://api.zeromq.org/4-2:zmq-ipc)

![image-20250511162804764](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511162804764.png)



###### 跨平台建议

<table>
<tr>
<td>平台<br/></td><td>推荐 IPC 方式<br/></td><td>理由<br/></td></tr>
<tr>
<td>Linux<br/></td><td>共享内存 + UNIX 套接字<br/></td><td>高性能；原生支持完善<br/></td></tr>
<tr>
<td>Windows<br/></td><td>命名管道<br/></td><td>API 稳定；与系统集成度高<br/></td></tr>
<tr>
<td>跨平台<br/></td><td>ZeroMQ（zmq_ipc）<br/></td><td>封装底层差异；提供统一的高性能接口<br/></td></tr>
</table>


###### 关键对比维度

1. **速度**：共享内存 > 管道/套接字 > 消息队列 > 文件
2. **复杂度**：共享内存（需同步） > 套接字 > 消息队列 > 管道 > 文件
3. **数据量**：共享内存（大块数据） > 消息队列（结构化数据） > 管道/套接字（流式数据）

## 60. 什么场景下使用锁？什么场景下使用原子变量？

#### 回答重点

锁（lock）和原子变量（atomic）都可以用作同步机制，它们有各自的适用场景：

1）使用锁的场景：

- 当需要保护长时间访问的临界区时，比如复杂的操作或逻辑（如链表、树等复杂数据结构的操作）。
- 当多个共享资源需要同步访问时，锁可以一次性锁定多个资源，确保整体一致性。
- 在涉及到复杂的操作时，比如需要一次性更新多个共享变量。

2）使用原子变量的场景：

- 当操作可以在一个原子步骤内完成时，比如简单的整数增减、标志位切换。
- 当性能非常关键，且锁的开销和上下文切换的成本过高时。原子操作通常比使用锁更轻量级。
- 用于实现非阻塞算法时，因为原子变量不会导致线程挂起而等待锁释放。

**建议：**优先使用原子变量，如果发现使用原子变量不能满足同步机制的需求，那就使用锁。

#### 扩展知识

这里进一步探讨锁和原子操作：

1）锁的类型：

- **互斥锁（Mutex）：**最常见的普通锁，用于保护一个共享资源。
- 读写锁（Read-Write Lock）：允许多个读者并行访问，但写者访问需要独占。
- **自旋锁（Spinlock）：**线程在等待时会不断轮询锁状态，而不是挂起，非常适合短时间持有锁的场景。

2）原子操作：

- 可以使用 `std::atomic` 库提供的原子类型，如 `std::atomic<int>`, `std::atomic<bool>`，`atomic` 是个模板类，你还可以使用 double 等类型填充。
- 这些操作通常由硬件直接支持，比如 x86 架构的"Lock"前缀指令，确保读取-修改-写入一个不可分割的操作。

3）实际应用示例：

- **使用锁的例子：**假设我们有一个共享的 `std::map`，需要在线程间进行插入和删除操作，我们可以使用 `std::mutex` 来保护这个 `map`。

```cpp
std::mutex mtx;
std::map<int, std::string> sharedMap;

void insertIntoMap(int key, const std::string& value) {
    std::lock_guard<std::mutex> lock(mtx);
    sharedMap[key] = value;
}
```

- **使用原子变量的例子：**假设我们有一个计数器，只需要每次增加或减少 1，使用 `std::atomic` 更高效。

```cpp
std::atomic<int> counter(0);

void incrementCounter() {
    counter++;
}

void decrementCounter() {
    counter--;
}
```

![image-20250511162822155](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511162822155.png)



###### 选择建议

<table>
    <tr>
    <td> 优先选择原子变量 <br/></td><td> 优先选择锁 <br/></td></tr>
    <tr>
    <td>- 操作可单指令完成（如标志位<br/>- 性能敏感场景（如高频计数器）<br/></td><td>- 操作涉及多个变量<br/>- 需要执行复杂逻辑（如遍历链表）<br/></td></tr>
    </table>


## C++ 面试真题（61-74）


## 61. C++ 什么场景用线程？什么场景用协程？

#### 回答重点

线程和协程是两种不同的并发编程方式，各有其适用的场景。

**1）线程使用场景：**

- **CPU 密集型任务：**线程适合处理需要大量计算的任务，如矩阵运算、复杂算法的并行处理。
- **I/O 密集型任务：**线程更适用于处理需要频繁与外部系统进行数据交换的任务，如网络请求、文件读写。
- **多核处理器充分利用：**当你希望充分利用多核处理器的优势，进行真正的并行计算时，线程非常适合。

**2）协程使用场景：**

- **轻量级任务切换：**协程适用于需要轻量级任务切换的场景，像是大量小任务需要被并发执行时，比如异步的任务处理、网络服务器等等。
- **高并发处理：**在处理大量高并发请求时，协程更合适，因为它的开销相对于线程更小，如高并发的 web 服务器处理请求时。
- **复杂控制流：**协程能够方便地暂停和恢复执行，在处理需要复杂状态机或多步骤操作时显得更加便捷。

#### 扩展知识

结合应用场景，可以更深入地探讨线程和协程的使用：

**1）线程的优势：**

- 真正并行：线程能够在多核处理器上实现真正的并行执行，充分利用硬件资源。
- 适用广泛的库支持：比如 C++ 标准库提供了 `std::thread`，非常易于使用。

**2）线程的劣势：**

- 资源开销大：线程创建和上下文切换的开销较大。
- 复杂的同步机制：面对竞争条件时，需要且不容易处理好各种同步机制，如锁、互斥量等，容易产生死锁等问题，或其他线程安全问题。

**3）协程的优势：**

- 高效的任务切换：协程是用户态的轻量级任务切换，创建和切换开销非常小。
- 更简单的逻辑控制：协程的暂停和恢复机制使编写异步代码更加直观、易读，避免了回调地狱。

**4）协程的劣势：**

- 单线程执行：协程本质上是单线程的，因此无法真正实现并行，需要与线程结合使用才能扩展到多核。
- 库支持欠缺：截至目前，C++20 才引入了较为标准的协程库支持，应用和生态相对较新，相比线程仍有待成熟。

**5）实际应用示例：**

- 多线程应用：如视频渲染、数据压缩解压、科学计算等需要占用多个 CPU 资源的场景。
- 协程应用：如高并发网络服务器、多任务调度器、大量异步 I/O 的处理等。

C++20 引入的协程库提供了更强大的工具来简化异步编程。在特定场景下，比如高并发请求或复杂异步操作，使用协程能够显著简化代码并提高性能，相较于传统的线程方案，优势明显。

<table>
    <tr>
    <td>特性<br/></td><td>线程（Thread）<br/></td><td>协程（Coroutine）<br/></td></tr>
    <tr>
    <td> 适用场景 <br/></td><td>- CPU 密集型任务（如矩阵运算、科学计算）<br/>- 需要真正并行（多核 CPU）<br/>- 阻塞式 I/O 操作（如文件读写、网络请求）<br/></td><td>- 高并发轻量级任务（如 Web 服务器）<br/>- 异步 I/O 操作（如非阻塞网络请求）<br/>- 需要复杂控制流（如状态机、多步骤异步逻辑）<br/></td></tr>
    <tr>
    <td> 并行能力 <br/></td><td>是（多核并行）<br/></td><td>否（单线程协作式调度，需结合线程池实现多核并行）<br/></td></tr>
    <tr>
    <td> 创建/切换开销 <br/></td><td>高（内核态切换，MB 级栈空间）<br/></td><td>极低（用户态切换，KB 级栈空间）<br/></td></tr>
    <tr>
    <td> 同步机制 <br/></td><td>需锁、条件变量等复杂同步原语<br/></td><td>无竞争（单线程内协作调度，天然避免数据竞争）<br/></td></tr>
    <tr>
    <td> 代码复杂度 <br/></td><td>高（需处理竞态条件和死锁）<br/></td><td>低（线性异步代码，避免回调地狱）<br/></td></tr>
    <tr>
    <td> C++ 标准支持 <br/></td><td>C++11（`std::thread`）<br/></td><td>C++20（协程框架，需编译器支持）<br/></td></tr>
    <tr>
    <td> 资源占用 <br/></td><td>每个线程占用独立栈和内核资源<br/></td><td>数千协程可共享同一线程栈<br/></td></tr>
    </table>



###### 选择建议

<table>
    <tr>
    <td> 优先选择线程 <br/></td><td> 优先选择协程 <br/></td></tr>
    <tr>
    <td>- 需要多核 CPU 并行计算<br/>- 处理阻塞式长耗时任务<br/></td><td>- 高并发 I/O 密集型任务<br/>- 需要简化异步代码逻辑<br/></td></tr>
    </table>



###### 混合使用场景

<table>
    <tr>
    <td>方案<br/></td><td>说明<br/></td></tr>
    <tr>
    <td> 线程池 + 协程 <br/></td><td>用线程池处理 CPU 密集型任务，协程处理 I/O 密集型任务（如 Nginx 架构）<br/></td></tr>
    <tr>
    <td> 多线程调度协程 <br/></td><td>每个线程运行独立协程调度器，平衡并行与并发（如 Go 语言的 Goroutine）<br/></td></tr>
    </table>



###### 补充说明

1. **协程的局限性**：

   - 协程依赖异步 I/O 框架（如 `io_uring` 或 `libuv`）才能发挥性能优势。
   - C++20 协程为无栈协程（Stackless），需通过 `co_await`/`co_yield` 显式切换。
2. **线程的优化方向**：

   - 使用线程池（如 `std::async`）避免频繁创建/销毁线程。
   - 结合无锁数据结构减少同步开销。

## 62. 用过哪些 C++ 日志框架？都有什么优缺点？

#### 回答重点

C++ 的日志框架，流行的主要有：log4cpp、Boost.Log、spdlog、glog。

各自的优缺点如下：

1）log4cpp：

- **优点：**功能强大，支持多种日志输出和格式化方式；配置灵活，支持外部配置文件。
- **缺点：**配置相对复杂，文档不是太完善，比较老的库了，现在用的不是很多。

2）Boost.Log：

- **优点：**属于 Boost 库的一部分，方便与 Boost 的其他库集成；功能全面，支持多线程。
- **缺点：**编译时间较长，配置略显复杂。

3）spdlog：推荐使用。

- **优点：**非常快，效率很高；易于使用，接口简洁；支持 header-only 形式的接入，不需要进行复杂的编译配置。
- **缺点：**功能相对简单，不如 log4cpp 和 Boost.Log 复杂功能全面。

4）glog：

- **优点：**Google 出品，性能和稳定性有保证；支持多线程，适合大型项目。
- **缺点：**库的体积相对较大，不支持 header-only 接入，接入起来比较麻烦。

#### 扩展知识

为了让大家更好地理解，我再补充一些背景和使用体验吧。

1）log4cpp： 我发现 log4cpp 在一些老旧的 C++ 项目中很常见，因为它出现的时间较长，功能也相对全面。通过 external 配置文件（如 XML 或 properties 文件），可以精确地控制日志的输出格式和目录，然而，新手在配置时可能会有点吃力，需要认真研读官方文档，新项目就不太建议使用 log4cpp 了。

2）Boost.Log： Boost.Log 是 Boost 库的一个模块，提供了相当全面的日志功能。由于库的复杂性以及编译时间较长，项目启动时间可能会有所增加。不过，如果你的项目已经使用了 Boost 库，添加 Boost.Log 可能会是一个不错的选择，因为它们之间的集成会比较顺畅。

3）spdlog：** 如果只推荐一个日志库的话，我推荐使用 spdlog。**spdlog 可以说是一个新时代的日志框架，它采用了 C++11/14 的新特性，使得整体效率非常高。我非常喜欢它的 header-only 设计，让你不用担心额外的库文件。对于性能有极高要求的项目，spdlog 是一个理想的选择，但要注意的是，它的功能相对简单，如果你需要非常复杂的日志功能，可能需要自己进行二次开发。

4）glog： glog 是 Google 的一个 C++ 日志库，通常用在一些需要高可靠性的项目中。它支持多线程，这让我在构建一些大型项目时非常放心。不过，相对于 spdlog，它在接入便捷性和性能上都略逊一筹。

<table>
    <tr>
    <td>日志框架<br/></td><td>优点<br/></td><td>缺点<br/></td><td>适用场景<br/></td><td>接入方式<br/></td></tr>
    <tr>
    <td> log4cpp <br/><br/></td><td>- 功能全面（支持多种输出格式）<br/>- 灵活配置（XML/Properties文件）<br/></td><td>- 配置复杂<br/>- 文档不完善<br/>- 社区活跃度低<br/></td><td>遗留项目维护<br/></td><td>需编译链接<br/></td></tr>
    <tr>
    <td> Boost.Log <br/></td><td>- 与Boost生态无缝集成<br/>- 多线程安全<br/>- 支持高级过滤和格式化<br/></td><td>- 编译时间长<br/>- 学习曲线陡峭<br/></td><td>已使用Boost的中大型项目<br/></td><td>需编译链接<br/></td></tr>
    <tr>
    <td> spdlog <br/></td><td>- 性能极致（异步模式可达百万条/秒）<br/>- 头文件-only<br/>- 简洁易用API<br/></td><td>- 功能较基础<br/>- 复杂需求需二次开发<br/></td><td>高性能应用/快速原型开发<br/></td><td>头文件-only<br/></td></tr>
    <tr>
    <td> glog <br/></td><td>- Google背书稳定性高<br/>- 崩溃日志自动记录<br/>- 多线程支持<br/></td><td>- 体积较大<br/>- 接口风格老旧<br/>- 定制化困难<br/></td><td>大型分布式系统<br/></td><td>需编译链接<br/></td></tr>
    </table>



###### 详细功能对比

<table>
    <tr>
    <td>特性<br/></td><td>log4cpp<br/></td><td>Boost.Log<br/></td><td>spdlog<br/></td><td>glog<br/></td></tr>
    <tr>
    <td> 异步日志 <br/></td><td>✔️<br/></td><td>✔️<br/></td><td>✔️<br/></td><td>❌<br/></td></tr>
    <tr>
    <td> 多线程安全 <br/></td><td>✔️<br/></td><td>✔️<br/></td><td>✔️<br/></td><td>✔️<br/></td></tr>
    <tr>
    <td> 日志分级 <br/></td><td>✔️<br/></td><td>✔️<br/></td><td>✔️<br/></td><td>✔️<br/></td></tr>
    <tr>
    <td> 文件滚动 <br/></td><td>✔️<br/></td><td>✔️<br/></td><td>✔️<br/></td><td>❌<br/></td></tr>
    <tr>
    <td> 头文件-only <br/></td><td>❌<br/></td><td>❌<br/></td><td>✔️<br/></td><td>❌<br/></td></tr>
    <tr>
    <td> 崩溃日志记录 <br/></td><td>❌<br/></td><td>❌<br/></td><td>❌<br/></td><td>✔️<br/></td></tr>
    </table>



## 63. 🌟 介绍下 socket 的多路复用？epoll 有哪些优点？

#### 回答重点

多路复用这个术语在网络编程中非常重要，尤其是在涉及 I/O 操作的时候。所谓 socket 的多路复用，指的是在单个线程或进程中可以同时处理多个 socket 的 I/O 事件，可以提高整体效率和资源利用率。

常见的多路复用机制包括 `select`、`poll` 和 `epoll`，在 Linux 平台上这种机制主要依赖于 `epoll`，因为它在大多数情况下性能更好。`epoll` 是 Linux 内核针对大量并发连接进行高效管理的系统调用接口。

编程过程中，建议使用 `epoll`。`epoll` 相比于 `select` 和 `poll`，主要有以下几个优点：

**1）效率高：**epoll 使用事件通知的方式能够解决轮询（polling）带来的性能瓶颈，对大量文件描述符的处理效率高。

**2）不受描述符数量限制：**select 有文件描述符数量的上限（通常是 1024），而 epoll 没有这种限制。

**3）内存拷贝少：**epoll 的系统调用仅在需要数据时进行内存拷贝，减少了系统开销。

**4）支持边沿触发：**相比于 select 和 poll 的水平触发（level-triggered），epoll 还支持边沿触发（edge-triggered），能够适应更多的应用场景。

#### 扩展知识

可以从几个方面进一步展开：

**1）select 和 poll 的缺点：** select 和 poll 都是 I/O 多路复用的早期实现，但它们有一些不足。例如，select 在每次调用时都需要重新传递所有文件描述符集合，并进行内存拷贝，而 poll 则需要传递整个文件描述符数组，这在文件描述符特别多的情况下，性能开销很大。此外，select 还有一个描述符数量的限制。

**2）epoll 的工作机制：** epoll 使用两个系统调用来操作：`epoll_create` 创建一个 epoll 实例，`epoll_ctl` 增加、修改或删除要控制的文件描述符。`epoll_wait` 则是用于等待事件的发生。与 select 不同的是，epoll 每次只需传递发生的事件，不需要传递所有文件描述符，极大提高了效率。

**3）边沿触发与水平触发：** 水平触发（Level-triggered, LT）是默认的触发模式，处理器只要发现事件有未处理的数据就会再次通知，在传统的 select 和 poll 中也是这种方式。而边沿触发（Edge-triggered, ET）是更高效的一种方式，它只会在状态变化（例如从无数据到有数据）时通知一次，开发难度稍大但可以减少系统调用次数，提高性能。

通过上面介绍，可以看出 `epoll` 是如何更高效率地进行 I/O 多路复用的。在实际工作中，在大规模高并发 I/O 操作时，**建议优先选择** `epoll`。

## 64. Socket 多路复用机制对比

<table>
    <tr>
    <td>特性<br/></td><td>select<br/></td><td>poll<br/></td><td>epoll<br/></td></tr>
    <tr>
    <td> 时间复杂度 <br/></td><td>O(n) 轮询所有fd<br/></td><td>O(n) 轮询所有fd<br/></td><td>O(1) 事件通知<br/></td></tr>
    <tr>
    <td> fd数量限制 <br/></td><td>1024 (FD_SETSIZE)<br/></td><td>无硬限制<br/></td><td>无硬限制<br/></td></tr>
    <tr>
    <td> 内存拷贝 <br/></td><td>每次调用需拷贝全部fd集合<br/></td><td>需拷贝整个fd数组<br/></td><td>仅返回就绪事件，无重复拷贝<br/></td></tr>
    <tr>
    <td> 触发模式 <br/></td><td>仅水平触发(LT)<br/></td><td>仅水平触发(LT)<br/></td><td>支持LT和边沿触发(ET)<br/></td></tr>
    <tr>
    <td> 适用场景 <br/></td><td>低并发兼容性场景<br/></td><td>中低并发跨平台场景<br/></td><td>高并发Linux专属场景<br/></td></tr>
    </table>



#### epoll 的核心优势详解

###### 1. 高效事件驱动(O(1)复杂度)

- **原理**：通过内核事件表(红黑树 + 就绪链表)直接获取就绪事件
- **优势**：无需遍历所有 fd，性能随 fd 数量增长几乎不下降

###### 2. 无文件描述符数量限制

- **select 限制**：默认仅支持 1024 个 fd
- **epoll 突破**：仅受系统内存限制，可支持数十万并发连接

## 65. 🌟C++ 中 vector 的 push_back 和 emplace_back 有什么区别？

#### 回答重点

两者都是 vector 类的成员函数，用于在 vector 的末尾添加元素。它们之间的主要区别在于添加元素的方式：

1. push_back：接受一个已存在的对象作为参数，进行拷贝或移动，将其添加到 vector 的末尾。这会**触发一次拷贝或移动构造函数的调用**，具体取决于传递的对象是否可移动。
2. emplace_back：接受构造函数的参数，直接在 vector 的内存空间中调用该对象的构造函数，**避免了额外的拷贝或移动操作**。这可以提高效率，特别是在处理复杂对象时。

#### 扩展知识

**性能差异：**

- push_back 因为需要拷贝或移动已经存在的对象，较之 emplace_back 效率稍低，特别是对于大型或复杂对象，额外的拷贝或移动会显著影响性能。
- emplace_back **直接在容器中构造对象**，避免了不必要的对象构造和析构以及拷贝或移动，效率更高。

**使用场景：**

- 如果需要将一个已经存在的对象添加到 vector 中，使用 push_back。
- 如果希望直接在 vector 中构造对象，避免额外的拷贝或移动开销，使用 emplace_back。

**代码示例：**

```cpp
#include <iostream>
#include <vector>
#include <string>

class MyClass {
public:
    MyClass(int a, std::string b) : a_(a), b_(b) {
        std::cout << "Constructor called\n";
    }
    MyClass(const MyClass& other) : a_(other.a_), b_(other.b_) {
        std::cout << "Copy Constructor called\n";
    }
    MyClass(MyClass&& other) noexcept : a_(other.a_), b_(std::move(other.b_)) {
        std::cout << "Move Constructor called\n";
    }
private:
    int a_;
    std::string b_;
};

int main() {
    std::vector<MyClass> v;
    v.reserve(16);
    
    std::cout << "Using push_back:\n";
    MyClass obj1(1, "example1");
    v.push_back(obj1);  // 会调用拷贝构造
    v.push_back(std::move(obj1)); // 会调用移动构造
    
    std::cout << "\nUsing emplace_back:\n";
    v.emplace_back(2, "example2"); // 直接在 vector 内存空间中构造，无需拷贝或移动
        std::cout << "\nover \n";
    return 0;
}

输出：
Using push_back:
Constructor called
Copy Constructor called
Move Constructor called

Using emplace_back:
Constructor called

over
```

在上面的示例中（输出结果也贴在了代码中），当使用 push_back 时，会调用拷贝构造函数或移动构造函数。而使用 emplace_back 时，直接构造对象，避免了额外的构造和析构开销。

**线程安全性：**无论是 push_back 还是 emplace_back，它们在多线程环境下都不是线程安全的。因此，必须考虑同步机制（如互斥锁）来避免数据竞争。

**二者选择：**建议无脑选择 emplace_back。

<table>
    <tr>
    <td>特性<br/></td><td>`push_back`<br/></td><td>`emplace_back`<br/></td></tr>
    <tr>
    <td> 参数类型 <br/></td><td>接受对象（左值/右值）<br/></td><td>接受构造参数（可变参数模板）<br/></td></tr>
    <tr>
    <td> 构造方式 <br/></td><td>调用拷贝/移动构造函数<br/></td><td>直接在容器内存中构造对象<br/></td></tr>
    <tr>
    <td> 性能开销 <br/></td><td>额外1次拷贝/移动操作<br/></td><td>零额外拷贝/移动<br/></td></tr>
    <tr>
    <td> 适用场景 <br/></td><td>已有对象需要插入<br/></td><td>需要直接构造新对象<br/></td></tr>
    <tr>
    <td> 代码示例 <br/></td><td>```vec.push_back(obj)；vec.push_back(std::move(obj))```<br/></td><td>```vec.emplace_back(args...)```<br/><br/></td></tr>
    </table>



## 66. 🌟C++ 成员变量的初始化顺序是固定的吗？

#### 回答重点

成员变量的初始化顺序是固定的。成员变量总是按照它们在类中出现的顺序进行初始化，而不是在构造函数中的初始化列表顺序。

例如：

```cpp
class MyClass {
public:
    MyClass(int a, int b) : b(b), a(a) {}
  
private:
    int b;
    int a;
};
```

即使在构造函数的初始化列表中先初始化 `b`，后初始化 `a`，成员变量仍然会按照 `int b; int a;` 的顺序来初始化。

#### 扩展知识

当涉及到成员变量的初始化时，有几个重要的点需要注意：

**1）依赖关系（重点）：** 如果类的成员变量之间存在依赖关系，需要特别注意初始化顺序。例如，当一个成员变量的初始值取决于另一个成员变量的值时，应确保这些依赖关系在声明顺序中得到正确处理。

**2）静态成员变量：** 静态成员变量的初始化与普通成员变量不同。静态成员变量的初始化通常在类定义体外进行，并且只初始化一次。

**3）初始化列表的使用：** 使用初始化列表对于效率和可读性通常是有益的，因为它避免了成员变量在构造函数体内先被默认初始化然后再被赋值的开销。

**4）继承时的初始化顺序：** 在类的继承体系中，基类的构造函数会先于派生类的构造函数执行，因此基类的成员变量也会先于派生类的成员变量进行初始化。

具体地说：

- 基类的成员变量按声明顺序初始化。
- 派生类的成员变量按声明顺序初始化。在这个过程中，派生类的初始化顺序紧随基类之后。

**5）成员对象的初始化顺序： **如果类中包含其他类的对象作为成员，这些成员对象也会按照它们的声明顺序优先于自身的构造函数体内的代码进行初始化。

## 67. C++ 中未初始化和已初始化的全局变量放在哪里？全局变量定义在头文件中有什么问题？

#### 回答重点

1）未初始化的全局变量放在 BSS 段，而已初始化的全局变量放在数据段（Data Segment）。

2）将全局变量定义在头文件中会引发多重定义的问题，如果头文件可能会被多个源文件包含，导致编译时同一个变量被多次定义，进而编译失败。

#### 扩展知识

下面深入探讨一下这些段和多重定义的问题。

**1）BSS 段和数据段**

BSS 段全称 "Block Started by Symbol" 或 "Block Storage Segment"，用于存放程序中未初始化或初始化为零的全局变量和静态变量。因为这些变量在程序加载时会被自动初始化为零，所以在编译好的程序中只占用很少的空间（只是需要在运行时期占用内存）。

数据段则包含初始化过的全局变量和静态变量。这个段在程序加载到内存时也被加载，并包含那些已经有初始值的变量，它们在程序运行期间保持这个初始值。

**2）头文件中的全局变量定义问题**

头文件中的全局变量定义会造成重复定义的问题。假设你在一个头文件 `example.h` 中这样定义了一个全局变量：

```cpp
int globalVar;
```

然后你在两个源文件 `file1.cpp` 和 `file2.cpp` 中都包含了 `example.h`。结果是编译器会在链接的时候发现 `globalVar` 被定义了两次，导致编译错误。

解决这一问题的方法，可以使用 `static` 修饰变量，或者使用 `extern` 关键字声明全局变量，然后在一个源文件中进行定义，例如：

```cpp
// example.h
extern int globalVar;

// file1.cpp
#include "example.h"
int globalVar = 0; // 这里只定义一次

// file2.cpp
#include "example.h"// 
可以直接使用 globalVar，但不需要再次定义
```

通过这个方式，在头文件中的 `extern` 声明不会实际分配内存，而是在一个特定的源文件中定义一次，其他源文件包含头文件时就共享这个变量的定义。这样不仅能避免多重定义的问题，还能确保每个文件都能访问同一个全局变量。这是很多项目标准的做法，能有效管理全局变量及其作用范围。

**开发建议：不要使用全局变量，可以考虑做好代码设计，非必要不使用全局变量。**

## 68. 🌟 什么情况下会出现内存泄漏？如何避免内存泄漏？

#### 回答重点

申请了内存，但未释放，就是内存泄漏，有几个经典的场景：

1）对象创建后却没有释放。

2）智能指针的循环引用，两者互相持有，导致引用计数永不为 0，内存无法释放。

3）集合类容器中，删除元素后未释放内存。

4）在外面手动申请的内存，但进入了异常处理，手动分配的内存未释放。

5）静态成员或全局变量持有动态分配的对象。

避免内存泄漏的方法：

1）使用智能指针（比如 `std::unique_ptr` 和 `std::shared_ptr`），自动管理内存。

2）用 `RAII` 原则，通过构造函数分配资源并在析构函数中释放。

3）执行静态分析工具（如 cppcheck）和内存检测工具（如 Valgrind）来检测代码质量和内存泄漏。

4）规范编码，确保每个 `new` 对应一个 `delete`，每个 `malloc` 对应一个 `free`。但在函数中也要注意对 if-else-return 分支的处理，确保每个 return 之前都能释放对应的内存。

5）避免循环引用，使用 `std::weak_ptr` 解决循环引用的问题。

#### 扩展知识

探讨一下智能指针的相关知识点。

C++11 标准引入了三种主要的智能指针：`std::unique_ptr`、`std::shared_ptr` 和 `std::weak_ptr`：

###### 1. `std::unique_ptr`

`std::unique_ptr` 是一种独占所有权的智能指针，意味着同一时间内只能有一个 `unique_ptr` 指向一个特定的对象。当 `unique_ptr` 被销毁时，它所指向的对象也会被销毁。

**使用场景：**

- 当你需要确保一个对象只被一个指针所拥有时。
- 当你需要自动管理资源，如文件句柄或互斥锁时。

**示例代码：**

```cpp
#include <iostream>
#include <memory>

class Test {
public:
    Test() { std::cout << "Test::Test()"; }
    ~Test() { std::cout << "Test::~Test()"; }
    void test() { std::cout << "Test::test()"; }
};

int main() {
    std::unique_ptr<Test> 
    ptr(new Test());
    ptr->test();
    // 当ptr离开作用域时，它指向的对象会被自动销毁
    return 0;
}
```

###### 2. `std::shared_ptr`

`std::shared_ptr` 是一种共享所有权的智能指针，多个 `shared_ptr` 可以指向同一个对象。内部使用引用计数来确保只有当最后一个指向对象的 `shared_ptr` 被销毁时，对象才会被销毁。

**使用场景：**

- 当你需要在多个所有者之间共享对象时。
- 当你需要通过复制构造函数或赋值操作符来复制智能指针时。

**示例代码：**

```cpp
#include <iostream>
#include <memory>

class Test {
public:
    Test() { std::cout << "Test::Test()"; }
    ~Test() { std::cout << "Test::~Test()"; }
    void test() { std::cout << "Test::test()"; }
};

int main() {
    std::shared_ptr<Test> ptr1(new Test());
    std::shared_ptr<Test> ptr2 = ptr1;
    ptr1->test();
    // 当ptr1和ptr2离开作用域时，它们指向的对象会被自动销毁
    return 0;
}
```

###### 3. `std::weak_ptr`

`std::weak_ptr` 是一种不拥有对象所有权的智能指针，它指向一个由 `std::shared_ptr` 管理的对象。`weak_ptr` 用于解决 `shared_ptr` 之间的循环引用问题。

**使用场景：**

- 当你需要访问但不拥有由 `shared_ptr` 管理的对象时。
- 当你需要解决 `shared_ptr` 之间的循环引用问题时。
- 注意 `weak_ptr` 肯定要和 `shared_ptr` 搭配使用。

**示例代码：**

```cpp
#include <iostream>
#include <memory>

class Test {
public:
    Test() { std::cout << "Test::Test()"; }
    ~Test() { std::cout << "Test::~Test()"; }void test() { std::cout << "Test::test()"; }
};

int main() {
    std::shared_ptr<Test> sharedPtr(new Test());
    std::weak_ptr<Test> weakPtr = sharedPtr;
    
    if (auto lockedSharedPtr = weakPtr.lock()) {
        lockedSharedPtr->test();
    }// 当sharedPtr离开作用域时，它指向的对象会被自动销毁
    return 0;
}
```

这三种智能指针各有其用途，选择哪一种取决于你的具体需求。

#### 内存泄漏常见场景

![image-20250511163054924](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511163054924.png)



#### 智能指针对比

![](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511163342110.png)


#### 防范措施对比

<table>
    <tr>
    <td>方法<br/></td><td>实现手段<br/></td><td>优点<br/></td><td>局限性<br/></td></tr>
    <tr>
    <td> RAII <br/></td><td>构造函数获取资源，析构函数释放<br/></td><td>自动管理，异常安全<br/></td><td>需设计资源管理类<br/></td></tr>
    <tr>
    <td> 智能指针 <br/></td><td>make_shared/unique<br/></td><td>零开销抽象，线程安全<br/></td><td>循环引用需配合weak_ptr<br/></td></tr>
    <tr>
    <td> 静态分析工具 <br/></td><td>cppcheck/Clang-Tidy<br/></td><td>提前发现潜在泄漏<br/></td><td>可能有误报<br/></td></tr>
    <tr>
    <td> 动态检测工具 <br/></td><td>Valgrind/ASan<br/></td><td>运行时精确检测<br/></td><td>性能开销大<br/></td></tr>
    <tr>
    <td> 编码规范 <br/></td><td>每个new对应delete<br/></td><td>简单直接<br/></td><td>难以保证异常安全<br/></td></tr>
    </table>




#### 最佳实践示例

###### 1. 智能指针使用

```cpp
// 推荐做法
auto ptr = std::make_shared<Resource>(); // 替代 new+shared_ptr
std::weak_ptr<Resource> observer = ptr; // 避免循环引用

// 危险做法
Resource* raw_ptr = new Resource();
std::shared_ptr<Resource> p1(raw_ptr);
std::shared_ptr<Resource> p2(raw_ptr); // 导致双重释放！
```

## 69. 🌟C++ 中为什么要引入 make_shared？它有什么优点？

#### 回答重点

C++ 中创建 `shared_ptr` 有两种方式，一种是直接把裸指针传递进去，一种是使用 `make_shared`：

```cpp
class A {
public:
    A() {}
};

std::shared_ptr<A> sp1 = std::shared_ptr<A>(new A());
std::shared_ptr<A> sp2 = std::make_shared<A>();
```

既然推出了 `make_shared`，肯定是有优点，它的优点包括：

1）简化代码：使用 `make_shared` 可以简化创建 `shared_ptr` 实例的代码，代码更加清晰。

2）性能更好：`make_shared` 在内存分配时只需要一次分配，而直接使用 `shared_ptr` 构造函数可能需要两次内存分配。

3）降低内存泄漏风险：使用 `make_shared` 可以避免由于异常导致的部分已分配内存未释放的问题。

#### 扩展知识

详细说一说 `make_shared` 相关的知识点：

1）`shared_ptr` 基础：与之对应的是 `unique_ptr`，, 但使用它有限制，比如不能共享所有权。而 `std::shared_ptr` 是一个可以共享控制权的智能指针，可以自动管理动态分配的对象生命周期。

2）`new` 和 `shared_ptr` ：在没有 `make_shared` 之前，我们通常这样创建 `shared_ptr`: `std::shared_ptr<int> sp(new int(5));`。这个过程其实做了两个动作：创建一个临时对象，又创建一个 `shared_ptr` 对象。如果第一步的内存分配成功，但第二步抛出异常，那么就会发生内存泄漏。

3）`make_shared` 内部原理：`make_shared` 将对象的动态内存和控制块内存（存储引用计数的那块内存）一次性分配，减少了内存分配的次数。例如：`auto sp = std::make_shared<int>(5);`，这种方式比前一种方式高效，并且更加安全。

4）性能更好：单次内存分配意味着分配器只调用一次，这比多次调用（可能导致的内存碎片问题）更加高效。此外，这种方式在多线程环境中也有一定优势，减少了分配内存时的竞争。

5）异常安全：使用 `make_shared`，如果在创建过程中抛出异常，因为它是“全有或全无”的过程，所以不需要担心部分资源分配成功导致的内存泄漏。例如，`make_shared` 可以保证在对象和控制块都构建成功之后才开始使用它们。

<table>
    <tr>
    <td>对比维度<br/></td><td>直接使用 shared_ptr 构造函数 (new)<br/></td><td>使用 make_shared<br/></td></tr>
    <tr>
    <td> 代码简洁性 <br/></td><td>需要显式使用 new，代码冗余<br/></td><td>直接传递参数，代码更简洁<br/></td></tr>
    <tr>
    <td> 内存分配次数 <br/></td><td>两次分配（对象内存 + 控制块内存）<br/></td><td>单次分配（对象和控制块内存连续）<br/></td></tr>
    <tr>
    <td> 性能 <br/></td><td>可能因多次分配导致内存碎片，效率较低<br/></td><td>减少内存碎片，分配效率更高<br/></td></tr>
    <tr>
    <td> 异常安全性 <br/></td><td>若构造 shared_ptr 时抛出异常，可能内存泄漏<br/></td><td>原子化操作，完全成功或失败，无内存泄漏风险<br/></td></tr>
    <tr>
    <td> 内存布局 <br/></td><td>对象和控制块内存可能不连续<br/></td><td>对象和控制块内存连续，缓存局部性更好<br/></td></tr>
    <tr>
    <td> 适用场景 <br/></td><td>需自定义删除器或分离内存分配时使用<br/></td><td>默认推荐方式，尤其适用于高频分配场景<br/></td></tr>
    <tr>
    <td> 多线程优势 <br/></td><td>无特殊优化<br/></td><td>单次分配减少竞争，潜在性能提升<br/></td></tr>
    </table>




## 70. 如何理解 C++ 中的 atomic？

#### 回答重点

`std::atomic` 用于实现原子操作，它也是 C++11 引入的新特性。

多个线程可以对同一个变量进行读写操作，不会导致数据竞争或中间状态，也不需要锁的保护，一定程度上简化了代码编写，性能也会有提高。

#### 扩展知识

这个话题其实挺有深度的，这里展开说说：

**1）什么是原子操作：**原子操作指的是一个不可分割的操作，要么完全执行，要么完全不执行，不会被中途打断。举个例子，假设两个线程要同时增加一个变量，如果没有原子操作，可能会导致未预期的结果。而使用 `std::atomic` 后，这个操作就十分安全了。

**2）如何使用** `std::atomic`：在 C++ 中，可以用 `std::atomic` 来修饰基本的数据类型，如 `int`, `bool`，甚至指针。示例如下：

```cpp
#include <atomic>
#include <iostream>
#include <thread>

std::atomic<int> counter(0);

void increment(int n) {
    for (int i = 0; i < n; ++i) {
        ++counter; // 原子操作，线程安全
    }
}

int main() {
    std::thread t1(increment, 1000);
    std::thread t2(increment, 1000);
    
    t1.join();
    t2.join();
   
    std::cout << "Final counter value: " << counter << std::endl; // 预期输出2000
    return 0;
}
```

3）底层实现：`std::atomic` 通过 CPU 提供的原子指令来实现这些不可分割的操作。现代 CPU 会提供一组指令，比如 CMPXCHG, XADD 等来实现原子的读或写。

4）内存序约束：C++ 提供了多种内存序约束，比如 `memory_order_relaxed`, `memory_order_acquire`, `memory_order_release` 等。这些约束让你可以更好地控制程序的内存可见性和行为。

例如，`memory_order_relaxed` 只保证原子性，但不提供任何同步或顺序保证，而 `memory_order_acquire` 和 `memory_order_release` 则提供更严格的同步机制。

`atomic` 默认使用的是 `memory_order_seq_cst`，也就是最严格的内存序约束，既保证原子性，又提供了同步顺序保证。详见 [cppreference](https://en.cppreference.com/w/cpp/atomic/memory_order)。

5）和锁比较：虽然 `std::atomic` 可以在某些场景下替代锁，但它并不是万能的。锁在某些复杂场景下仍然是不可替代的。原子操作更适合一些基本的计数器或标志位，而对于复杂的数据结构，锁的使用仍是较优选择。

6）性能：使用原子操作通常比使用锁要快，因为锁涉及到上下文切换和操作系统调度，而原子操作都是硬件级别的操作。经过优化的原子操作可以使得你的程序在多线程环境下有更好的性能表现。

![image-20250511163419971](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511163419971.png)

## 71. 什么情况下会出现死锁？如何避免死锁？

#### 回答重点

本题主要考察死锁出现的四大必要条件。

什么情况下会发生死锁？一般来说，多个线程相互等待对方持有的资源且都不释放自己的资源，这种现象称为死锁。

具体有四个必要条件，必须同时满足才会发生死锁：

**1） 互斥条件：**线程对分配的资源有排他性访问，即每一个资源要么分配给一个线程，要么是可用的。

**2） 占有且等待：**一个线程已经占有至少一个资源，但又在等待另一个资源，而此时该资源被其他线程占有。

**3） 不可剥夺：**线程占有的资源不能被剥夺，资源只能在使用完后由线程自行释放。

**4） 环路等待：**存在一种资源等待的环形链，即线程 A 在等待线程 B 占有的资源，而线程 B 在等待线程 C 占有的资源，....，直到最后一个线程等待线程 A 占有的资源，从而形成一个等待环路。

这四大必要条件，只要能够破坏其一，就能避免死锁，可以采取以下几种措施：

**1） 避免互斥条件：**尽量减少资源的独占性，使用非阻塞同步机制。

**2） 破坏占有且等待：**采用资源预分配策略，即进程一次性请求所需的所有资源。

**3） 破坏不可剥夺：**如果一个进程得不到所需的资源，应释放它所持有的资源，或者使用优先级来剥夺资源。

**4） 破坏环路等待：**对系统中的资源进行排序，每个线程按序请求资源，避免形成环路。

#### 扩展知识

下面详细解释下这几个关键点。

**1）互斥条件：**资源同一时间只能被一个线程所占有，可以通过使用锁（如 `std::mutex`）来确保互斥。

```cpp
std::mutex mtx;
void critical_section() {
 std::lock_guard<std::mutex> lock(mtx);  // 确保互斥访问
 // 临界区代码
}
```

**2）占有且等待：**

- 一个线程可能需要在持有资源 A 的情况下再去请求资源 B，这样就满足了占有且等待的条件。
- 避免这种情况可以用资源一次性分配，确保一个线程在开始执行时已经获得了所有所需资源。

```cpp
std::mutex mtxA, mtxB;
void thread_func() {
  std::unique_lock<std::mutex> lockA(mtxA, std::defer_lock);std::unique_lock<std::mutex> lockB(mtxB, std::defer_lock);
  std::lock(lockA, lockB);  // 脱离单独锁定合并为原子操作，避免死锁
  // 临界区代码
}
```

**3）不可剥夺：**如果一个线程得不到它所需的所有资源，可以释放已占用的资源，然后过一段时间再尝试重新获取。

```cpp
std::mutex mtx1, mtx2;
void thread_func() {
while (true) {
      mtx1.lock();
      if (mtx2.try_lock()) {
          // 获得所需资源，进行处理
          mtx2.unlock();
          mtx1.unlock();
          break;
      } else {
          mtx1.unlock();
          std::this_thread::yield();  // 让出处理器一段时间再重试
      }
  }
}
```

4）环路等待：通过对所有资源进行排序，确保按序请求资源，这样就避免了环形等待。

```cpp
std::mutex mtx1, mtx2;
void thread_func1() {
  std::lock(mtx1, mtx2);  // 遵守资源请求顺序
  std::lock_guard<std::mutex> lock1(mtx1, std::adopt_lock);
  std::lock_guard<std::mutex> lock2(mtx2, std::adopt_lock);
  // 临界区代码
}
void thread_func2() {
  std::lock(mtx1, mtx2);  // 遵守资源请求顺序
  std::lock_guard<std::mutex> lock1(mtx1, std::adopt_lock);
  std::lock_guard<std::mutex> lock2(mtx2, std::adopt_lock);
  // 临界区代码
}
```

通过这些方法就可以有效地避免死锁的发生。需要注意的是，避免死锁是一项非常重要也非常复杂的工作，需要仔细的设计和检查。

**破坏环路等待条件很常见，一般开发过程中，只要我们可以保证资源加锁的顺序是一致的，基本都可以避免死锁的发生。**

![](https://chengxuchu-1301103198.cos.ap-beijing.myqcloud.com/Photo/image-20250511163448150.png)



## 72. C++ 中如何实现一个单例模式？

#### 回答重点

现在最常见的实现单例模式的方法就是使用 `static` 静态局部变量的懒汉模式了，可以归纳为以下几点：

1. 将构造函数、拷贝构造函数和赋值操作符设为 `private`，防止外部模块通过它们创建对象。
2. 在类中提供一个静态的、返回类实例的 `public` 方法。
3. 使用局部静态变量初始化类实例，确保线程安全的懒汉模式。

下面是一个具体的示例代码：

```cpp
class Singleton {
private:
    // 私有构造函数
    Singleton() {}
    
    // 禁止拷贝构造函数和赋值操作符
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

public:
    // 获取唯一实例的静态方法
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }
    
    // 其他成员函数
    void someMemberFunction() {
        // TODO: 实现功能
    }
};
```

这个实现利用了 C++11 局部静态变量的线程安全性，确保多线程环境下仅创建一个实例。

#### 扩展知识

**饿汉模式与懒汉模式：**

- 饿汉模式：实例在程序开始运行时就被创建，常见的做法是直接在类中初始化静态成员。
- 懒汉模式：实例在首次使用时才被创建，节省资源。

上面的代码属于懒汉模式，而且线程安全。

**线程安全：** C++11 之前的局部静态变量并不保证线程安全。在 C++11 及之后，局部静态变量的初始化是线程安全的。此外，也可以使用互斥锁（如 `std::mutex`）来显式保证线程安全。

```cpp
class Singleton {
private:
    Singleton() {}
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
    
    static std::mutex mutex_;

public:
    static Singleton& getInstance() {
        std::lock_guard<std::mutex> lock(mutex_); // C++11 之后可以不用锁
        static Singleton instance;
        return instance;
    }
};

std::mutex Singleton::mutex_;
```

**双重检测机制：** 在某些情况下，双重检测机制也是实现单例模式的常用方法，尤其是针对一些旧版本的多线程系统。

```cpp
class Singleton {
private:
    Singleton() {}
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
    
    static Singleton* instance_;
    static std::mutex mutex_;

public:
    static Singleton* getInstance() {
        if (instance_ == nullptr) {
            std::lock_guard<std::mutex> lock(mutex_);
            if (instance_ == nullptr) { // 双重检测
                instance_ = new Singleton();
            }
        }
        return instance_;
    }
};

Singleton* Singleton::instance_ = nullptr;
std::mutex Singleton::mutex_;
```

**内存管理和实例销毁：** 在需要明确销毁单例对象的情况下，可以使用智能指针（如 `std::unique_ptr`）来管理单例对象的生命周期，或者在程序结束时手动清理。

**单例模式的应用场景：** 单例模式通常用于需要全局访问的系统配置、日志记录器、线程池管理、数据库连接池等场景。

## 73. 🌟 请介绍下 C++ 中的 std::sort 算法？

#### 回答重点

`std::sort` 非常高效，它不单纯是快速排序，而是使用了一种名为 `introspective sort`（内省排序）的算法。

内省排序是快速排序、堆排序和插入排序的结合体，它结合这些算法优点的同时避免它们的缺点，特别是快速排序在最坏情况下的性能下降问题。

> 注意：本题介绍，仅限于 GCC 的源码实现。

#### 扩展知识

**快速排序**：内省排序首先使用快速排序算法。利用快速排序分而治之的特点，通过选取一个 pivot 元素，将数组分为两个子数组，一个包含小于 `pivot` 的元素，另一个包含大于 `pivot` 的元素，然后递归地对这两个子数组进行快速排序。快速排序在平均情况下非常高效，时间复杂度为 O(n log n)。

```cpp
/// This is a helper function for the sort routine.
template <typename _RandomAccessIterator, typename _Size, typename _Compare>
_GLIBCXX20_CONSTEXPR void
__introsort_loop(_RandomAccessIterator __first, _RandomAccessIterator __last,
                 _Size __depth_limit, _Compare __comp) {
  while (__last - __first > int(_S_threshold)) {
    if (__depth_limit == 0) {
      std::__partial_sort(__first, __last, __last, __comp);
      return;
    }
    --__depth_limit;
    _RandomAccessIterator __cut =
        std::__unguarded_partition_pivot(__first, __last, __comp);
    std::__introsort_loop(__cut, __last, __depth_limit, __comp);
    __last = __cut;
  }
}

// sort

template <typename _RandomAccessIterator, typename _Compare>
_GLIBCXX20_CONSTEXPR inline void __sort(_RandomAccessIterator __first,
                                        _RandomAccessIterator __last,
                                        _Compare __comp) {
  if (__first != __last) {
    std::__introsort_loop(__first, __last, std::__lg(__last - __first) * 2,
                          __comp);
    std::__final_insertion_sort(__first, __last, __comp);
  }
}
```

**内省排序**：通过限制快速排序递归深度，避免其最坏情况的性能问题。递归深度的限制基于输入数组的大小，通常是对数组长度取对数然后乘以一个常数（在 GCC 实现中是 `2 * log(len)`）。如果排序过程中递归深度超过了这个限制，算法会切换到堆排序。

```cpp
/// This is a helper function for the sort routine.
template <typename _RandomAccessIterator, typename _Size, typename _Compare>
_GLIBCXX20_CONSTEXPR void
__introsort_loop(_RandomAccessIterator __first, _RandomAccessIterator __last,
                 _Size __depth_limit, _Compare __comp) {
  while (__last - __first > int(_S_threshold)) {
    if (__depth_limit == 0) {
      std::__partial_sort(__first, __last, __last, __comp);
      return;
    }
    --__depth_limit;
    _RandomAccessIterator __cut =
        std::__unguarded_partition_pivot(__first, __last, __comp);
    std::__introsort_loop(__cut, __last, __depth_limit, __comp);
    __last = __cut;
  }
}
```

**堆排序**：当快速排序的递归深度超过限制时，内省排序会切换到堆排序，保证最坏情况下的时间复杂度为 O(n log n)。堆排序不依赖于数据的初始排列，因此它的性能无论在最好、平均和最坏情况下都是稳定的。

```cpp
template <typename _RandomAccessIterator, typename _Compare>
_GLIBCXX20_CONSTEXPR inline void
__partial_sort(_RandomAccessIterator __first, _RandomAccessIterator __middle,
               _RandomAccessIterator __last, _Compare __comp) {
  std::__heap_select(__first, __middle, __last, __comp);
  std::__sort_heap(__first, __middle, __comp);
}
```

**插入排序**：最后，当数组的大小减小到一定程度时，内省排序会使用插入排序来处理小数组。插入排序在小数组上非常高效，尽管它的平均和最坏情况时间复杂度为 O(n^2)，但在数据量小的情况下，这种复杂度不是问题。此外，插入排序是稳定的，可以保持等值元素的相对顺序。

```cpp
/// This is a helper function for the sort routine.
template <typename _RandomAccessIterator, typename _Compare>
_GLIBCXX20_CONSTEXPR void __final_insertion_sort(_RandomAccessIterator __first,
                                                 _RandomAccessIterator __last,
                                                 _Compare __comp) {
  if (__last - __first > int(_S_threshold)) {
    std::__insertion_sort(__first, __first + int(_S_threshold), __comp);
    std::__unguarded_insertion_sort(__first + int(_S_threshold), __last,
                                    __comp);
  } else
    std::__insertion_sort(__first, __last, __comp);
}
```

<table>
    <tr>
    <td>排序算法<br/></td><td>平均时间复杂度<br/></td><td>最坏时间复杂度<br/></td><td>最好时间复杂度<br/></td><td>空间复杂度<br/></td><td>稳定性<br/></td><td>适用场景<br/></td><td>关键特点<br/></td></tr>
    <tr>
    <td> 冒泡排序 <br/></td><td>O(n²)<br/></td><td>O(n²)<br/></td><td>O(n)<br/></td><td>O(1)<br/></td><td>稳定<br/></td><td>小规模数据或基本有序数据<br/></td><td>相邻元素比较交换，每一轮将最大元素“冒泡”到末尾。<br/></td></tr>
    <tr>
    <td> 选择排序 <br/></td><td>O(n²)<br/></td><td>O(n²)<br/></td><td>O(n²)<br/></td><td>O(1)<br/></td><td>不稳定<br/></td><td>小规模数据<br/></td><td>每次选择最小（或最大）元素放到已排序区间末尾。<br/></td></tr>
    <tr>
    <td> 插入排序 <br/></td><td>O(n²)<br/></td><td>O(n²)<br/></td><td>O(n)<br/></td><td>O(1)<br/></td><td>稳定<br/></td><td>小规模或基本有序数据<br/></td><td>将未排序元素插入已排序区间的正确位置。<br/></td></tr>
    <tr>
    <td> 希尔排序 <br/></td><td>O(n log n)<br/></td><td>O(n²)<br/></td><td>O(n log n)<br/></td><td>O(1)<br/></td><td>不稳定<br/></td><td>中等规模数据<br/></td><td>分组插入排序，通过增量序列逐步缩小间隔。<br/></td></tr>
    <tr>
    <td> 归并排序 <br/></td><td>O(n log n)<br/></td><td>O(n log n)<br/></td><td>O(n log n)<br/></td><td>O(n)<br/></td><td>稳定<br/></td><td>大规模数据、外部排序<br/></td><td>分治法，递归拆分后合并有序子序列。<br/></td></tr>
    <tr>
    <td> 快速排序 <br/></td><td>O(n log n)<br/></td><td>O(n²)<br/></td><td>O(n log n)<br/></td><td>O(log n)<br/></td><td>不稳定<br/></td><td>大规模数据（默认最优选择）<br/></td><td>分治法，通过基准值分区，递归排序左右子序列。<br/></td></tr>
    <tr>
    <td> 堆排序 <br/></td><td>O(n log n)<br/></td><td>O(n log n)<br/></td><td>O(n log n)<br/></td><td>O(1)<br/></td><td>不稳定<br/></td><td>大规模数据、需要原地排序<br/></td><td>利用堆结构（大顶堆/小顶堆）进行选择排序。<br/></td></tr>
    <tr>
    <td> 计数排序 <br/></td><td>O(n + k)<br/></td><td>O(n + k)<br/></td><td>O(n + k)<br/></td><td>O(k)<br/></td><td>稳定<br/></td><td>非负整数且范围k较小的数据<br/></td><td>统计元素出现次数，直接计算输出位置。<br/></td></tr>
    <tr>
    <td> 桶排序 <br/></td><td>O(n + k)<br/></td><td>O(n²)<br/></td><td>O(n)<br/></td><td>O(n + k)<br/></td><td>稳定<br/></td><td>均匀分布的数据<br/></td><td>将数据分到有限数量的桶内，每个桶单独排序后合并。<br/></td></tr>
    <tr>
    <td> 基数排序 <br/></td><td>O(n × k)<br/></td><td>O(n × k)<br/></td><td>O(n × k)<br/></td><td>O(n + k)<br/></td><td>稳定<br/></td><td>非负整数或定长字符串<br/></td><td>按位数从低位到高位依次排序（依赖稳定的子排序算法，如计数排序）。<br/></td></tr>
    </table>



## 74. `Reactor` 和 `Proactor` 的区别？

#### 回答重点

`Reactor` 和 `Proactor` 都是用于处理大量网络 IO 操作的编程模式。

它们的主要区别在于如何处理 IO 操作。

- `Reactor` 模式，程序会先注册一些事件处理器，监听需要处理的 IO 事件，例如 `socket ` 读写事件。当这些事件发生时，事件处理程序会通知相应的事件处理器来处理该事件。这种方式通常使用同步 I/O 操作，即程序需要等待 IO 操作完成才能进行下一步操作。
- 在 `Proactor` 模式中，程序也会先注册一些事件处理器来监听需要处理的 IO 事件。但是与 `Reactor` 不同的是，`Proactor` 使用异步 I/O 操作，即程序可以继续执行其他任务而不必等待 IO 操作完成。当 IO 操作完成后，事件处理程序会自动调用相关的回调函数来处理已经就绪的 IO 结果。

#### 扩展知识

由于 `Proactor` 使用异步 I/O 操作，因此它比 `Reactor` 更适合处理大量数据或者需要进行复杂计算的场景。然而，它的实现可能会更加复杂，需要使用回调函数、协程或者异步框架等技术来支持。比如 `asio`。

<table>
    <tr>
    <td> 对比维度 <br/></td><td> Reactor 模式 <br/></td><td> Proactor 模式 <br/></td></tr>
    <tr>
    <td> 核心机制 <br/></td><td>同步非阻塞 I/O（事件驱动）<br/></td><td>异步 I/O（操作完成后回调）<br/></td></tr>
    <tr>
    <td> 工作流程 <br/></td><td>1. 注册事件处理器<br/>2. 监听事件就绪<br/>3. 程序主动完成 I/O 操作<br/></td><td>1. 注册事件处理器和缓冲区<br/>2. 系统完成 I/O 操作<br/>3. 回调通知程序处理结果<br/></td></tr>
    <tr>
    <td> I/O 操作主体 <br/></td><td>应用程序自身（需调用 `read/write`）<br/></td><td>操作系统（内核完成 `read/write`，程序只需处理回调）<br/></td></tr>
    <tr>
    <td> 性能 <br/></td><td>高并发下上下文切换较多<br/></td><td>减少上下文切换，适合高吞吐量场景<br/></td></tr>
    <tr>
    <td> 复杂度 <br/></td><td>实现简单，主流框架（如 Nginx、Redis）广泛使用<br/></td><td>实现复杂，需依赖操作系统异步 I/O 支持（如 Windows IOCP，Linux AIO 不完善）<br/></td></tr>
    <tr>
    <td> 典型应用 <br/></td><td>- Nginx<br/>- Redis<br/>- Java NIO<br/></td><td>- Boost.Asio（跨平台）<br/>- Windows IOCP<br/>- 高性能文件/网络服务<br/></td></tr>
    <tr>
    <td> 适用场景 <br/></td><td>- 连接数多但数据量小<br/>- 需要跨平台兼容性<br/></td><td>- 数据量大或计算密集<br/>- 需极致性能（如金融交易系统）<br/></td></tr>
    </table>

