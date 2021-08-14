# ARC

```swift
class Person {
  var name: String
  init(name: String) {
    self.name = "name"
    print("\(name) is being initialized")
  }
  deinit {
    print("\(name) is being deinitialized")
  }
}

var one: Person?
var two: Person?
var three: Person? // default is nil, no instance and no reference

one = Person(name: "li")
two = one
three = one // instance of Person has three references now

one = nil
two = nil
three = nil // deinit called
```

# Cyclic strong reference

```swift
class Person {
    var name: String
    var aprtment: Apartment? // not all people have an apartment, thus optional
    init(name: String) {
        self.name = name
    }
    deinit {
        print("\(name) is being deinitialized")
    }
}
class Apartment {
    var address: String
    init(address: String) {
        self.address = address
    }
    weak var person: Person?
    deinit {
        print("\(address) is beding deinitialized")
    }
}
var person: Person?
var apartment: Apartment?
person = Person(name: "li")
apartment = Apartment(address: "chengdu")
person?.aprtment = apartment
apartment?.person = person // cyclic strong reference has been made
```

# weak reference

- must be optional var, because it might be set to nil at runtime

```swift
weak var person: Person?
```

# unownd reference

- always point to an instance
- cannot be set to nil when its reference deconstructed, because it is not an optional type

- A must have B, but not vice versa

```swift
class Custom {
  var name: String
  init(name: String) {
    self.name = name
  }
  var crediCard: CrediCard?
}
class CrediCard {
  var id: String
  unownd let customer: Custom
  init(id: String, customer: Custom) {
    self.id = id
    self.customer = customer
  }
}
```

# closure cyclic reference 

```swift
class TextElement {
    var text: String
  	// a function type var and its default value is a closure. the closure point to class instance
    lazy var display: () -> Void  = {
        print("default closure \(self.text)") 
    }
    init(text: String) {
        self.text = text
    }
    deinit {
        print("deint")
    }
}

var text: TextElement?
text = TextElement(text: "hello")
text?.display() // excute closure
text = nil // ARC of text is 1 now, will not be deinit
```

```swift
text?.display = { print(text?.text as Any) } // self-defined
text.display()
```

- think of closure as property will be more clear

# to solve cyclic reference

```swift
lazy var display = { [unowned self] in
	// body
  print("default closure \(self.text)")           
}
```



