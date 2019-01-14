# 目录
* 基础知识
    * 关键字
    * 原始类型字节大小
    * 浮点数精度计算
    * sizeof
    * 数组
* 面向对象
    * class vs struct
    * 构造和析构
    * 对象创建
    * 派生

# 基础知识
## 关键字

|Key Workds|Key Workds|Key Workds|Key Workds|
|:-|:-|:-|:-|
|alignas (since C++11)|co_await (coroutines TS)|new|thread_local (since C++11)|
|alignof (since C++11)|co_return (coroutines TS)|noexcept (since C++11)|throw|
|and|co_yield (coroutines TS)|not|true|
|and_eq|decltype (since C++11)|not_eq|try|
|asm|default(1)|nullptr (since C++11)|typedef|
|atomic_cancel (TM TS)|delete(1)|operator|typeid|
|atomic_commit (TM TS)|do|or|typename|
|atomic_noexcept (TM TS)|double|or_eq|union|
|auto(1)|dynamic_cast|private|unsigned|
|bitand|else|protected|using(1)|
|bitor|enum|public|virtual|
|bool|explicit|reflexpr (reflection TS)|void|
|break|export(1)|register(2)|volatile|
|case|extern(1)|reinterpret_cast|wchar_t|
|catch|FALSE|requires (since C++20)|while|
|char|float|return|xor|
|char8_t (since C++20)|for|short|xor_eq|
|char16_t (since C++11)|friend|signed|
|char32_t (since C++11)|goto|sizeof(1)|
|class(1)|if|static|
|compl|import (modules TS)|static_assert (since C++11)|
|concept (since C++20)|inline(1)|static_cast|
|const|int|struct(1)|
|consteval (since C++20)|long|switch|
|constexpr (since C++11)|module (modules TS)|synchronized (TM TS)|
|const_cast|mutable(1)|template|
|continue|namespace|this|


## 原始类型字节大小
|类型|字节数|类型|字节数|
|:-|:-:|:-|:-:|
|bool|1|
|char|1|wchar_t|2|
|float|4|double|8|
|short|2|int|4|
|long|4|long long|8|

**备注:** 不包含虚函数的空类是1个字节，包含需函数的空类是4个字节。

## 浮点数的精度计算
估算公式: `2^10 = 1024 ≈ 10^3` 每10个bit可以表示3位精度<br>
根据IEEE754标准：<br>
|类型|符号位(bits)|阶乘(bits)|小数部分(bits)|精度|
|:-|:-:|:-:|:-:|:-:|
|float|1|8|23|6 - 7位|
|double|1|11|52|15 - 16位|

## sizeof
### 特殊结构
计算时需要注意以下特殊结构：
1. 空的类或结构体，sizeof为**1**。
2. 带虚函数的空类，sizeof为**4**(32位是4, 64位是8)。
3. 多重派生需要注意，每从一个带虚函数的类派生，sizeof需要加**4**(32位是4, 64位是8)。
4. union是取最大成员的长度。

### 字节对齐
1. 结构体中每个成员的位置必须能被其大小整除，否则在成员之间补字节数。
2. 结构体总大小必须是最大成员大小的整数倍，否则在结构体最后补字节数。

## 数组
基础知识：
1. 没有多维数组，多维数组是用一维数组模拟的；
2. 数组名是一个常量（所以不能进行赋值），其代表数组首元素的首地址；
3. 数组与指针的关系是因为数组下标操作符[]，此如，int a[3][2]相当于*(*(a+3)+2)；
4. 指针可以进行加减算术运算，加减的基本单位是sizeof(指针所指向的数据类型)；
5. 对数组的数组名进行取地址(&)操作，其类型为整个数组类型；
6. 对数组的数组名进行sizeof运算符操作，其值为整个数组的大小（以字节为单位）；
7. 数组作为函数形参时会退化为指针。

**要点1**：
```
int a[5];
int * pa1 = a;      // a代表数组元素首元素的地址
int * pa2 = &a[0];  // pa1和pa2等同
int * pa3 = &a;     // 错误：对数组名取地址，表示时指向数组的指针
int (*pa4)[5] = &a; // 正确
a + 1 = &a[0] + 1 = a[1]
```

**要点2**：
```
sizeof(a)  // 表示整个数组的大小
sizeof(&a) // 表示一个指针的大小 
```

# 面向对象
面向对象编程基于三个基本概念：数据抽象、继承、动态绑定。

## class vs struct
1. 默认访问权限不同，class为私有，struct为共有；
2. 默认继承访问权限不同，class默认派生是私有的，struct默认是共有的。

## 构造和析构
### 构造顺序
基类(成员变量 > 构造函数) > 派生类(成员变量 > 构造函数)
1. 成员变量构造和初始化列表构造是一回事，构造的顺序取决于声明的顺序；
2. 先进行成员变量和初始化列表构造，后调用构造函数；
3. 初始化列表的初始化顺序与声明顺序相同；
4. 先构造基类，后构造派生类。
5. 静态成员变量的初始化取决于初始化的位置。

### 析构顺序
1. 先声明后析构；
2. 先类析构，后成员析构；
3. 先析构派生类，后析构基类。

### 虚构造函数
虚函数表指针是在构造函数中进行初始化的，所以如果先使用虚函数表指针进行动态绑定的话，此时指针是无效的，必然引起错误。

### 虚析构函数
静态类型和被删除对象的动态类型有可能不同，此时如果析构函数不是虚函数的话，将导致派生类的析构函数不会被调用，产生问题。

### 构造函数（析构函数）中调用虚函数
如果在构造函数（析构函数）中调用虚函数，则运行的是类自身定义的函数，即不发生动态绑定。（解析：如果在基类的构造函数中发生动态绑定，调用派生类的虚函数，此时，派生类并没有进行构造，引发问题。）

## 对象创建
**要求在堆中创建**
让构造函数成为私有函数。
```c++
// 要求在堆上建立对象
class A
{
private:
    A();
}
```

**禁止在堆中创建**
通常禁止的方法是禁止用户调用new，可以重载operator new函数来达到这个目的。
```c++
// 禁止在堆中建立对象
class B
{
private:
    void * operator new(size_t size);
}
```

## 派生
### 重载、重写、隐藏
#### 重载
1. 相同的范围（在同一个类中）；
2. 函数名称相同；
3. 参数不同。

#### 重写
1. 不同的范围（分别位于派生类于基类）；
2. 函数名字相同；
3. 参数相同；
4. 基类函数必须有virtual关键字。

#### 隐藏
“隐藏”是指派生类的函数屏蔽了与其同名的基类函数，规则如下：
1. 如果派生类的函数与基类的函数同名，但参数不同。此时，不论有无virtual关键字，基类的函数将被隐藏。
2. 如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有virtual关键字。此时，基类的函数被隐藏。