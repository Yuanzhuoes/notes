# Optional

- 变量的可选初始化为nil
- 常量的可选未被初始化
- 若属性都有默认值，结构体和类都有默认构造器，无参数
- 结构体有默认的逐一成员构造器，类没有

所以下面的例子，结构体可以用默认的逐一成员构造器，不会报错。而类需要显示的定义构造器，为没有默认值的常量可选类型赋值，否则编译时报错：Class 'SomeClass' has no initializers

```swift
var varOptional: Double? 
let letOptional: Double? 

struct SomeStruct {
    var varOptional: Double?
    let letOptional: Double?
} // 

class SomeClass {
    var varOptional: Double?
    let letOptional: Double?
} 
```

# Implicity unwrapped optional

-  两层含义：1. 可选。2. 可以隐式解析。理解为一种可以用特别的方式解包的可选类型。
- 如果不能确定IUO有值，用隐式解析会运行时报错
- 所以尽量用可选类型代替，或者在确认IUO有值的情况下使用

```swift
var iuo: String! = "abc"
var str: String = iuo // 隐式解析

var optional: String? = "abc"
var str: String = optional! // 必须显示解析或者可选绑定
```

# unwrap

- 强制解析
- 可选绑定
  - guard else
  - if

```swift
var optinal: String? = "abc"

// 强制解析，失败则运行时报错
var bind = optional!

// 可选绑定，失败进入else语句，成功绑定后bind类型为String，且bind作用域在括号外部
guard let bind = optional else {
  fatalError("optional is nil")
}

// 可选绑定，成功进入大括号，bind作用域在括号内部
if let bind = optional {
  print(type(of: bind)) // String
}
```

