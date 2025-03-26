系统的动态分配函数new malloc free delete

stl相关

mutex

size_t

右值引用



申请一块较大的内存块，之后将这块内存的管理放在应用层进行，减小系统调用带来的开销

- 减小动态内存分配的开销
- 避免内存碎片
- 降低系统调用频率
- 拥有稳定的分配时间

适用场合：游戏开发中大量小对象、高性能计算、web服务器和数据库服务器处理大量连接和请求

缺点：需要预先分配较大的内存区域，可能浪费一些内存；对大对象的分配并不划算



## cpp基础



### 数据类型

| 数据类型      | 32 位系统   | 64 位系统 | 说明                                             |
| ------------- | ----------- | --------- | ------------------------------------------------ |
| `bool`        | 1           | 1         | 仅能取 `true` 或 `false`                         |
| `char`        | 1           | 1         | 存储单个字符（ASCII 或 Unicode）                 |
| `wchar_t`     | 2 或 4      | 4         | 宽字符类型（存储 Unicode）                       |
| `char16_t`    | 2           | 2         | 16 位字符（UTF-16）                              |
| `char32_t`    | 4           | 4         | 32 位字符（UTF-32）                              |
| `short`       | 2           | 2         | 短整数（通常为 16 位）                           |
| `int`         | 4           | 4         | 标准整数（通常为 32 位）                         |
| `long`        | 4           | 8         | 在 32 位系统中为 4 字节，在 64 位系统中为 8 字节 |
| `long long`   | 8           | 8         | 长整型（通常为 64 位）                           |
| `float`       | 4           | 4         | 单精度浮点数（IEEE 754 标准）                    |
| `double`      | 8           | 8         | 双精度浮点数（IEEE 754 标准）                    |
| `long double` | 8、12 或 16 | 16        | 可能因编译器不同而变化                           |

**指针类型**

| 指针类型                              | 32 位系统 | 64 位系统 | 说明                               |
| ------------------------------------- | --------- | --------- | ---------------------------------- |
| 指向 `char`、`int`、`double` 等的指针 | 4         | 8         | 指针的大小取决于平台地址总线的宽度 |

- `sizeof(type)` 可以用于查看数据类型的具体大小。
- `long` 在 32 位系统中通常是 4 字节，而在 64 位系统中可能是 8 字节（如 Linux）。



### 常量指针与指针常量

**常量指针**

`*p`（指向的值）不能修改，但 `p`（指针本身）可以改变指向其他地址。

```cpp
const int *p;  // 等价于 int const *p;
```

**指针常量**

指针本身不可修改，但它指向的值可以更改

```cpp
int *const p = &a;
```



### define、typedef和inline

#### define

`#define` 是 预处理指令，用于定义 宏，可以用来定义 常量 或 代码片段。它是在 编译前 由 预处理器 进行替换的，而不是由编译器解析的。

- 没有类型检查，可能会导致意想不到的错误。
- 只是 简单的文本替换。
- 无法调试（因为它在编译前被替换）。

```cpp
#define PI 3.14159  // 定义常量
#define SQUARE(x) ((x) * (x))  // 宏函数
```

注意：

```cpp
cout << SQUARE(5 + 1) << endl;  // 期待 (5+1) * (5+1) = 36
```

由于 `#define SQUARE(x) ((x) * (x))`，实际展开是：

```cpp
((5+1) * (5+1))  // 结果：6 * 6 = 36
```

如果 `#define SQUARE(x) (x * x)`，那么：

```cpp
(5+1 * 5+1)  // 结果：5 + 5 + 1 = 11（错误！）
```

使用 `#define` 定义宏时，参数最好用括号括起来，避免运算符优先级问题。



#### typedef

`typedef` 用于给已有的类型 **创建一个新的名称**，让代码更简洁易读。

- 只做类型重命名，不会创建新的数据类型。
- 在编译时生效（不像 `#define` 只是文本替换）。
- 支持复杂类型，可提高可读性。

```cpp
#include <iostream>
using namespace std;

typedef unsigned int uint;  // uint 是 unsigned int 的别名
typedef int* IntPtr;  // IntPtr 代表 int* 类型

int main() {
    uint a = 100;  // 等价于 unsigned int a = 100;
    IntPtr p;  // 等价于 int* p;
    cout << "a: " << a << endl;
    return 0;
}
```

**注意**

```cpp
typedef int* IntPtr;
IntPtr p1, p2;
```

这里的 `p1` 和 `p2` **都是 `int\*` 指针**，因为 `typedef` **直接替换类型**，不会影响变量定义的方式。

**C++11 新特性：`using`**

```cpp
using uint = unsigned int;  // 等价于 typedef unsigned int uint;
using IntPtr = int*;  // 等价于 typedef int* IntPtr;
```

相比 `typedef`，`using` **可读性更好**，并且可以用于 **模板别名**，推荐在 C++ 代码中使用 `using`。



#### inline

`inline` 用于修饰 **函数**，建议编译器在调用该函数时 **直接展开代码**，减少函数调用的开销。

- 减少函数调用开销，提高效率（适用于 短小函数）。
- 由编译器决定是否真正内联（不是强制的）。
- 避免 `#define` 宏的缺点（支持类型检查、调试）。

```cpp
#include <iostream>
using namespace std;

inline int square(int x) {  // 建议编译器内联
    return x * x;
}

int main() {
    cout << "square(5): " << square(5) << endl;  // 结果：25
    return 0;
}
```

**等价于**

```cpp
cout << "square(5): " << (5 * 5) << endl;
```



### new malloc delete free





### const和constexpr

在 C++ 中，`const` 和 `constexpr` 都用于 **声明常量**，但它们的 **使用场景和特性** 有一些关键区别。

#### const

`const` 关键字 **用于声明不可修改的变量**，但它的值 **不一定在编译时已知**，可以在 **运行时初始化**。

- 变量声明后 **不可修改**。
- **可以在运行时初始化**（但如果是全局 `const`，仍然需要编译时初始化）。
- 不能作为数组大小（除非是全局或静态 `const`）。
- **默认是内链接**（`static` 作用域）。

```cpp
#include <iostream>
using namespace std;

int getValue() {
    return 42;
}

int main() {
    int x = 10;
    const int a = 5;  // ✅ 编译时可确定值
    const int b = x;  // ✅ 运行时确定值
    // int arr[a];  // ✅ 可以用作数组大小
    // int arr[b];  // ❌ 错误，b 不是编译时常量

    cout << "a: " << a << ", b: " << b << endl;
    return 0;
}
```

> **注意：** `const int b = x;` **允许使用** `x` 的值进行初始化，而 `constexpr` 不能这样做。

------



#### constexpr

`constexpr` 用于声明 **编译期常量**，**它的值必须在编译时可确定**。

- **必须在编译时初始化**，不能依赖运行时计算的值。
- **可用于数组大小、模板参数等** 需要编译期常量的地方。
- **自动内联（`inline`）**，可以用于 **函数和类成员**。
- **适用于编译期计算的场景**（如 `constexpr` 函数）。

```cpp
#include <iostream>
using namespace std;

constexpr int getConstValue() { return 10; }

int main() {
    constexpr int a = 5;   // ✅ 编译时常量
    constexpr int b = getConstValue();  // ✅ 允许，返回值是 constexpr
    // int x = 10;
    // constexpr int c = x;  // ❌ 错误，x 不能用于 constexpr 初始化

    int arr[a];  // ✅ 可以用作数组大小
    cout << "a: " << a << ", b: " << b << endl;
    return 0;
}
```

> **注意：** `constexpr` 变量的值 **必须能在编译时确定**，所以不能用运行时变量 `x` 进行初始化。



#### constexpr函数

C++11 引入了 `constexpr` **函数**，可以在编译期 **进行计算**。

```cpp
#include <iostream>
using namespace std;

constexpr int square(int x) { return x * x; }

int main() {
    constexpr int result = square(5);  // ✅ 计算在编译期完成
    int x = 10;
    int runtime_result = square(x);  // ✅ 运行时计算
    cout << "result: " << result << ", runtime_result: " << runtime_result << endl;
    return 0;
}
```

> **注意：** `constexpr` 函数在编译时可以被计算，**也可以在运行时使用**（如果传入的参数是运行时变量）。

------



#### const和 constexpr结合

可以将 `constexpr` **返回的值赋给 `const` 变量**：

```cpp
constexpr int getVal() { return 100; }

int main() {
    const int a = getVal();  // ✅ 允许
    constexpr int b = getVal();  // ✅ 允许
}
```

但如果 `getVal()` **不是 `constexpr` 函数**，那么 `constexpr int b = getVal();` **会报错**。



#### 总结

- `const` 变量在 运行时初始化，不能修改，但可以依赖运行时变量。
- `constexpr` 变量必须在编译时初始化，适用于 数组大小、模板参数、编译期优化。
- `constexpr` 函数 可以 在编译时计算，也可以 在运行时使用。
- `constexpr` 更强大，但使用时要确保值可在编译期确定。



### 前置++与后置++

- 前置 `++`（`++obj`）：返回自增后的对象。
- 后置 `++`（`obj++`）：需要一个 int 形参作为区分，并返回自增前的对象。

```cpp
#include <iostream>

class Counter {
private:
    int value;
public:
    Counter(int v) : value(v) {}

    // 前置++ (prefix increment)
    Counter& operator++() {
        ++value;    // 先自增
        return *this; // 返回自增后的对象（引用）
    }

    // 后置++ (postfix increment)
    const Counter operator++(int) { // 注意这里的 int 参数
        Counter temp = *this; // 先保存当前状态
        ++value;   // 自增
        return temp; // 返回原始值
    }

    void print() const {
        std::cout << "Value: " << value << std::endl;
    }
};

int main() {
    Counter c(5);

    ++c;   // 调用前置++
    c.print(); // Value: 6

    c++;   // 调用后置++
    c.print(); // Value: 7

    return 0;
}
```

 1、为什么后置返回对象，⽽不是引⽤ 

因为后置为了返回旧值创建了⼀个临时对象，在函数结束的时候这个对象就会被销毁，如果返回引⽤，那么我请问 你？你的对象对象都被销毁了，你引⽤啥呢？ 

2、为什么后置前⾯也要加const 

其实也可以不加，但是为了防⽌你使⽤i++++,连续两次的调⽤后置++重载符，为什么呢? 原因： 它与内置类型⾏为不⼀致；你⽆法活得你所期望的结果，因为第⼀次返回的是旧值，⽽不是原对象，你调⽤两次后 置++，结果只累加了⼀次，所以我们必须⼿动禁⽌其合法化，就要在前⾯加上const。 

3、处理⽤户的⾃定义类型 最好使⽤前置++，因为他不会创建临时对象，进⽽不会带来构造和析构⽽造成的格外开销。



### 函数指针

```cpp
#include <iostream>

// 定义一个普通函数
int add(int a, int b) {
    return a + b;
}

int main() {
    // 声明函数指针（指向返回值为 int，参数为 (int, int) 的函数）
    int (*funcPtr)(int, int);

    // 赋值（将 add 函数的地址赋给 funcPtr）
    funcPtr = add;

    // 通过函数指针调用函数
    std::cout << "Result: " << funcPtr(3, 5) << std::endl;  // 输出 8

    return 0;
}

```

#### 要点

1. **函数指针的声明格式**：

   ```cpp
   返回类型 (*指针变量名)(参数类型);
   ```

   例如：`int (*funcPtr)(int, int);`

2. **赋值时可以省略 `&`**，即 `funcPtr = add;` 等价于 `funcPtr = &add;`

3. **调用方式**：

   - `funcPtr(3, 5);`  ✅（推荐）
   - `(*funcPtr)(3, 5);`  ✅（等价）



#### 应用

**（1）函数指针作为参数（回调函数）**

函数指针可以作为参数传递，使函数的行为更加灵活。

```cpp
#include <iostream>

// 定义两个运算函数
int add(int a, int b) { return a + b; }
int multiply(int a, int b) { return a * b; }

// 计算函数，接收一个函数指针作为参数
int calculate(int x, int y, int (*operation)(int, int)) {
    return operation(x, y);  // 调用传入的函数
}

int main() {
    std::cout << "Add: " << calculate(4, 5, add) << std::endl;         // 9
    std::cout << "Multiply: " << calculate(4, 5, multiply) << std::endl; // 20
    return 0;
}
```

📌 **优点**：

- **`calculate`** 函数可以使用不同的运算函数，提高代码复用性。
- **回调函数**：适用于排序、自定义操作等场景。

------

**（2）数组 + 函数指针（函数表/跳转表）**

可以用函数指针数组创建**函数表**，让程序动态选择要执行的函数。

```cpp
include <iostream>

// 定义三个操作函数
int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }

int main() {
    // 函数指针数组
    int (*operations[])(int, int) = {add, subtract, multiply};

    // 选择运算方式
    int choice = 2;  // 0 = add, 1 = subtract, 2 = multiply

    std::cout << "Result: " << operations[choice](10, 5) << std::endl;  // 50 (调用 multiply)
    
    return 0;
}
```

📌 **优势**：

- **类似 `switch` 语句，但更灵活**，可以动态扩展。
- **常用于命令解析、菜单系统、事件回调等**。

------

**（3）指向成员函数的指针**

C++ 允许函数指针指向**类的成员函数**，但**成员函数指针的语法不同**：

```cpp
#include <iostream>

class Calculator {
public:
    int add(int a, int b) { return a + b; }
};

int main() {
    Calculator obj;
    
    // 定义成员函数指针
    int (Calculator::*funcPtr)(int, int) = &Calculator::add;

    // 通过对象调用成员函数指针
    std::cout << "Result: " << (obj.*funcPtr)(3, 7) << std::endl;  // 10
    
    // 通过指针调用
    Calculator* pObj = &obj;
    std::cout << "Result: " << (pObj->*funcPtr)(3, 7) << std::endl;  // 10

    return 0;
}
```

📌 **要点**：

- **成员函数指针的定义**：

  ```cpp
  返回类型 (类名::*指针变量)(参数列表) = &类名::成员函数;
  ```

- **调用方式**：

  - `对象.*指针(参数);`
  - `对象指针->*指针(参数);`
  - **调用成员函数指针时，`.\*` 和 `->\*` 是必须的**，不能省略 `*` 号

------

**（4）使用 `std::function` 代替函数指针（C++11+）**

C++11 提供了 `std::function`，可以存储普通函数、lambda、成员函数等，可读性更好。

```cpp
#include <iostream>
#include <functional>  // 引入 std::function

int add(int a, int b) { return a + b; }

int main() {
    std::function<int(int, int)> funcPtr = add;
    
    std::cout << "Result: " << funcPtr(4, 6) << std::endl;  // 10
    return 0;
}
```

📌 **优势**：

- 可以存储**普通函数、lambda 表达式、成员函数**等。
- 更加**灵活和安全**，避免函数指针的复杂语法。



### 强制类型转换

在 C++ 中，提供了 4 种类型转换关键字，分别是 `static_cast`、`dynamic_cast`、`reinterpret_cast` 和 `const_cast`。相比 C 语言的强制转换（`(type)value`），C++ 的这 4 种转换方式提供了更强的**类型安全性**和**可读性**。



#### 1. `static_cast`（静态转换）

`static_cast` 主要用于**编译期可以确定的转换**，比如基本类型转换、父子类指针转换（非虚函数的情况）。

✅ **基本数据类型转换** ✅ **子类指针 → 父类指针** ✅ **父类指针 → 子类指针（不安全，需开发者保证正确性）** ✅ **void\* 指针转换为具体类型指针**

```cpp
#include <iostream>

class Base {};
class Derived : public Base {};

int main() {
    // 基本类型转换
    double d = 3.14;
    int i = static_cast<int>(d);  // ✅ 转换 double → int
    std::cout << "i = " << i << std::endl;

    // 子类指针转换为父类指针（安全）
    Derived dObj;
    Base* basePtr = static_cast<Base*>(&dObj);  // ✅ 子类 → 父类

    // 父类指针转换为子类指针（不安全，需保证是正确的子类对象）
    Base* bPtr = new Derived();
    Derived* dPtr = static_cast<Derived*>(bPtr);  // ❗不检查安全性，需开发者保证 bPtr 指向 Derived

    return 0;
}
```

📌 **注意**：

- `static_cast` **不会执行运行时检查**，如果 `bPtr` 指向的对象不是 `Derived`，转换会导致**未定义行为**（UB）。
- 不能用于不同类型对象之间的转换，比如 `Base` → `int`，这会导致编译错误。



#### 2. `dynamic_cast`（动态转换）

`dynamic_cast` 主要用于**含有虚函数的多态类之间的安全转换**，依赖 **RTTI（运行时类型识别）**。

✅ **父类指针（或引用） → 子类指针（或引用）** ✅ **必须是多态类（基类要有虚函数）** ✅ **转换失败时，指针返回 `nullptr`，引用抛 `std::bad_cast` 异常**

```cpp
#include <iostream>
#include <typeinfo>

class Base {
public:
    virtual void show() {}  // 必须有虚函数，保证是多态类
};

class Derived : public Base {};

class OtherClass {};

int main() {
    Base* basePtr = new Derived();
    Derived* derivedPtr = dynamic_cast<Derived*>(basePtr);  // ✅ 安全转换

    if (derivedPtr) {
        std::cout << "Conversion successful!" << std::endl;
    } else {
        std::cout << "Conversion failed!" << std::endl;
    }

    // 错误转换（转换失败，返回 nullptr）
    Base* basePtr2 = new Base();
    Derived* derivedPtr2 = dynamic_cast<Derived*>(basePtr2);  // ❌ 失败，basePtr2 指向的是 Base
    if (!derivedPtr2) {
        std::cout << "Conversion failed!" << std::endl;
    }

    return 0;
}
```

📌 **注意**：

- `Base` 必须是**多态类**（有至少一个 `virtual` 函数）。

- ```
  dynamic_cast
  ```

  有运行时类型检查，如果转换失败：

  - **指针** 返回 `nullptr`
  - **引用** 抛 `std::bad_cast` 异常

- 比 `static_cast` **更安全，但有性能损耗**，因为它依赖 RTTI。



#### 3. `reinterpret_cast`（重新解释转换）

`reinterpret_cast` 用于**几乎任意类型之间的转换**，不会检查安全性。

✅ **指针类型间转换（如 `int\*` ↔ `char\*`）** ✅ **整数 ↔ 指针** ✅ **函数指针转换** ❌ **不要用于类层次结构之间的转换（`Base` ↔ `Derived`），可能导致 UB**

```cpp
#include <iostream>

int main() {
    int a = 65;
    char* c = reinterpret_cast<char*>(&a);  // ✅ int* → char*
    std::cout << "Char value: " << *c << std::endl;  // 可能输出 'A'（ASCII 65）

    // int 转指针
    int* ptr = reinterpret_cast<int*>(0x12345678);
    std::cout << "ptr: " << ptr << std::endl;  // 可能导致 UB

    return 0;
}
```

📌 **注意**：

- `reinterpret_cast` **不会进行类型检查**，完全是**二进制级别**的转换。
- 可能导致**未定义行为（UB）**，需开发者确保合理使用。



#### 4. `const_cast`（去除 `const` 限定）

`const_cast` 主要用于**去除 `const` 或 `volatile` 修饰**，使得可修改常量。

✅ **去除 `const`，让原本 `const` 对象可修改** ✅ ==**常用于需要修改 `const` 参数的旧 API**== ❌ **不能用于 `const` 变量的直接修改，否则 UB**

```cpp
#include <iostream>

void modify(const int* ptr) {
    int* modifiable = const_cast<int*>(ptr);  // ✅ 去掉 const
    *modifiable = 100;
}

int main() {
    int x = 10;
    modify(&x);
    std::cout << "Modified x: " << x << std::endl;  // ✅ 输出 100

    // ❌ 错误用法（修改真正的常量，UB）
    const int y = 20;
    int* p = const_cast<int*>(&y);
    *p = 30;  // ❌ UB，不要修改 const 变量

    return 0;
}
```

📌 **注意**：

- `const_cast` **只能移除 `const`，不能用于类型转换**。
- **如果 `const` 变量存放在只读内存区，修改它可能会导致崩溃（UB）**。



#### 5. 总结

| 类型               | 用途                       | 运行时检查 | 安全性     | 典型用法                                   |
| ------------------ | -------------------------- | ---------- | ---------- | ------------------------------------------ |
| `static_cast`      | 编译期可确定的转换         | ❌ 无       | ⚠️ 相对安全 | 基本类型、父子类指针转换                   |
| `dynamic_cast`     | 多态类型转换               | ✅ 有       | ✅ 高安全性 | 运行时检查 RTTI，安全转换父类 ↔ 子类       |
| `reinterpret_cast` | 任意类型转换               | ❌ 无       | ❌ 不安全   | 指针类型、整数 ↔ 指针、函数指针转换        |
| `const_cast`       | 移除 `const/volatile` 修饰 | ❌ 无       | ⚠️ 可能 UB  | 修改 `const` 变量（仅用于非 `const` 变量） |

📌 **最佳实践**

- ✅ **优先使用 `static_cast`，避免 `reinterpret_cast`**
- ✅ **多态类型转换应使用 `dynamic_cast`**
- ✅ **尽量避免 `const_cast` 修改 `const` 变量**

这 4 种转换方式比 C 风格的 `(T)value` 更安全、可读性更好，能帮助开发者更清晰地管理类型转换！



### cpp内存管理

C++ 内存大致分为以下几个区域：

| **内存区域**                    | **特点**             | **典型内容**                                                 |
| ------------------------------- | -------------------- | ------------------------------------------------------------ |
| **栈（Stack）**                 | 由系统自动分配和释放 | ==局部变量、函数参数、返回地址==                             |
| **堆（Heap）**                  | 由程序员分配和释放   | `new`/`malloc` 创建的动态对象，程序员手动分配和释放          |
| **静态存储区（Static/Global）** | 程序开始到结束       | `static` 变量、全局变量、常量字符串，程序运行期间一直存在，直到程序结束。**自动初始化**，全局变量和 `static` 变量默认初始化为 0。 |
| **代码段（Code Segment）**      | 只读、存放可执行代码 | 可执行程序指令，**只读**，防止程序意外修改代码               |



### 智能指针

智能指针（Smart Pointer）是 C++ 标准库提供的 **自动管理内存** 的指针对象，能够在不需要时 **自动释放内存**，从而防止内存泄漏和悬浮指针问题。

C++ 标准库提供了 三种主要的智能指针（位于 `<memory>` 头文件中）：

- `std::unique_ptr`（独占所有权）
- `std::shared_ptr`（引用计数共享）
- `std::weak_ptr`（弱引用，避免循环引用）



#### `std::unique_ptr`（独占所有权）

- **特点**
  - **唯一所有权**：`unique_ptr` 只能有一个智能指针管理同一个对象，不能共享。
  - **自动释放**：`unique_ptr` 在离开作用域时自动释放内存。
  - **不可复制**，但**可以移动**（`std::move`）。
- **示例**

```cpp
#include <iostream>
#include <memory>

class Test {
public:
    Test() { std::cout << "Constructor\n"; }
    ~Test() { std::cout << "Destructor\n"; }
};

int main() {
    std::unique_ptr<Test> ptr1 = std::make_unique<Test>();  // ✅ 创建对象
    // std::unique_ptr<Test> ptr2 = ptr1;  ❌ 错误，unique_ptr 不能复制

    std::unique_ptr<Test> ptr2 = std::move(ptr1);  // ✅ 转移所有权
    if (!ptr1) std::cout << "ptr1 is now nullptr\n";

    return 0;
}  // ✅ ptr2 离开作用域，自动释放对象
```

- `std::make_unique<T>()` 推荐用法

  - `std::make_unique<T>()` **比 `new` 更安全**，可以防止异常时的内存泄漏：

  ```cpp
  std::unique_ptr<int> p = std::make_unique<int>(10);  // ✅ 推荐
  ```

  而 

  ```
  std::unique_ptr<int> p(new int(10));
  ```

   可能导致异常时内存泄漏。



####  `std::shared_ptr`（引用计数共享）

- **特点**
  - **多个 `shared_ptr` 可以共享同一对象**，使用**引用计数**（Reference Counting）。
  - **当最后一个 `shared_ptr` 离开作用域时，自动释放对象**。
  - **适用于多个对象共享资源的情况**。
- **示例**

```cpp
#include <iostream>
#include <memory>

class Test {
public:
    Test() { std::cout << "Constructor\n"; }
    ~Test() { std::cout << "Destructor\n"; }
};

int main() {
    std::shared_ptr<Test> ptr1 = std::make_shared<Test>();  // ✅ 共享所有权
    std::shared_ptr<Test> ptr2 = ptr1;  // ✅ 引用计数 +1

    std::cout << "Reference count: " << ptr1.use_count() << "\n";  // 输出 2
    ptr1.reset();  // ✅ 释放 ptr1，引用计数 -1
    std::cout << "Reference count: " << ptr2.use_count() << "\n";  // 输出 1

    return 0;
}  // ✅ ptr2 离开作用域，引用计数变 0，释放对象
```

- `std::make_shared<T>()` 推荐用法

  ```cpp
  std::shared_ptr<int> p = std::make_shared<int>(10);  // ✅ 推荐
  ```

  ```
  std::make_shared<T>()
  ```

  减少了额外的内存分配，比 

  ```
  new
  ```

   更高效。



#### `std::weak_ptr`（弱引用，避免循环引用）

- **特点**
  - `weak_ptr` **不会增加引用计数**，只对 `shared_ptr` 进行**非拥有性引用**。
  - **用于避免 `shared_ptr` 循环引用**（比如 `std::shared_ptr<A>` 和 `std::shared_ptr<B>` 互相持有对方）。
  - `weak_ptr` **不能直接使用**，需要使用 `.lock()` 转换为 `shared_ptr`。
- **示例（避免循环引用）**

```cpp
#include <iostream>
#include <memory>

class B;  // 前向声明

class A {
public:
    std::shared_ptr<B> b_ptr;
    ~A() { std::cout << "A destroyed\n"; }
};

class B {
public:
    std::weak_ptr<A> a_ptr;  // ❗️使用 weak_ptr 解决循环引用
    ~B() { std::cout << "B destroyed\n"; }
};

int main() {
    std::shared_ptr<A> a = std::make_shared<A>();
    std::shared_ptr<B> b = std::make_shared<B>();

    a->b_ptr = b;
    b->a_ptr = a;  // ✅ 使用 weak_ptr，避免循环引用

    return 0;  // ✅ 由于 weak_ptr，不会导致内存泄漏
}
```

📌 **如果 `B` 也使用 `shared_ptr<A>`，对象不会释放，导致** **内存泄漏（循环引用）**！



#### 智能指针 VS 原始指针

| **比较项**   | **智能指针（Smart Pointer）**        | **原始指针（Raw Pointer）** |
| ------------ | ------------------------------------ | --------------------------- |
| **所有权**   | `unique_ptr` 独占, `shared_ptr` 共享 | 仅存储地址，无所有权        |
| **生命周期** | **自动释放内存**                     | 需手动 `delete`             |
| **异常安全** | **安全**，防止内存泄漏               | 可能泄漏                    |
| **引用计数** | `shared_ptr` 有引用计数              | 无                          |
| **循环引用** | 可能存在，但 `weak_ptr` 可解决       | 不存在                      |



#### 什么时候使用智能指针？

| **场景**              | **推荐智能指针**                                   |
| --------------------- | -------------------------------------------------- |
| 独占所有权            | `std::unique_ptr<T>`                               |
| 共享所有权            | `std::shared_ptr<T>`                               |
| 避免循环引用          | `std::weak_ptr<T>`                                 |
| 资源管理              | `std::unique_ptr<T>`（文件、Socket、数据库连接等） |
| 避免手动 `new/delete` | `std::make_unique<T>()` / `std::make_shared<T>()`  |



#### 总结

1. `std::unique_ptr`（**独占所有权**）—— **不允许拷贝，可移动**，生命周期结束时自动释放。
2. `std::shared_ptr`（**引用计数**）—— **多个指针共享资源**，最后一个指针释放资源。
3. `std::weak_ptr`（**弱引用**）—— **不影响引用计数**，**解决 `shared_ptr` 循环引用问题**。
4. **推荐使用 `std::make_unique<T>()` 和 `std::make_shared<T>()` 创建智能指针**，比 `new` 更安全高效。

💡 **建议始终优先使用智能指针，避免手动 `new/delete`，提高代码的安全性和可维护性！** 🚀



### cpp面向对象

三大特征：继承、封装、多态（覆盖、重载）

访问修饰符：private、public、protected

#### 多重继承

什么时候使用 `virtual` 继承？

- 当多个基类继承同一个基类，并且派生类又多重继承这些基类时，应该使用 `virtual` 继承。
- 防止“菱形继承”导致的多份基类实例问题。

```cpp
#include <iostream>

class A {
public:
    int value;
    A(int v) : value(v) { std::cout << "A constructed\n"; }
};

class B : virtual public A {
public:
    B() : A(20) { std::cout << "B constructed\n"; }
};

class C : virtual public A {
public:
    C() : A(30) { std::cout << "C constructed\n"; }
};

class D : public B, public C {
public:
    D() : A(40) { std::cout << "D constructed\n"; }
};

int main() {
    D d;
    std::cout << "A::value in D: " << d.value << std::endl;
    return 0;
}
```

```css
A constructed   ✅ 由 D 构造 A
B constructed
C constructed
D constructed
A::value in D: 40
```



#### 多态实现

**多态（Polymorphism）** 是 C++ 面向对象编程（OOP）的三大特性之一（封装、继承、多态）。它允许 **父类指针或引用** 调用 **子类的重写方法**，从而实现 **动态绑定（Dynamic Binding）**。

**多态的核心机制：**

1. **虚函数（Virtual Function）**
2. **虚函数表（Virtual Table, vtable）**
3. **虚指针（vptr）**



**示例：不使用虚函数（静态绑定）**

```cpp
#include <iostream>

class Base {
public:
    void show() { std::cout << "Base::show()" << std::endl; }
};

class Derived : public Base {
public:
    void show() { std::cout << "Derived::show()" << std::endl; }
};

int main() {
    Base* ptr = new Derived();
    ptr->show();  // 输出：Base::show()  ❌ 发生静态绑定，调用的是 Base 版本
    delete ptr;
    return 0;
}
```

**❌ `ptr->show()` 调用的是 `Base::show()`，而不是 `Derived::show()`。**
 **原因**：

- `show()` **不是虚函数**，C++ **编译时（编译期）决定调用哪个版本（静态绑定）**。
- **调用的是指针的类型 `Base\*` 的函数，而不是 `Derived` 版本。**



**使用虚函数实现多态（动态绑定）**

```cpp
#include <iostream>

class Base {
public:
    virtual void show() { std::cout << "Base::show()" << std::endl; } // 虚函数
};

class Derived : public Base {
public:
    void show() override { std::cout << "Derived::show()" << std::endl; } // 重写
};

int main() {
    Base* ptr = new Derived();
    ptr->show();  // ✅ 输出：Derived::show()  （动态绑定）
    delete ptr;
    return 0;
}
```

- `Base::show()` 是 **虚函数**，C++ **在运行时决定调用哪一个版本**（动态绑定）。
- `ptr` 指向 `Derived` 对象，因此 `ptr->show()` 调用 **Derived 版本的 `show()`**。
- 加上override关键字可以更好地帮助编译器检查。



**vtable（虚函数表）**

- **每个拥有虚函数的类** 都会有一个 **虚函数表（vtable）**。
- vtable 是一个 **指针数组**，**存储该类的虚函数地址**。

**vptr（虚指针）**

- **每个对象** 都有一个 **隐藏的指针（vptr）**，指向 **该类的 vtable**。

```cpp
class Base {
public:
    virtual void func1() { std::cout << "Base::func1()" << std::endl; }
    virtual void func2() { std::cout << "Base::func2()" << std::endl; }
};

class Derived : public Base {
public:
    void func1() override { std::cout << "Derived::func1()" << std::endl; }
};
```

**vtable 结构**

```css
Base vtable:
+-----------------+
| Base::func1()  |
| Base::func2()  |
+-----------------+

Derived vtable:
+-----------------+
| Derived::func1()  |  (覆盖 Base::func1())
| Base::func2()     |  (继承 Base::func2())
+-----------------+

对象 (Derived)
+-------------------+
| vptr -> Derived vtable |
+-------------------+
```

📌 **Derived 只重写 `func1()`，所以 `func2()` 仍然指向 `Base::func2()`！**



**纯虚函数和抽象类**

- **纯虚函数没有具体实现**，派生类**必须重写**它。
- **有纯虚函数的类叫抽象类，不能实例化**。

```cpp
#include <iostream>

class Base {
public:
    virtual void show() = 0;  // 纯虚函数
};

class Derived : public Base {
public:
    void show() override { std::cout << "Derived::show()" << std::endl; }
};

int main() {
    // Base b; ❌ 错误，抽象类不能实例化
    Derived d;
    d.show();  // ✅ 输出：Derived::show()
    return 0;
}
```



**虚析构函数**

为什么要虚析构，为什么不能虚构造？

通常，如果⼀个类可能被继承，且在其派生类中有可能使用该基类的析构函数应该声明为虚析构函数。

- 如果基类的析构函数不是虚函数，当**使用基类指针删除子类对象时**，只会调用基类的析构函数，而不会调用子类的析构函数，导致子类的资源没有正确释放。
- 构造函数不能是虚的，因为构造时还没有 vtable，无法通过虚表找到正确的构造函数。

**如果基类的析构函数是虚函数**，则在删除对象时，会**先调用子类的析构函数，再调用基类的析构函数**，确保资源正确释放。

```cpp
#include <iostream>

class Base {
public:
    Base() { std::cout << "Base Constructor\n"; }
    virtual ~Base() { std::cout << "Base Destructor\n"; }  // ✅ 设为虚函数
};

class Derived : public Base {
public:
    Derived() { std::cout << "Derived Constructor\n"; }
    ~Derived() { std::cout << "Derived Destructor\n"; }  // 现在会被正确调用
};

int main() {
    Base* obj = new Derived();  // 基类指针指向子类对象
    delete obj;  // ✅ 先调用 Derived 析构函数，再调用 Base 析构函数
}
```

```css
Base Constructor
Derived Constructor
Derived Destructor
Base Destructor
```

✅ **结果**：

- **先调用子类 `Derived` 的析构函数，再调用基类 `Base` 的析构函数**，确保资源正确释放。

哪些函数不能被声明为虚函数：构造函数、普通函数、静态成员函数、友元函数、内联成员函数。



#### 友元

**友元函数**

- 允许一个普通函数访问类的私有成员。
- 该函数不是类的成员，但可以访问类的私有数据。

```cpp
#include <iostream>
using namespace std;

class A {
private:
    int x;

public:
    A(int val) : x(val) {}

    // 友元函数声明
    friend void show(const A& obj);
};

// 友元函数定义
//常引用（const &） 是 C++ 中的一种特殊的引用类型，它的主要作用是 防止被引用的值被修改，提高程序的安全性和可读性。
void show(const A& obj) {
    cout << "Private x = " << obj.x << endl;  // ✅ 可以访问私有变量 x
}

int main() {
    A a(10);
    show(a);  // 调用友元函数
    return 0;
}

```

**友元类**

- 允许一个类访问另一个类的私有和保护成员。
- 适用于两个类关系紧密，但不希望把数据暴露给所有类的情况。

```cpp
#include <iostream>
using namespace std;

class B;  // 先声明类 B，避免错误

class A {
private:
    int x;

public:
    A(int val) : x(val) {}

    // 友元类声明
    friend class B;  
};

class B {
public:
    void show(const A& obj) {
        cout << "Accessing private x from class A: " << obj.x << endl;  // ✅ 访问私有变量
    }
};

int main() {
    A a(20);
    B b;
    b.show(a);  // 通过 B 访问 A 的私有成员
    return 0;
}

```

**友元成员函数**

- 只让另一个类的某个特定成员函数访问私有数据，而不是整个类。
- 比友元类的权限更小，更符合封装性。

```cpp
#include <iostream>
using namespace std;

class A {
private:
    int x;

public:
    A(int val) : x(val) {}

    // 让 B 的 `show()` 成员函数成为友元
    friend void B::show(const A& obj);
};

class B {
public:
    void show(const A& obj) {
        cout << "B accessing A's private x: " << obj.x << endl;  // ✅ 访问 A 的私有成员
    }
};

int main() {
    A a(30);
    B b;
    b.show(a);
    return 0;
}

```

**常见应用：重载`<<`操作符**

对`operator<<`函数进行重载，原本是一个全局函数（非成员函数），如果没有特殊的机制，它不能直接访问类的私有成员

```cpp
#include <iostream>
using namespace std;

class A {
private:
    int x;

public:
    A(int val) : x(val) {}

    // 友元函数声明
    friend ostream& operator<<(ostream& os, const A& obj);
};

// 友元函数定义
ostream& operator<<(ostream& os, const A& obj) {
    os << "x = " << obj.x;  // ✅ 访问私有成员
    return os;
}

int main() {
    A a(100);
    cout << a << endl;  // ✅ 友元函数重载 <<，可以直接打印私有成员
    return 0;
}
```





#### 拷贝构造函数

拷贝构造函数的定义格式：

```cpp
ClassName(const ClassName& other);
```

- **`const ClassName& other`**：参数是对已有对象的**常引用**，避免无意义的递归拷贝。
- 如果是`ClassName(const ClassName other)`，other传递时要创建一个临时对象，则再次调用拷贝构造函数导致无限递归。
- **不返回值**，因为它是构造函数。



**浅拷贝（默认拷贝构造函数）**

- 默认拷贝构造函数 进行 “按位复制”，如果对象包含指针成员，可能导致重复释放同一块内存（悬空指针问题）。

**深拷贝（自定义拷贝构造函数）**

- 深拷贝（Deep Copy） 需要分配新内存，确保每个对象有自己的副本。

如果不希望对象被拷贝，使用 `= delete`，或者使用 `std::unique_ptr` 代替原始指针。





## cpp STL



### 简介

C++ 的 **STL（Standard Template Library，标准模板库）** 是一个功能强大且高效的库，它提供了一系列通用算法和数据结构，使 C++ 编程更加简洁和高效。STL 主要由以下几个部分组成：

------

**1. 容器（Containers）**

容器是 STL 中用于存储数据的数据结构，根据底层实现可分为 **序列式容器** 和 **关联式容器**。

**（1）序列式容器**

序列式容器（Sequential Containers）按照元素的插入顺序存储数据：

- `vector<T>`：动态数组，支持快速随机访问，适合频繁读取操作。
- `deque<T>`：双端队列，支持头尾快速插入和删除。
- `list<T>`：双向链表，适合频繁的插入、删除操作（中间位置）。
- `forward_list<T>`：单向链表，内存占用更少，但只支持单向遍历。
- `array<T, N>`：固定大小的数组，封装了普通 C++ 数组的功能。
- `string`：专门用于存储和操作字符串的类。

**（2）关联式容器**

关联式容器（Associative Containers）采用 **平衡二叉搜索树（如红黑树）** 进行存储，支持高效的查找、插入、删除操作：

- `set<T>`：集合，不允许重复元素，元素默认按 **升序** 排序。
- `multiset<T>`：允许重复元素的集合。
- `map<K, V>`：键值对（映射），不允许重复键，按 **键升序** 排序。
- `multimap<K, V>`：允许重复键的映射。

**（3）无序容器**

无序容器（Unordered Containers）基于 **哈希表（hash table）** 实现，查找速度更快（O(1) 平均时间复杂度）：

- `unordered_set<T>`：无序集合，不允许重复元素。
- `unordered_multiset<T>`：无序集合，允许重复元素。
- `unordered_map<K, V>`：无序映射，不允许重复键。
- `unordered_multimap<K, V>`：无序映射，允许重复键。

------

**2. 迭代器（Iterators）**

迭代器用于遍历 STL 容器中的元素，类似指针，可以支持不同的遍历方式：

- **输入迭代器**（Input Iterator）：只能从头到尾单向读取，如 `istream_iterator`。
- **输出迭代器**（Output Iterator）：只能向容器写入数据，如 `ostream_iterator`。
- **前向迭代器**（Forward Iterator）：支持单向遍历，能多次读取相同元素，如 `forward_list`。
- **双向迭代器**（Bidirectional Iterator）：支持双向遍历，如 `list`、`set`。
- **随机访问迭代器**（Random Access Iterator）：支持类似数组的随机访问，如 `vector`、`deque`。

------

**3. 算法（Algorithms）**

STL 提供了大量的通用算法，如：

- **排序**：`sort()`、`stable_sort()`。
- **查找**：`find()`、`binary_search()`。
- **变换**：`transform()`。
- **计数**：`count()`。
- **删除/替换**：`remove()`、`replace()`。
- **合并/分割**：`merge()`、`partition()`。

示例：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 9};
    std::sort(vec.begin(), vec.end());  // 升序排序

    for (int num : vec) {
        std::cout << num << " ";
    }
    return 0;
}
```

------

**4. 仿函数（Functors）**

仿函数（函数对象）是重载 `operator()` 的类或结构体，主要用于定制 STL 算法：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

struct Compare {
    bool operator()(int a, int b) { return a > b; }  // 降序
};

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 9};
    std::sort(vec.begin(), vec.end(), Compare());

    for (int num : vec) {
        std::cout << num << " ";
    }
    return 0;
}
```

------

**5. 适配器（Adapters）**

适配器是对已有 STL 组件的封装，使其行为发生变化：

- **容器适配器**：`stack`、`queue`、`priority_queue`。
- **迭代器适配器**：`reverse_iterator`、`insert_iterator`。
- **函数适配器**：`bind()`、`not1()`、`not2()`。

示例：

```cpp
#include <iostream>
#include <stack>

int main() {
    std::stack<int> s;
    s.push(10);
    s.push(20);
    s.push(30);

    while (!s.empty()) {
        std::cout << s.top() << " ";  // 30 20 10
        s.pop();
    }
    return 0;
}
```



### vector

`vector` 是 C++ STL（标准模板库）中最常用的 **序列式容器**，它实现了 **动态数组**，支持高效的 **随机访问** 和 **自动扩容**，非常适合存储和操作连续存储的数据。

------

#### **1. `vector` 的特点**

- **动态扩展**：`vector` 会在容量不足时自动扩展，通常是 **2 倍增长**。
- **连续存储**：底层实现是 **动态数组**，所有元素都存储在 **连续的内存块** 中，因此可以像普通数组一样通过索引访问元素。
- **快速随机访问**：支持 **O(1)** 时间复杂度的随机访问，即 `vec[i]`。
- **支持增删改查**：
  - 在 **末尾插入、删除** 元素（`push_back()`、`pop_back()`）速度快（O(1)）。
  - **中间插入、删除** 需要 **移动数据**，时间复杂度是 O(n)。
- **可与标准算法兼容**：支持 STL 算法，如 `sort()`、`find()` 等。

------

#### **2. `vector` 的常见操作**

##### **（1）定义和初始化**

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec1;               // 默认构造空 vector
    vector<int> vec2(5);            // 5 个元素，默认初始化为 0
    vector<int> vec3(5, 10);        // 5 个元素，每个值都是 10
    vector<int> vec4 = {1, 2, 3};   // 列表初始化
    vector<int> vec5(vec4);         // 拷贝构造

    return 0;
}
```

------

##### **（2）基本操作**

| 操作             | 方法                 | 说明                   |
| ---------------- | -------------------- | ---------------------- |
| **增**           | `push_back(x)`       | 末尾添加元素           |
| **插**           | `insert(it, x)`      | 在指定位置插入         |
| **删**           | `pop_back()`         | 删除末尾元素           |
| **删**           | `erase(it)`          | 删除指定位置元素       |
| **改**           | `vec[i] = x`         | 修改指定位置元素       |
| **查**           | `vec[i]`             | 访问元素               |
| **查**           | `at(i)`              | 安全访问（带边界检查） |
| **大小**         | `size()`             | 获取元素个数           |
| **容量**         | `capacity()`         | 获取当前容量           |
| **清空**         | `clear()`            | 清空所有元素           |
| **判断是否为空** | `empty()`            | 返回 `true` 或 `false` |
| **前后元素**     | `front()` / `back()` | 访问首/尾元素          |

示例：

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    vec.push_back(6);      // 添加元素 6
    vec.insert(vec.begin(), 0);  // 在头部插入 0

    vec.pop_back();        // 删除末尾元素 6
    vec.erase(vec.begin()); // 删除第一个元素 0

    for (int num : vec) {
        cout << num << " ";  // 输出 1 2 3 4
    }

    return 0;
}
```

------

##### **（3）遍历 `vector`**

**方法 1：普通 `for` 循环**

```cpp
for (size_t i = 0; i < vec.size(); ++i) {
    cout << vec[i] << " ";
}
```

**方法 2：范围 `for` 循环**

```cpp
for (int num : vec) {
    cout << num << " ";
}
```

**方法 3：迭代器**

```cpp
for (vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
    cout << *it << " ";
}
```

**方法 4：C++11 `auto` 关键字**

```cpp
for (auto it = vec.begin(); it != vec.end(); ++it) {
    cout << *it << " ";
}
```

------

##### **（4）容量管理**

```cpp
vector<int> vec;
vec.reserve(100);   // 预分配 100 个空间，避免频繁扩容
vec.shrink_to_fit(); // 释放多余的内存
```

------

#### **3. `vector` 的扩容机制**

- `vector` 在空间不足时会 **自动扩展**，通常是 **原容量的 2 倍**。
- `capacity()` 返回当前分配的总容量，而 `size()` 仅返回已存储的元素个数。

示例：

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec;
    for (int i = 0; i < 10; ++i) {
        vec.push_back(i);
        cout << "Size: " << vec.size() << ", Capacity: " << vec.capacity() << endl;
    }
    return 0;
}
```

**输出示例（不同编译器可能略有不同）：**

```
Size: 1, Capacity: 1
Size: 2, Capacity: 2
Size: 3, Capacity: 4
Size: 4, Capacity: 4
Size: 5, Capacity: 8
Size: 6, Capacity: 8
Size: 7, Capacity: 8
Size: 8, Capacity: 8
Size: 9, Capacity: 16
Size: 10, Capacity: 16
```

------

#### **4. `vector` 与 `array`、`list` 的比较**

| 特性              | `vector`           | `array`            | `list`         |
| ----------------- | ------------------ | ------------------ | -------------- |
| 存储方式          | **动态数组**       | **静态数组**       | **双向链表**   |
| 访问速度          | **O(1) 随机访问**  | O(1)               | O(n)           |
| 插入/删除（末尾） | **O(1)**           | 不支持             | O(1)           |
| 插入/删除（中间） | **O(n)**           | O(n)               | **O(1)**       |
| 迭代器            | **随机访问迭代器** | **随机访问迭代器** | **双向迭代器** |
| 适用场景          | 频繁访问、少量插入 | 固定大小的数组     | 频繁插入删除   |

------

#### **5. `vector` 的常见问题**



resize()：改变当前容器内含有元素的数量(size())，⽽不是容器的容量

1. 当resize(len)中len>v.capacity()，则数组中的size和capacity均设置为len;
2. 当resize(len)中len<=v.capacity()，则数组中的size设置为len，⽽capacity不变;   

reserve()：改变当前容器的最⼤容量（capacity）

1. 如果reserve(len)的值 > 当前的capacity()，那么会重新分配⼀块能存len个对象的空间，然后把之前的对象通 过copy construtor复制过来，销毁之前的内存; 
2. 当reserve(len)中len<=当前的capacity()，则数组中的capacity不变，size不变，即不对容器做任何改变。 



##### **（1）如何防止扩容影响性能？**

可以使用 `reserve(n)` 预分配空间，避免不必要的扩容：

```cpp
vector<int> vec;
vec.reserve(1000); // 预分配 1000 个元素的空间
```

##### **（2）如何释放 `vector` 多余的内存？**

使用 `shrink_to_fit()` 释放未使用的容量：

```cpp
vector<int> vec(1000, 1);
vec.clear();
vec.shrink_to_fit();  // 释放多余的内存
```

------

#### **总结**

1. `vector` 是 **动态数组**，支持 **快速随机访问（O(1)）**，但在 **插入和删除** 时可能需要 **移动元素**（O(n)）。
2. 末尾插入/删除效率高，适合 **存储大量连续数据**。
3. `vector` 会 **自动扩容**，但可以使用 `reserve()` 预分配空间以优化性能。
4. **适用于频繁随机访问、不常插入/删除中间元素的场景**。

如果需要 **高效插入/删除**，建议使用 `list`；如果数据大小固定，建议使用 `array`。





### list

`list` 是 C++ STL（标准模板库）中的 **双向链表（Doubly Linked List）** 容器，它支持 **快速的插入和删除** 操作，但不支持 **随机访问**。

------

#### **1. `list` 的特点**

- **双向链表结构**：每个元素存储一个指向前后元素的指针（`prev` 和 `next`）。
- **插入和删除操作高效（O(1)）**：
  - **在中间插入/删除** 元素不需要移动其他元素，只需要调整指针。
  - **在头部/尾部插入/删除** 的效率远高于 `vector`。
- **不支持随机访问（O(n））**：
  - 不能使用 `[]` 或 `at(i)` 访问特定索引元素，只能使用 **迭代器遍历**。
- **比 `vector` 占用更多内存**：
  - 由于存储了额外的指针（`prev` 和 `next`），`list` 的内存开销比 `vector` 大。

------

#### **2. `list` 的常见操作**

##### **（1）定义和初始化**

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> lst1;               // 空链表
    list<int> lst2(5);            // 5 个元素，默认初始化为 0
    list<int> lst3(5, 10);        // 5 个元素，每个值为 10
    list<int> lst4 = {1, 2, 3};   // 列表初始化
    list<int> lst5(lst4);         // 拷贝构造

    return 0;
}
```

------

##### **（2）基本操作**

| 操作     | 方法                         | 说明                              |
| -------- | ---------------------------- | --------------------------------- |
| **增**   | `push_back(x)`               | 末尾添加元素                      |
| **增**   | `push_front(x)`              | 头部添加元素                      |
| **插**   | `insert(it, x)`              | 在指定位置插入                    |
| **删**   | `pop_back()`                 | 删除末尾元素                      |
| **删**   | `pop_front()`                | 删除头部元素                      |
| **删**   | `erase(it)`                  | 删除指定位置元素                  |
| **改**   | `it = lst.begin(); *it = x;` | 通过迭代器修改元素                |
| **查**   | `front()` / `back()`         | 获取头/尾元素                     |
| **大小** | `size()`                     | 获取元素个数                      |
| **清空** | `clear()`                    | 清空所有元素                      |
| **排序** | `sort()`                     | 升序排序（默认 `operator<` 比较） |
| **反转** | `reverse()`                  | 反转链表                          |
| **合并** | `merge(lst2)`                | 合并两个有序链表                  |
| **去重** | `unique()`                   | 删除相邻重复元素                  |

示例：

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> lst = {1, 2, 3, 4, 5};

    lst.push_front(0);  // 头部插入 0
    lst.push_back(6);   // 尾部插入 6

    lst.pop_front();    // 删除头部元素 0
    lst.pop_back();     // 删除尾部元素 6

    for (int num : lst) {
        cout << num << " ";  // 输出 1 2 3 4 5
    }

    return 0;
}
```

------

##### **（3）遍历 `list`**

**方法 1：范围 `for` 循环**

```cpp
for (int num : lst) {
    cout << num << " ";
}
```

**方法 2：迭代器**

```cpp
for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
    cout << *it << " ";
}
```

**方法 3：C++11 `auto` 关键字**

```cpp
for (auto it = lst.begin(); it != lst.end(); ++it) {
    cout << *it << " ";
}
```

------

##### **（4）插入和删除**

`list` 允许在任意位置高效插入/删除：

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> lst = {1, 2, 3, 4, 5};

    auto it = lst.begin();
    advance(it, 2); // 移动到第 3 个元素（从 0 开始计数）

    lst.insert(it, 99);  // 在第 3 个元素前插入 99
    lst.erase(it);       // 删除第 3 个元素（原来的 3）

    for (int num : lst) {
        cout << num << " ";
    }

    return 0;
}
```

------

##### **（5）排序和去重**

#### **排序**

`list` 自带 `sort()` 方法，可以对链表进行排序：

```cpp
list<int> lst = {4, 2, 3, 1, 5};
lst.sort();  // 升序排序
```

也可以使用自定义排序规则：

```cpp
lst.sort([](int a, int b) { return a > b; });  // 降序排序
```

#### **去重**

`unique()` 只能删除 **相邻的重复元素**：

```cpp
list<int> lst = {1, 2, 2, 3, 3, 4, 5};
lst.unique();  // 变为 {1, 2, 3, 4, 5}
```

如果 `list` 不是排序好的，去重前需要先 `sort()`：

```cpp
lst.sort();
lst.unique();
```

------

#### **3. `list` 与 `vector` 的比较**

| 特性          | `list`                   | `vector`                 |
| ------------- | ------------------------ | ------------------------ |
| 底层结构      | **双向链表**             | **动态数组**             |
| 随机访问      | **O(n)（不支持 `[]`）**  | **O(1)（支持 `[]`）**    |
| 头部插入/删除 | **O(1)**                 | **O(n)**                 |
| 中间插入/删除 | **O(1)**（需要迭代器）   | **O(n)**（需要移动元素） |
| 末尾插入/删除 | **O(1)**                 | **O(1)**（如果无扩容）   |
| 内存占用      | **较大**（存储额外指针） | **较小**（连续存储）     |
| 适用场景      | **频繁插入/删除**        | **频繁访问、批量处理**   |

------

#### **4. `list` 适用场景**

- 需要 **频繁插入或删除**（特别是 **中间** 位置）。
- **不需要随机访问**，主要进行 **顺序遍历**。
- 需要支持 **合并、排序、去重** 操作。
- 适合 **大型数据结构**，如 **链表、队列、栈**。

**如果需要频繁访问元素，建议使用 `vector`；如果插入/删除较多，建议使用 `list`。**

------

#### **总结**

1. **`list` 是双向链表**，支持 **O(1) 头部/尾部插入删除**，但不支持 **O(1) 随机访问**。
2. **适合频繁的插入/删除操作**，特别是 **中间位置** 的修改。
3. **比 `vector` 占用更多内存**，但可通过 `sort()`、`merge()`、`unique()` 进行高效操作。
4. **适用于队列、链表、排序、去重等操作**，但如果需要 **随机访问**，应该使用 `vector`。





## cpp 泛型编程



#### 模板全特化和偏特化

⼀个模板被称为全特化的条件：1.必须有⼀个主模板类 2.模板类型被全部明确化。  

**类模板**

```cpp
template<typename T1, typename T2>
class Test
{
    public:
    Test(T1 i,T2 j):a(i),b(j){cout<<"模板类"<<endl;}
    private:
    T1 a;
    T2 b;
};

template<>
class Test<int , char>
{
    public:
    Test(int i, char j):a(i),b(j){cout<<"全特化"<<endl;}
    private:
    int a;
    char b;
};

template <typename T2>
class Test<char, T2>
{
    public:
    Test(char i, T2 j):a(i),b(j){cout<<"偏特化"<<endl;}
    private:
    char a;
    T2 b;
}
```

**函数模板**

函数模板，只有全特化，偏特化的功能可以通过函数的重载完成。  

```cpp
#include <iostream>
using namespace std;

// 通用模板
template <typename T>
void print(T value) {
    cout << "通用模板: " << value << endl;
}

// 全特化：针对 int 类型
template <>
void print<int>(int value) {
    cout << "全特化: 处理 int -> " << value << endl;
}

// 偏特化（通过重载实现）：针对指针类型
template <typename T>
void print(T* value) {
    cout << "偏特化: 处理指针类型 -> " << *value << endl;
}

int main() {
    print(3.14);    // 通用模板: 3.14
    print(42);      // 全特化: 处理 int -> 42
    string str = "hello";
    print(&str);    // 偏特化: 处理指针类型 -> hello
    return 0;
}
```

**特殊案例：指针偏特化**

```cpp
#include <iostream>
using namespace std;

// 通用模板
template <typename T>
class MyClass {
public:
    void show() { cout << "通用模板: " << typeid(T).name() << endl; }
};

// 偏特化：T 是指针类型时
template <typename T>
class MyClass<T*> {
public:
    void show() { cout << "偏特化: T 是指针类型 -> " << typeid(T).name() << "*" << endl; }
};

int main() {
    MyClass<int> obj1;
    obj1.show(); // 输出：通用模板: int

    MyClass<int*> obj2;
    obj2.show(); // 输出：偏特化: T 是指针类型 -> int*

    return 0;
}
```





## cpp11的新特性



### **统一的初始化方法**

在 C++11 之前，初始化不同类型的变量和容器有不同的方式，容易混淆。而 C++11 引入了 **统一的初始化方法**（列表初始化 `{}`），使得初始化更加简洁、安全。

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int a = 10;     // 传统初始化
    int b(20);      // 传统初始化
    int c{30};      // C++11 统一初始化
    int d = {40};   // C++11 统一初始化

    vector<int> vec1 = {1, 2, 3, 4, 5}; // 统一初始化
    vector<int> vec2{6, 7, 8, 9, 10};   // 统一初始化

    cout << "a=" << a << ", b=" << b << ", c=" << c << ", d=" << d << endl;
    return 0;
}
```

**优点**

- **防止窄化转换**（narrowing conversion），比如 `double` 赋值给 `int` 时会出错。
- **支持初始化 STL 容器**，更简洁直观。

------

### **成员变量默认初始化**

C++11 允许类的**成员变量在定义时直接初始化**，不需要在构造函数中初始化，简化了代码。

```cpp
#include <iostream>
using namespace std;

class Test {
private:
    int x{10};  // 直接初始化
    double y = 3.14; // 也可以使用 = 进行初始化
public:
    void show() {
        cout << "x = " << x << ", y = " << y << endl;
    }
};

int main() {
    Test t;
    t.show(); // 输出：x = 10, y = 3.14
    return 0;
}
```

**优点**

- **代码更清晰**，减少构造函数的初始化代码。
- **避免未初始化变量的错误**。

------

### **`auto` 关键字**

`auto` 允许编译器自动推导变量的类型，减少手动声明的冗余，使代码更简洁。

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    auto x = 42;     // x 被推导为 int
    auto y = 3.14;   // y 被推导为 double
    auto z = "Hello"; // z 被推导为 const char*

    vector<int> vec = {1, 2, 3, 4};
    for (auto v : vec) {
        cout << v << " "; // 自动推导 v 的类型
    }
    return 0;
}
```

**优点**

- **减少冗长的类型声明**，尤其是模板类型。
- **提高代码可读性**。

------

### **`decltype` 关键字**

`decltype` 用于获取表达式的**类型**，类似于 `typeof`。

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    decltype(a) b = 20; // b 的类型和 a 一样，都是 int

    cout << "b = " << b << endl; // 输出：b = 20
    return 0;
}
```

**用途**

- **用于模板元编程**，可获取变量、表达式或函数返回值的类型。

------

### **智能指针**

C++11 引入了 **智能指针**，用于自动管理动态分配的对象，避免内存泄漏。

```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    shared_ptr<int> p1 = make_shared<int>(10); // 创建 shared_ptr
    unique_ptr<int> p2 = make_unique<int>(20); // 创建 unique_ptr

    cout << "p1 = " << *p1 << ", p2 = " << *p2 << endl;
    return 0;
}
```

**优点**

- **自动管理资源**，避免手动 `delete`。
- **`shared_ptr`** 允许多个指针共享同一资源。
- **`unique_ptr`** 只能有一个所有者，防止误用。

------

### **`nullptr` 关键字**

C++11 引入 `nullptr` 作为 **空指针的标准表示**，替代 `NULL`，提高类型安全性。

```cpp
#include <iostream>
using namespace std;

void func(int* ptr) {
    if (ptr) cout << "指针有效" << endl;
    else cout << "空指针" << endl;
}

int main() {
    func(nullptr); // 传递 nullptr
    return 0;
}
```

------

### **基于范围的 `for` 循环**

简化遍历 **数组** 和 **STL 容器**。

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};
    for (int v : vec) { // 遍历 vec
        cout << v << " ";
    }
    return 0;
}
```

------

### **右值引用 (`&&`) 和 `move` 语义**

#### 左值和右值

```cpp
//左值（Lvalue）：可以取地址，有名称的变量，例如：
int a = 10;  // a 是左值
//右值（Rvalue）：临时值，没有名称，不能取地址，例如：
int b = 2 + 3;  // 2+3 是右值
int c = a * 5;  // a*5 是右值

int x = 10;  // x 是左值
int y = x + 20; // x+20 是右值
```

C++11 允许使用 `&&` 来**绑定右值**：

```cpp
int &&r = 10;  // 10 是右值，r 绑定它
```

但不能绑定**左值**：

```cpp
int a = 10;
int &&r = a;  // ❌ 错误，不能绑定左值
```

**解决方案**：使用 `std::move()` 将左值转换为右值：

```cpp
int &&r = std::move(a); // ✅ 这样才行
```

用于函数重载：

```cpp
void process(int& x) { cout << "左值引用" << endl; }
void process(int&& x) { cout << "右值引用" << endl; }

int main() {
    int a = 10;
    process(a);  // 输出：左值引用
    process(20); // 输出：右值引用
}
```

#### 移动构造

```cpp
#include <iostream>
#include <vector>
using namespace std;

class Test {
public:
    vector<int> data;
    Test() { cout << "默认构造" << endl; }
    Test(const Test& t) { cout << "拷贝构造" << endl; } // 传统拷贝
    Test(Test&& t) noexcept { // 移动构造
        cout << "移动构造" << endl;
        data = move(t.data);
    }
};

int main() {
    Test t1;
    Test t2 = t1;       // 调用拷贝构造
    Test t3 = move(t1); // 调用移动构造
    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

class Test {
public:
    vector<int> data;
    
    Test() { cout << "默认构造" << endl; }

    // 拷贝构造（深拷贝，创建新副本）
    Test(const Test& t) {
        cout << "拷贝构造" << endl;
        data = t.data;
    }

    // 移动构造（偷走资源）
    Test(Test&& t) noexcept {
        cout << "移动构造" << endl;
        data = move(t.data); // “偷走”资源
    }
};

int main() {
    Test t1;
    Test t2 = t1;       // 调用拷贝构造
    Test t3 = move(t1); // 调用移动构造（t1 资源被偷走）
    return 0;
}
```

- **拷贝构造**：需要 **分配新内存 + 复制数据**，速度慢。
- **移动构造**：**只是转移指针**，**不复制数据**，速度快。

`std::move()` 不会删除对象，而是转移对象的资源，使原对象的资源变为空，提高程序运行效率！

#### noexcept关键字

C++ 标准库的某些容器（如 `std::vector`）在**扩容或重新分配内存时**，如果元素类型的**移动构造函数**是 `noexcept`，则会使用**移动操作**而不是**拷贝操作**。

如果**没有 `noexcept`**，标准库会选择**拷贝构造**，即使有移动构造！

因为如果移动构造可能抛异常，万一移动失败，程序状态可能会变得不可预测，所以 `std::vector` 等容器会回退到**更安全的拷贝构造**（但性能更差）。

#### 完美转发

```cpp
#include <iostream>
using namespace std;

void func(int& x) { cout << "左值引用" << endl; }  // 处理左值的版本
void func(int&& x) { cout << "右值引用" << endl; } // 处理右值的版本

void wrapper(int x) { // ❌ 传值调用，无法区分左值右值
    func(x);  // 这里的 `x` 是左值
}

int main() {
    int a = 10;
    wrapper(a);    // ❌ 期望调用 `func(int&)`，但 `x` 变成左值
    wrapper(20);   // ❌ 右值 `20` 传进来变左值
}
```

- `wrapper` 传值调用时，参数 `x` 是左值，导致 `func(x)` 无法区分原始的左右值属性！
- 无论 `wrapper(a)` 还是 `wrapper(20)`，`x` 在 `wrapper` 内部都是左值，导致错误调用 `func(x)`。

要解决这个问题，我们需要：

- 保持 `wrapper` 传递参数的原始左右值属性
- 确保 `wrapper(a)` 传左值，`wrapper(20)` 传右值

✅ 使用 `std::forward<T>(arg)` 进行完美转发

```cpp
#include <iostream>
using namespace std;

void func(int& x) { cout << "左值引用" << endl; }
void func(int&& x) { cout << "右值引用" << endl; }

template<typename T>
void wrapper(T&& arg) { // T&& 是万能引用
    func(std::forward<T>(arg)); // std::forward 保持左右值属性
}

int main() {
    int a = 10;
    wrapper(a);    // ✅ `a` 是左值，调用 `func(int&)`
    wrapper(20);   // ✅ `20` 是右值，调用 `func(int&&)`
}
```

使用例：构造函数转发

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    template<typename T1, typename T2>
    Person(T1&& name, T2&& age) : name_(forward<T1>(name)), age_(forward<T2>(age)) {}

private:
    string name_;
    int age_;
};

int main() {
    string myName = "Alice";
    Person p1(myName, 25);   // ✅ `myName` 是左值，std::forward 保持左值
    Person p2("Bob", 30);    // ✅ `"Bob"` 是右值，std::forward 保持右值
}
```

#### move与forward

| 功能             | `std::move(arg)`                     | `std::forward<T>(arg)`      |
| ---------------- | ------------------------------------ | --------------------------- |
| 用途             | 强制转换为右值                       | 仅在 `T` 为右值时转换为右值 |
| 是否保留参数类型 | 否，直接变成右值                     | 是，保留左值或右值属性      |
| 适用场景         | 显式移动对象（如 `std::unique_ptr`） | 完美转发                    |

```cpp
void test(int&& x) {}

int a = 10;
test(std::move(a));  // ✅ std::move 强制 `a` 变成右值

template<typename T>
void wrapper(T&& arg) {
    test(std::forward<T>(arg));  // ✅ `std::forward` 只有在原本是右值时才会转成右值
}
```

`std::move` 强制转换为右值，`std::forward` 只在必要时转换为右值！



#### 万能引用

什么时候 `T&&` 是右值引用，什么时候是万能引用？

- `T&&` 是右值引用：当 `T` 是非模板参数或显式指定的类型。
- `T&&` 是万能引用：当 `T` 通过模板参数推导出来，并且 `T&&` 直接出现在 模板参数中。

```cpp
template <typename T>
void wrapper(T&& x) {}

int a = 10;
wrapper(a);  // T 被推导为 int&，所以 wrapper<int&>(int& x)
wrapper(20); // T 被推导为 int，  所以 wrapper<int>(int&& x)
```

右值引用：

```cpp
void func(int&& x) {
    cout << "右值引用" << endl;
}

int main() {
    int a = 10;
    func(a);  // ❌ 错误，a 是左值，不能绑定到 int&&
    func(20); // ✅ 正确，20 是右值
}
```

万能引用：

```cpp
template <typename T>
void wrapper(T&& x) {
    cout << "T 是 " << typeid(T).name() << endl;
}

int main() {
    int a = 10;
    wrapper(a);   // ✅ T 被推导为 int&，所以 T&& 变成 int&
    wrapper(20);  // ✅ T 被推导为 int， 所以 T&& 变成 int&&
}
```



### **无序容器 (`unordered_map` 等)**

C++11 引入了 **哈希表容器**，相比 `map`，效率更高。

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    unordered_map<string, int> myMap = {{"Alice", 25}, {"Bob", 30}};
    cout << "Alice: " << myMap["Alice"] << endl;
    return 0;
}
```

------

### **正则表达式 (`regex`)**

支持 **正则匹配** 和 **字符串处理**。

```cpp
#include <iostream>
#include <regex>
using namespace std;

int main() {
    string str = "abc123";
    regex pattern("[a-z]+[0-9]+");
    cout << (regex_match(str, pattern) ? "匹配成功" : "匹配失败") << endl;
    return 0;
}
```

------

### **Lambda 表达式**

支持 **匿名函数**，简化代码。

```cpp
#include <iostream>
using namespace std;

int main() {
    auto add = [](int a, int b) { return a + b; };
    cout << "3 + 5 = " << add(3, 5) << endl;
    return 0;
}
```

















## 其他



### std::allocator

`allocator`是标准库提供的一个通用工具，用于管理内存分配和释放。它是STL（标准模板库）容器如`vector`、`list`等背后进行内存管理的机制。通过自定义分配器，开发者可以控制容器如何获取和释放内存，这对于优化性能或使用特定类型的内存资源非常有用。

```cpp
#include <iostream>
#include <memory> // std::allocator位于该头文件中

int main() {
    std::allocator<int> alloc; // 创建一个用于分配int类型的allocator对象
    int* p = alloc.allocate(5); // 分配能存储5个int的空间

    // 构造这些int对象
    for (size_t i = 0; i < 5; ++i) {
        alloc.construct(&p[i], i); // 初始化每个元素
    }

    // 使用这些int对象...
    for (size_t i = 0; i < 5; ++i) {
        std::cout << p[i] << " ";
    }
    std::cout << std::endl;

    // 销毁这些int对象
    for (size_t i = 0; i < 5; ++i) {
        alloc.destroy(&p[i]); // 销毁每个元素
    }

    alloc.deallocate(p, 5); // 释放分配的内存

    return 0;
}
```

### new

- **`new`**：分配内存并构造对象，是最常用的动态内存分配方式。
- **`operator new`**：只负责分配内存，不进行构造。可以被重载以提供定制化的内存分配策略。
- **`placement new`**：在已分配的内存中构造对象，适用于需要精细控制内存分配的情况，如内存池实现。

```cpp
#include <new> // 必须包含这个头文件来使用placement new

char buffer[sizeof(MyClass)]; // 预先分配的内存
MyClass* obj = new(buffer) MyClass(); // 在buffer中构造对象
```

理解和正确使用这三个概念可以帮助更高效地管理内存，避免常见的内存泄漏和其他与内存管理相关的问题。



### 自旋锁

一种轻量级的锁机制，用于实现线程同步。当一个线程尝试获取锁时，如果锁已经被其他线程占用，它不会进入阻塞状态，而是不断尝试获取锁，直到成功，这种“忙等”行为就叫作“自旋”。

在`fetchRange`函数代码中的部分：

```cpp
while (locks_[index].test_and_set(std::memory_order_acquire));
```

- `test_and_set()`是原子操作，它将locks_[index]设置为true，并返回锁的原始状态。
- 如果返回值表示锁已经被占用（true），线程会继续尝试获取锁（自旋）。
- 这种方式避免了线程切换的开销。

| 自旋锁                         | 互斥锁                   |
| ------------------------------ | ------------------------ |
| 忙等，反复尝试获取锁           | 阻塞，线程会被挂起       |
| 适合锁持有时间很短的场景       | 适合锁持有时间较长的场景 |
| 无需操作系统上下文切换，性能高 | 需要上下文切换，开销较大 |



## MemoryPool-v1 项目框架