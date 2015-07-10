---

layout: post
title:  Boost.Asio库
category: "游戏研发"
tag:  "Boost"

---



Boost.Asio封装了一个io异步调用接口，可以用于实现网络通信操作。Asio提供了IO服务接口，I/O 服务抽象了操作系统的接口，允许第一时间进行异步数据处理，而 I/O 对象（通过IO服务创建，比如Socket）则用于初始化特定的操作。

###1 Asio库运行过程：
####同步操作
![enter image description here](http://www.boost.org/doc/libs/1_58_0/doc/html/boost_asio/sync_op.png)
程序首先需要一个`io_service对象`，`io_service`代表着程序到操作系统IO服务的连接。`boost::asio::io_service io_service;`

为了IO操作程序需要通过io_service创建一个IO对象，比如TCP socket：`boost::asio::ip::tcp::socket socket(io_service);`

当执行异步连接操作时，执行以下过程：
- 程序通过IO对象请求连接操作:`socket.connect(server_endpoint);`
- IO对象转发请求到io_service
- io_service通知操作系统执行网络连接操作。
- 操作系统返回连接请求的结果给io对象。


####异步操作

![enter image description here](http://www.boost.org/doc/libs/1_58_0/doc/html/boost_asio/async_op1.png)

1. 程序调用IO对象初始化连接操作：`socket.async_connect(server_endpoint, your_completion_handler);`其中，`your_completion_handler`参数是一个函数或函数对象。
2. IO对象发送请求到io_service.
3. `io_service`通知操作系统使其开始一个异步连接操作。
4. 程序做自己的事情（time pass）

![enter image description here](http://www.boost.org/doc/libs/1_58_0/doc/html/boost_asio/async_op2.png)
5. 操作系统通知连接操作完成，将结果放入队列等待`io_service`取出。
6. 程序会调用`io_service::run()`函数，这个函数会从结果队列中取出结果，然后解析给回调函数使用。



REF: http://www.boost.org/doc/libs/1_58_0/doc/html/boost_asio/overview/core/basics.html



###2 `run/run_one/poll/poll_one`的区别：
`poll/poll_one`无论事件是否准备好，都马上返回，不会进行阻塞。区别在于如果多个事件准备好，poll_one每次只执行其中一个，而poll执行所有。

`run/run_one`都是阻塞的，程序执行到这里就会等待。区别在于`run_one`等待一个事件准备好即可执行并返回，而`run`会等待所有的监控事件准备好，直至所有的事件准备好，然后返回。





### 参考：
http://zh.highscore.de/cpp/boost/
http://www.boost.org/doc/libs/1_58_0/doc/html/boost_asio.html

### 关于多线程：

使用thread创建新线程：`boost::thread t(boost::bind(&boost::asio::io_service::run, &ios));`

新建线程里执行的那个函数的名称被传递到 boost::thread 的构造函数。 一旦上述示例中的变量 t 被创建，该 thread() 函数就在其所在线程中被立即执行。 同时在 main() 里也并发地执行该 thread() 。

为了防止程序终止，就需要对新建线程调用 join() 方法。 join() 方法是一个阻塞调用：它可以暂停当前线程，直到调用 join() 的线程运行结束。 这就使得 main() 函数一直会等待到 thread() 运行结束。


 
