# Runtime

- #### Runtime是什么

runtime我们称之为**运行时**，是Objective-C 之所以称为**动态语言**的基础，也是oc中的核心功能之一。苹果官方是这样描述的：runtime将尽可能多的决策从**编译时和链接时**推迟到**运行时**。例如**OC中调用方法**，我们称之为**消息转发**，并在**运行时**再去**确认接收者**的**类型**和实现的**方法**。runtime是基于C实现的，所以使用runtime, 调用的基本都是C的函数。

- #### Runtime可以做什么
  
  - 消息发送
  - 方法交换
  - 获取类的属性列表和方法列表
  - 动态的为类添加属性和方法
  - KVC的实现
  - KVO的实现
  - ……
- #### 什么是消息发送机制

在OC中，**方法调用**称之为**消息发送**，实际上方法的调用会首先调用C语言的objc_msgSend(receiver, SEL)函数，**receiver是接受者，也就是调用方法的对象**，**SEL就是调用方法的字符串**。objc_msgSend(receiver, SEL)主要做了以下几件事情：

1. 通过receiver获取isa指针。如果是对象调用方法，isa指针指向的是对象。如果是类调用方法，isa指针指向的元类。
2. 通过isa指针获取Class。
3. 先从类的缓存中获取imp指针（imp指针指向调用方法的实现地址），如果缓存中没有imp指针，则从方法列表中获取，如果本类没有找到，继续向父类查找。获取imp指针后，将其存入缓存。
4. 如果没有找到imp指针，进入**消息转发**流程，如果执行消息转发后，方法任没有被执行，则会引发崩溃。
5. 消息转发机制主要是解决如何处理未知消息的问题。

- #### 什么是方法交换

方法交换指的是在运行时替换某个类的方法实现或者在执行方法前添加自定义内容，控制App的执行流程。由上可知，消息发送最终目的是找到imp指针，OC中每个方法都有imp指针，所以方法交换本质交换的是imp指针，通过系统提供method_exchangeimplementation函数实现imp指针的交换。

- #### KVC

Key Value Coding（键值编码）。KVC提供了一种间接访问类属性方法或成员变量的机制，可以通过建字符串来访问对应的属性方法或成员变量。KVC本质上是在运行时，动态向对象发送setValue :ForKey:方法，为对象的属性设置数值。

```swift
class KVC: NSObject {
    @objc var number: Int = 0 // @objc 支持OC调用
}
let kvc = KVC()
kvc.setValue(1, forKey: "number")
print(kvc.number) // 1
```

- #### KVO是怎样通过Runtime实现的

Key Value Observing（键值观察）是OC对观察者模式的实现。当被观察者的属性值发生改变时，注册的观察者便能获得通知。

```swift
class KVOModel: NSObject {
    @objc dynamic var name: String = "tom"
}
// 添加观察者
class Obsever: NSObject {
    override func observeValue(forKeyPath keyPath: String?,
                               of object: Any?,
                               change: [NSKeyValueChangeKey : Any]?,
                               context: UnsafeMutableRawPointer?) {
        print(keyPath as Any)
        if let oldValue = change?[NSKeyValueChangeKey.oldKey] {
            print(oldValue)
        }
        if let newValue = change?[NSKeyValueChangeKey.newKey] {
            print(newValue)
        }
    }
}
let model = KVOModel()
let observer = Obsever()
model.addObserver(observer, forKeyPath: "name", options: [.new, .old], context: nil)
// 修改model的值
model.name = "jack" // name tom jack
```
