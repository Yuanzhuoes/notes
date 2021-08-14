# IOS GCD (grand central dispatch)

- Synchronization
  - 进程或线程同步执行任务，任务未结束时需等待，结束后才能执行下一个任务

- Asynchronization
  - 进程或线程异步执行任务，任务未结束但可在等待的过程中执行下一个任务

- dispatchQueue 调度队列 即线程 可同步异步
- workItem 在queue里运行的代码块 可同步异步
- serial & concurrent 串行和并行  串行是执行完一个任务才继续执行下一个 并行是触发后立即执行，不管是否完成 

# 串行 并行 同步 异步

``` swift
// 串行执行后台和主线程
// 1.创建一个队列
// 2.同步执行这个队列, 队列的workItem(here is block)在后台线程运行
// 3.第二个for在主线程运行
// 4.主线程的执行在后台线程未完成时阻塞，完成时运行
func synTaks() {
  let queue = DispatchQueue(label: "myQueue") 
    queue.sync {
      for number in 0..<10 {
        print(number)
      }
    }
  	
  	for number in 10..<20 {
      print(number)
    }
}

// 并行执行后台线程和主线程
// 异步执行这个队列
// 未执行完时立即返回主线程，主线程不会被阻塞
queue.async {
  for number in 0..<10 {
      print(number)
  }
}

for number in 10..<20 {
      print(number)
}
// 两个线程单独看都是串行的，因为是顺序的输出数字
// 实现两个线程的并行必须是异步，因为同步会阻塞另一个线程

// 并行执行多线程
// 具体是怎样并行的是操作系统决定的，每次的输入输出可能都不一样
func simpleQueue() {
    // 后台异步线程队列
  	let myQueue = DispatchQueue(label: "myQueue", attributes: .concurrent)
    myQueue.async {
        for number in 0..<10 {
            print("first: \(number)")
        }
    }
    myQueue.async {
        for number in 0..<10 {
            print("second: \(number)")
        }
    }
    myQueue.async {
        for number in 0..<10 {
            print("third: \(number)")
        }
    }
}
```

# 主队列和全局队列

- 并不需要总是自己手动创建队列，特别是不需要改变队列优先级的时候。
- 操作系统会创建后台队列(全局队列)的集合

```swift
// 创建后台（全局队列）
let queue = DispatchQueue.global()
// 访问主队列
DispatchQueue.main.asyn {
  // do something
}
```



# 应用

- 主线程负责UI界面和用户响应，任何操作都应当尽量避免阻塞主线程（异步）。如果出现资源竞争，酌情考虑同步。
- 耗时操作或对CPU需求大的任务在后台线程或并发执行。
- 为什么跟新UI要在主线程？
  - 1. UIKit不是线程安全的类（线程安全的类指的是，当多个线程访问类方法或属性时，不管以什么样的方式调用，或者线程如何交替进行，类的行为都是设想中的正确行为。），UI操作涉及到访问view对象的属性，异步操作会出现读写的问题，同步不会，因为同步的读写是顺序执行的。
    2. 程序的起点UIAppliacation是在主线程初始化的，所有的用户事件是在主线程上进行传递，所以UI要在主线程才能响应用户事件。
    3. 虽然加锁可以解决问题，但繁琐复杂效率低，用户体验差
    4. 所以，“在一个串行队列处理这些事就好了”
- Alamofire的reques方法是异步的，response中的闭包是默认主线程执行的，调用这个闭包主线程会被阻塞

