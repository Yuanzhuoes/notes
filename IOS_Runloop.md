# Runloop

- #### 是什么

一个线程一次只能执行一个任务，执行完毕后就会退出。

而runloop（运行循环）是一种允许线程随时处理时间而不退出的机制。

runloop实际上是一个对象，管理它需要处理的事件和消息，并且提供一个入口函数来执行事件。

线程执行这个函数后，会进入接收消息->等待->处理消息->休眠等待下一个消息->接收消息的循环。
