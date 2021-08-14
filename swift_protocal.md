# protocal & delegate design patterns

```swift
import Foundation
protocol RentDelegate: AnyObject {
    func rentHouse(number: Int)
}
class Custom {
    weak var delegate: RentDelegate?
    func need(number: Int) {
        self.delegate?.rentHouse(number: number)
    }
}
class Server: RentDelegate {
    let custom = Custom()
    func help() {
        custom.delegate = self // self is RentDelegae type
    }
    func rentHouse(number: Int) {
        print("server help custom to rent \(number) houses.")
    }
}
let server = Server()
server.help() // 将custom的代理设置为server
server.custom.need(number: 4) // server遵循和实现了事先约定好的的协议，由server决定实现的时机。就如viewController一样，由它执行协议方法
```



```swift
protocol RentDelegate: AnyObject {
    func rentHouse(number: Int)
}
class Custom {
    weak var delegate: RentDelegate?
    func need(number: Int) {
        self.delegate?.rentHouse(number: number)
    }
}
class Server: RentDelegate {
    func rentHouse(number: Int) {
        print("server help custom to rent \(number) houses.")
    }
}
let custom = Custom()
let server = Server()
custom.delegate = server
custom.need(number: 3)
// "server help custom to rent 3 houses."
```

