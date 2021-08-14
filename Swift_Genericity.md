# genericity

```swift
func swap<T>(_ first: inout T, _ second: inout T) {
  var temp: T
  temp = first
  first = second
  second = temp
}
```

# genericity struct

```swift
struct Stack<Element> {
  var items = [Element]()
  mutating func push(_ item: Element) {
    items.append(item)
  }
  mutating func pop() -> Element{
    items.removeLast()
  }
}
var stack = Stack<Int>()
```

# extension

```swi
extension Stack {
	var topItem: Element? {
		return items.isempty ? nil : items[items.count - 1]
	}
}
```

# constraints

```swift
func findIndex<T: Equatable> (of valueToFind: T, in array: [T]) -> Int? { // return type always Int
  for (index, value) in array.enumerated {
    if value == valueToFind { // not all type can be applied "==" operator, thus constraint T to follow the protocol Equatable
      return index
    }
    return nil
  }
}
```

