# CS-RS：计算机科学与C++编程概念入门

涵盖了编程中的一些基本概念，包括基本原理，如过程和抽象化，资源管理和基本数据结构。

内容诸如编程和算法的技巧，自顶向下的分析，结构化编程，测试和程序正确性。程序语言语法以及静态和运行时语义。作用域、过程实例化、递归、抽象数据类型和参数传递方法。结构化数据类型、指针、链接数据结构、堆栈、队列、数组、记录和树。

注意，此处既不深入计算机系统也不关心其原理组成，sicp:一般意义上的计算机科学，与计算机无关。

目的是研究cs最重要的组成：研究编程中的一般化概念。

比如类型系统，函数抽象，数据抽象等。

## AMM

抽象机器模型，既然不关心具体计算机，哪概念上计算机组成有吗？

- 进程，数据，栈，堆，下推自动机，线程，寄存器，异步（非阻塞），同步（阻塞）。

- 机器，OS，shell ，app
- 01 机器码，指令集，汇编语言，编译，链接，高级语言，原语，抽象，组合，first class,函数即数据... eval/apply。

心智模型：

关键：

- 零开销抽象
- 直接用类型和数据结构表示知识 
- 迭代和递归是计算的隐喻 
- 语法糖下是编译器模式匹配（Ocaml）
- 语义：静态类型推导 + 运行时语义
- 过程和数据类型的抽象
- 使用对象类和方法组织和模块化系统 
- 算法设计，但不要重复造轮子。
- 避免UB，思考所有权，生命周期模型下的内存及资源管理。
- 泛型编程，trait, 黑魔法
- 并发
- morden cpp is not c\cpp 

## C++ 语法及语义

类型系统：静态，类型推导，声明，初始化，值，表达式，const, 指针旋转跳跃。

函数模型:

```cpp
void foo() {
  auto x {3};
  double y = 4.1;
  const int z = x;
  x = 5;
}
```

hello world

```cpp
#include <iostream>
int main(){
	std::cout << "Hello world\n";
}
// g++ -std=c++20 -Wall -g -o hello hello.cpp
```

互递归函数

```cpp
#include <iostream>

// never using namespace std;
//互递归

bool odd(int);
bool even(int i)
{
  if (i == 0)
    return true;
  else
    return odd(i - 1);
}

bool odd(int i)
{
  if (i == 0)
    return false;
  else
    return even(i - 1);
}

int main()
{
  std::cout << even(0) << "\n"
            << odd(0) << "\n";
}
```

变量不同于对象，变量强调名称绑定，在源代码体现，对象是内存抽象，与运行时绑定，存在类型和生存期。

变量初始化语义：在 c + + 中，默认值语义

lvalue:对象

rvalue: 值：**const** **int** **&ref** **=** **3;**

```cpp
  auto x{1};
  int &z = x;
  auto y{2};
  x = y;
  z++;
  auto *ptr {&x};

  std::cout << "x:" << x << '\n';
  std::cout << "y:" << y << '\n';
  std::cout << "z:" << z << '\n';
  std::cout << "*ptr:" << *ptr << '\n';
// = 赋值语义是可变性，含有副作用。
//C + + 仅在初始化新变量时支持引用语义。
//变量名称绑定后与内存对象关联到生存期结束。
/* output:
x:3
y:2
z:3
*ptr:3 */
```

栈资源自动管理（*栈向下增长收回*），堆内存手动new delete | 基于内存所有权的读写请求权语义：智能指针

C++传参：参数的求值顺序在 c + + 中是不确定的。【ocaml为从右到左】

- 默认值语义：对于按值传递的参数，复制副本
- 按引用传递的参数：获取对象。可变性不好  【】表示rust\OCaml注解
  - 【mut】 & ,自动deref 
  - const T &
- 传指针

```cpp
void swap(int &x, int &y) {
  int tmp = x;
  x = y;
  y = tmp;
}
// call
swap(a, b)   
```

## 过程抽象

现代语言中最好任何元素都是first class的，

至少需要函数是first class的，但在C中，函数和字符串类型都需要指针才能变成first class.

morden C++ 现在来看函数是能完成过程抽象，但first class function引入了lambda function来完成。

函数抽象是计算机科学精髓：黑盒抽象 + 因信称义（wishful thinking）

知识表述：what + how

程序： 数据流 + 控制流。

计算机程序类似地使用抽象层来构建。当涉及到计算过程时，函数是我们定义过程抽象的机制。函数的用户只需要知道函数做什么，而不需要关心函数如何完成它的任务。用户需要知道函数的接口，包括其实际的代码接口和函数的功能文档。用户不需要关心函数的实现，以及函数如何工作的实际细节。

## C++ 代码 组合机制

模块化编程：C++ 20 支持import 了，不愧为morden python.

模块由文件组成，文件分为.h头文件和.cpp源代码文件.【类比ocaml】，.h负责类型定义，程序接口，其他扩展有.hpp .hxx .cc .cxx。

使用 某个.h模块的源文件只需要知道模块的接口即可。只要接口保持不变，模块的实现就可以随意更改，而不影响使用它的源文件的行为。

```cpp
#include <iostream>
#include "stats.h"

using std::cout;  
using std::endl;  

int main() {
  // must prefix vector with std::
  std::vector<double> data = { 1, 2, 3 };
  // can elide std:: with cout and endl
  cout << mean(data) << endl;
}
```

![image-20220502165939328](C:\Users\aresyang\AppData\Roaming\Typora\typora-user-images\image-20220502165939328.png)

编译：在编译项目时，只将源文件传递给编译器。头文件通过 # include 指令折叠到源文件中。

```
$ g++ --std=c++11 -pedantic -g -Wall -Werror p1_library.cpp stats.cpp main.cpp -o main.exe
```

大项目需要cmake构建工具，没有cargo。

## cpp生态： 

缺乏像cargo 的好东西，需要用cmake, xmake.

库有std，对utf-8支持需要引入库。

为啥要学C++，搞机器人，CV，自动驾驶工业实现就是这华山一条路啊，不想造轮子就得学。

## 测试

测试是编程的第五纵队和教条主义。

Rob Miller：

1. 对付bug的第一道防线就是让它们变得不可能。内存安全+ 类型安全（cpp流下悔恨泪水并羡慕看向ocaml，无视rust）
2. 第二个防御错误的方法是使用能够发现它们的工具。（cpp有ocaml写的自动化形式验证工具就偷着乐吧）
3. 第三个防御错误的方法是让它们立即可见。（result/option 和go 的err都一致看向了C++异常（ps: 非 first class的异常））
4. 第四道防线是广泛的测试。比如单元测试，集成测试和压力测试等。
5. 程序员被迫诉诸调试。
6. 文档及注释原则：前置条件，后置定理

## 继续语法语义

指针

```cpp
int *ptr1 = nullptr;
//对空指针解引用也会导致未定义行为。
// 引用考虑lifetime 集合论： 对象：引用-> T : &'a
auto cc {42};
auto *ptr {&cc} // ptr := p2int: &'a mut cc obj 

```

因为指针本身就是一个对象，所以它可以存在于它所指向的对象的生命周期之后。【UB警告】

![image-20220502173653905](C:\Users\aresyang\AppData\Roaming\Typora\typora-user-images\image-20220502173653905.png)

```cpp
int * get_address(int x) {
  return &x;
}

void print(int val) {
  cout << val << endl;
}

int main() {
  int a = 3;
  int *ptr = get_address(a);
  print(42);
  cout << *ptr << endl;       // UNDEFINED BEHAVIOR
}
//从语义角度，* 把函数变成first class后都是local变量了，栈自动就修改该帧内存的值了
// 引用，避免指针
int * get_address(int &x) {
  return &x;
}

void print(int val) {
  cout << val << endl;
}

int main() {
  int a = 3;
  int *ptr = get_address(a);
  print(42);
  cout << *ptr << endl;       // prints 3
}

```

![image-20220502173827851](C:\Users\aresyang\AppData\Roaming\Typora\typora-user-images\image-20220502173827851.png)

- 可变性和别名是万恶之源，重载真恶心。
- 在类型中使用时，* 表示该类型是指针类型，而 & 表示该类型是引用类型。
- 当在表达式中用作一元前缀运算符时，* 用于解引用指针，而 & 用于获取对象的地址。
- 当用作二进制中缀运算符时，* 是乘法，而 & 是按位运算
- 引用就是实现自动解引用trait的指针:必须初始化引用。

```cpp
int x = 3;
int *ptr = &x;
int &ref = *ptr;

// ptr
void swap_pointed(int *x, int *y) {
  int tmp = *x;
  *x = *y;
  *y = tmp;
}
// call
swap_pointed(&a, &b)
    
// vs ref
void swap(int &x, int &y) {
  int tmp = x;
  x = y;
  y = tmp;
}
// call
swap(a, b) 

```

指针和引用涉及的可变性和别名带来的好日子还在后头呢，后面还会涉及动态内存和类，subtype，继承。



















## rust视角下C++

Rust 中最大的特性是 Ownership，这意味着非原语值在默认情况下被移动，而不是被复制。

C + + 有左值，右值的概念。

在 c + + 中，左值是复制的，而右值是可以移动的，如果类型实际上实现了移动操作。

并且C + + std 库中有一个函数，它允许我们将任何 lvalue 转换为 rvalue，称为 std: : move

比如std: : string 实现了 move 操作。

注意 std: : move 实际上并不移动任何东西，它只是改变了编译器在这个特定位置处理值的方式。

值语义问题在于在栈上大量copy开销很大，所以如同rust ,

值可以被包装到 unique _ ptr (如 Box)或 shared _ ptr (如 Arc)中，这样可以在堆上保留值的单个实例。

- 读远多于写，rust 是 变量默认不可变，C++默认可变， const int & val
- C++ 指针类型前置，需要以*为起点顺时针旋转跳跃阅读类型。
- 后置类型有：void print_full_name() **const** {}
- class： 类可以看成sum type ,product type 拧在一起，集成trait,继承被过度使用，oop 的self 类比ocaml mod组织代码方式理解。
- 所有权霸凌： c + + 将函数传参按值语义默认为拷贝。还牵连operator= 重载，烦死了。即使用std::move(name)传参，foo 的调用者必须做同样的事情来确保移动。
- 
