# Closure

### expression
```swift
{  (parameters) -> returnType in
	body
} // anonynous function
```
### useage
```swift
arrary.sort({ (first: Int, second: Int) -> bool in
	return first > second
})
```
### simplification based on auto type inference
```swift
arrary.sort({ first, second in
	return first > second
})
```
### parameters abbreviation

```swift
arrary.sort({$0 > $1})
```

# Trailing closure

### When the closure is the last parameter in function

- put it after ()

```swift
arrary.sort() { first, second in
	first > second
}
```

### When the closure is the unique parameter

- omit ()

```swift
arrary.sort { first, second in
	first > second
}
```

### Why using trailing closure

- 

# value capture

```swift
typealias closure = (Int) -> Int
func getClosure() -> closure {
	var number = 0
	func plus(incremental: Int) -> Int {
		number += incremental
		return number
	}
	return plus
}
```

### In fact, addInt is the address of plus, the local number in stack is now repalced in heap by closure ''plus''. (Life cycle extended)

```swift
var addInt = getClosure() // return type is (Int) -> Int
print(addInt(1)); print(addInt(2)); print(addInt(3))// 1, 3, 6 local var "number" in getClosure is alive
```

- using closure expression for abbrevation

```swift
func getClosure() -> closure {
	var number = 0
	return { (incremental: Int) -> Int in
		number += incremental
		return number
  }
}
```

- more concise style

```swift
func getClosure() -> closure {
	var number = 0
	return { 
		number += $0
		return number
	}
}
```

### That's is, we can use closure to capture local value and extend its life cycle.

# Nonescaping & escaping closure

- They both are the **parameters** of the function
- Nonescaping closure is called **before** the function return sth, and **in** the function action scope 
- escaping closure is probably called **after** the function return sth, and **out of** the function action scope

### Escaping closure

- closure delayed excution

#### **when test() called, clouser finish after test() return** 

```swift
typealias closureType = () -> Void
var completionHander : closureType?

func doSomething(hander: @escaping closureType) {
	completionHander = hander // assign
	DispatchQueue.global().asyncAfter(deadline: .now() + 0.1) {
    hander() // call and excute closure after the delay
  }
  print("doSomething() over")
  return
} 

doSomething() { 
	print("escaping closure over")
} // call and excute doSomething()
completionHander?() // "doSomething() over", "escaping closure over"
```

- Use closure to pass local values from one to another

```swift
class FirstPage {
    var text: String
    typealias clouserType = (_ text: String) -> Void
    var delivery: clouserType?
    init(_ text: String = "") {
        self.text = text
    }
    func passTextToAnother() {
        self.delivery?(text)
    }
    func escapingTextAndPassToAnother(completion: @escaping clouserType) {
        completion(self.text) // capture
        delivery = completion // assign
    }
}

class SecondPage {
    var text: String = ""
}

let first = FirstPage("hello")
let second = SecondPage()
first.delivery = { text in
    second.text = text
} // asssign
first.passTextToAnother() // excute conventional closure
print(second.text) // "hello"
first.text += " world"
first.escapingTextAndPassToAnother { text in
    second.text = text
} // escaping closure
print(second.text) // "hello world
```

