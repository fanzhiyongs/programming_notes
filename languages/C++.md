# 目录
* 基础知识
* 面向对象
* C++ 11

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


## 字节大小
|类型|字节数|类型|字节数|
|:-|:-:|:-|:-:|
|bool|1|
|char|1|wchar_t|2|
|float|4|double|8|
|short|2|int|4|
|long|4|long long|8|

**备注:** 不包含虚函数的空类是1个字节，包含需函数的空类是4个字节。

## 浮点数的精度计算
估算公式: 2^10 = 1024 ≈ 10^3 每10个bit可以表示3位精度
根据IEEE754标准：<br>
float中1位表示正负，8位表示阶乘，23位表示小数精度就是6 - 7位之间<br>
double中1位表示正负，11位表示阶乘，52位表示小数精度在15 - 16位之间<br>

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

### sizeof数组

## 数组


# 面向对象
# C++ 11