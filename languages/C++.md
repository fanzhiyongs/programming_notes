# 目录
* 基础知识
* 面向对象
* C++ 11

# 基础知识
## 关键字

|Key Workds|Key Workds|Key Workds|
|:-|:-|:-|
|aalignas (since C++11)|delete(1)|reflexpr (reflection TS)|
|alignof (since C++11)|do|register(2)|
|and|double|reinterpret_cast|
|and_eq|dynamic_cast|requires (since C++20)|
|asm|else|return|
|atomic_cancel (TM TS)|enum|short|
|atomic_commit (TM TS)|explicit|signed|
|atomic_noexcept (TM TS)|export(1)|sizeof(1)|
|auto(1)|extern(1)|static|
|bitand|false|static_assert (since C++11)|
|bitor|float|static_cast|
|bool|for|struct(1)|
|break|friend|switch|
|case|goto|synchronized (TM TS)|
|catch|if|template|
|char|import (modules TS)|this|
|char8_t (since C++20)|inline(1)|thread_local (since C++11)|
|char16_t (since C++11)|int|throw|
|char32_t (since C++11)|long|true|
|class(1)|module (modules TS)|try|
|compl|mutable(1)|typedef|
|concept (since C++20)|namespace|typeid|
|const|new|typename|
|consteval (since C++20)|noexcept (since C++11)|union|
|constexpr (since C++11)|not|unsigned|
|const_cast|not_eq|using(1)|
|continue|nullptr (since C++11)|virtual|
|co_await (coroutines TS)|operator|void|
|co_return (coroutines TS)|or|volatile|
|co_yield (coroutines TS)|or_eq|wchar_t|
|decltype (since C++11)|private|while|
|default(1)|protected|xor|
|public|xor_eq|

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
根据IEEE754标准：
float中1位表示正负，8位表示阶乘，23位表示小数精度就是6 - 7位之间
double中1位表示正负，11位表示阶乘，52位表示小数精度在15 - 16位之间

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