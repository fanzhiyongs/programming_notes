# 目录
* MOC
* Qt的信号和槽机制
* Qt的信号的连接方式

# 内容
## MOC
MOC(Meta Object Compiler)，其实MOC工具就是一个C++预处理程序，能为高层次的事件处理自动生成所需要的附加代码。

## Qt的信号和槽机制
来源：http://blog.51cto.com/9291927/2070398<br>
深入：https://blog.csdn.net/liuuze5/article/details/53523463

信号和槽机制实际时观察者模式的一种变形，信号就是一个能被观察的事件，一个槽就是一个观察者。
当我们执行connect的时候，槽会被转换成为一个Connection对象，插入到被观察者(Sender)的ConnectionList中，当我们触发信号或者emit的时候，被观察者(sender)会遍历自己的ConnectionList，对每一个Connection对象进行通知。

### 信号的发射
信号会被转换为一个函数，此函数会调用QMetaObject::activate，在activate中遍历ConnectionList，根据连接类型，决定是直接调用还是，还是发送QEvent::MetaCall事件

## emit,signal,slot
简单点说：Debug模式下，附带了文件位置代码行数等信息，Release模式下，就是在头上添加0(METHOD),1(SLOT),2(SIGNAL)三个字符串，这个3个字符串用于做类型区分。emit什么都不做。

``` C++
TODO
```

## 信号和槽机制、普通函数能不能重名？
名称可以重复，但参数必须不同。

## Qt的信号的连接方式
|类型|说明|
|:-|:-|
|Qt::AutoConnection|如果在同一个线程，等同于Qt::DirectConnection，否则等同于Qt::QueuedConnection|
|Qt::DirectConnection|直接回调，执行在信号线程|
|Qt::QueuedConnection|发送一个QEvent::MetaCall信号到接收队列里，执行在接收队列中|
|Qt::BlockingQueuedConnection|发送一个QEvent::MetaCall，并等待接收线程执行完|

## 进程间同步、通信、远程调用、远程对象
### 线程间同步
* QMutex
* QMutexLocker(自动锁)
* QReadWriteLock - 允许并行读，只一个写
* QSemaphore

#### QMutex的模式(RecursionMode)
|类型|说明|
|:-|:-|
|QMutex::NonRecursive|默认，不可嵌套锁| 
|QMutex::Recursive|可以嵌套锁|

### 进程间同步(IPS, Interprocess synchronization)
* QSystemSemaphore

### 进程间通信(IPC)
* QSharedMemory
* TCP/IP
* D-Bus(Unix)
* QProcess
* Session Management(Linux/X11)
* WM_COPYDATA(Windows)

#### QLocalServer与QLocalSocket
QLocalServer/QLocalSocket在Windows上，底层是使用windows的name pipe。在Unix上是domain socket，domain socket不需要经过网络协议，不需要打包拆包，计算校验和，维护序号和应答等，知识将应用层数据从一个进行拷贝到另一个进程。

#### 匿名管道和命名管道的区别
**匿名管道**:
* 由于不具名，所以必须有一定关系的进程间才能通信(父子进程和兄弟进程)。
* 单工（半双工），固定的读写端。

**命名管道**:
* 任意进程间通信。
* 全双工，可度可写。

### 远程调用(RPC Remote Procedure(程序) Call)
TODO

### Qt远程对象(RO Remote Objects)
TODO

## Qt内存管理
### Qt对象树
在Qt中，每个QObject内部都有一个list，用来保存所有的children，还有一个指针，保存自己的parent。当它自己析构时，它会将自己从parent列表中删除，并且析构掉所有的children。

### delete和deleteLater
delete - 直接删除对象
deleteLater - 如果消息循环没有启动前调用，则在消息循环启动后执行一次删除；如果在消息循环结束后调用，对象将不会被删除。<br>

**示例**: new一个窗口然后close的时候deleteLater，可以创建出一个自删除的窗口。

**备注**：多次调用deleteLater是安全的，当第一个延迟删除消息收到以后，任何此对象未执行的事件将从事件队列中移除了。

### Qt中已下情况new出的对象可以不用亲自去delete
* QObject及其派生类的对象，如果其parent非0，则其parent析构时会析构该对象；
* QWidget及其派生类的对象，可以设置Qt::WA_DeleteOnClose标志位（当close时会析构该对象）；
* QAbstractAnimation派生类的对象，可以设置QAbstractAnimation::DeleteWhenStopped
* QRunnable::setAutoDelete()；
* MediaSource::setAutoDelete()；

## Qt属性系统
属性系统，让类可以拥有动态的成员。

### Q_DECLARE_METATYPE与qRegisterMetaType的关系
来源：https://blog.csdn.net/andyjim/article/details/41248829

## Qt事件和事件过滤器

## Qt线程池

## MVC
