# struct and class

- Defination and instantiation

```swift
struct Student {
  var id = 0
  var name = ""
  var gender: String? // default nil
} // value type

class Union {
  var chairMan = Student()
  var id = 0
  var name: String?
} // reference type

let lyz = Student(id: 1, name: "li", gender: "man") // member by member constructors of struct type
lyz.id = 2 // error, lyz is a constant
let studentUnion = Union() // right, studentUnion is a 'let' constant pointer
```

## property

- **stored** property of struct

```swift
struct FixedLengthRange { // value type
    var firstValue: Int // var memory attribute
    let length: Int // let memory attribute
}

let first = FixedLengthRange(firstValue: 1, length: 3)
var second = FixedLengthRange(firstValue: 2, length: 5)
first.firstValue = 2 // error, first is a constant
second.length = 4 // error, length is a constant
```

- lazy load **stored** attribute

```swift
class Extern {
  var number = 0
  init(number: Int){
    self.number = number
  }
}
class LazyMan {
	var number = 0
  // var extern = Extern(number: number) error: initializer will run but self.number is not available
  lazy var extern = Extern(number: number)
}
let lazyMan = LazyMan() // extern not initialized
print(lazyMan.extern) // extern initialied
```

- **computed** attribute

```swift
struct Point {
	var x = 0
  var y = 0
}
struct Size {
  var width = 0APP
  var Height = 0
}
struct Rect {
  var origin = Point()
  var size = Size()
  var center: Point { // cannot be anything but var
    get {
      let centerX = origin.x + size.width / 2
      let centerY = origin.y + size.height / 2
      return Point(x: centerX, y: centerY) // caculate center point using origin and size
    }
    set(newCenter) { // set parameter name "newCenter" of the new value, type is Point
      origin.x = newCenter.x - size.width / 2
      origin.y = newCenter.y - size.height / 2 // set a new center point, thus origin changed
    }
  }
}

var square = Rect(origin: Point(x: 0, y: 0), size: Rect(width: 10, height: 10)) // get
square.center = Point(x: 10, y: 10) // set
// abbreviation
var center: Point {
  get {
  	Point(x: origin.x + size.width / 2, y: origin.y + size.height / 2) 
  }
  set { // using default parameter name "newValue"
  	origin.x = newValue.x - size.width / 2
    origin.y = newValue.y - size.height / 2
  }
}
```

- **read-only computed** attributes (no setter)

```swift
struct Cube {
	var width, height, depth: Double
  var volume: Double {
      return width * height * depth // give volume a setter attribute is meaningless
  }
}
let cube = Cube(width: 2, height: 3, depth: 4)
```

- add property observer for **stored** attribute.  willSet and didSet

```swift
class StepCounter {
	var totalSteps: Int = 0 {
		willSet {
			print("set \(totalSteps) to \(newValue)")
		}
		didSet {
			if totalSteps > oldValue {
        print("\(totalSteps > oldValue) more steps")
      }
		}
	}
}
```

- type property

```swift
struct SomeStrut {
  static var storedProperty: Int = 0 // stored
  static var computedProperty: String { // computed
    return "computed" 
  }
}
class SomeClass {
  static var storedProperty: Int = 0
  static var computedProperty: String {
    return "computed"
  }
}
class SomeEnum {
  static var storedProperty: Int = 0
  static var computedProperty: String {
    return "computed"
  } 
}
```

- get and set type property

```swift
struct AudioChannel {
  static let maxHoldLevel = 10
  static var inputLevel = 0
  var currentLevel: Int = 0 {
    didSet {
        if currentLevel > AudioChannel.maxHoldLevel {
        currentLevel = AudioChannel.maxHoldLevel
      }
        if currentLevel > AudioChannel.inputLevel {
        AudioChannel.inputLevel = currentLevel
      }
    }
  }
}
var channel = AudioChannel()
channel.currentLevel = 12
print("current: \(channel.currentLevel), input: \(AudioChannel.inputLevel)") // 10, 10
```

## method

- instance method and type method

```swift
struct SomeStruct {
  var x: Int = 0
  static y: Int = 0
  mutating func instanceFunc(x: Int) {
    self.x = x
  }
  static func typeFunc(y: Int) {
    self.y = y
    self.x = y // error: instance member CANNOT be used on type SomeStruct
  }
}
```

- override (method, property and subscript)

```swift
class Father {
  func doSomething() {
    print("father's property")
  }
}
class Son: Father {
	override func doSomething() {
    super.doSomething()
    print("son's property")
  }
}
let son = Son()
son.doSomething() // "father's property" "son's property"
// if not call super doSomething, only print "son's property"
// that's is, make sure to call super methods which return nothing to avoid unexpected errors. especially in UIViewController.
```

