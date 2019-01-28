# 目录
* MOC
* Qt的信号和槽机制
* Qt的信号的连接方式

# 内容
## MOC
MOC(Meta Object Compiler)，其实MOC工具就是一个C++预处理程序，能为高层次的事件处理自动生成所需要的附加代码。

## Qt的信号和槽机制
来源：http://blog.51cto.com/9291927/2070398
深入：https://blog.csdn.net/liuuze5/article/details/53523463

信号和槽机制实际时观察者模式的一种变形，信号就是一个能被观察的事件，一个槽就是一个观察者。
当我们执行connect的时候，槽会被转换成为一个Connection对象，插入到被观察者(Sender)的ConnectionList中，当我们触发信号或者emit的时候，被观察者(sender)会遍历自己的ConnectionList，对每一个Connection对象进行通知。

## emit,signal,slot
简单点说：Debug模式下，附带了文件位置代码行数等信息，Release模式下，就是在头上添加0(METHOD),1(SLOT),2(SIGNAL)三个字符串，这个3个字符串用于做类型区分。emit什么都不做。
TODO 补充SIGNAL和SLOT宏的代码

## 信号和槽机制、普通函数能不能重名？
名称可以重复，但参数必须不同，其实信号最终会转换成为有一个普通的函数：调用QMetaObject::active函数

## Qt的信号的连接方式
