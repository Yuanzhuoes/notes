# enum

#### Define the same type for a set of related values

- they are all CompassPoint type with non-default values.

```swift
enum CompassPoint {
  case east
  case south
  case west
  case north
  // single line form:
  // case esat, south, west, north
}
```

- switch matching and auto type inference

```swift
var direction = CompassPoint.west
direction = .north

switch direction {
  case .north:
   // do something
  default:
   // do something
}
```

- following  CaseIterable protocol, to traverse the enumeration

```swift
enum CompassPoint: CaseIterable {
	case esat, south, west, north
}
let count = CompassPoint.allCases.count
for point in ComapssPoint.allCases {
  // do something
}
```

- associated values

```swift
enum Graphic {
  case rect (Int, Int, Int, Int)
  case circle (Int, Int, Int)
}

var graphic: Graphic = .rect(0, 0, 10, 10)

switch graphic {
case .rect(_, _, let width, let hight):
    print(width == hight ? "square" : "rectangle", terminator: " ")
    print("area is \(width * hight)")
case .circle(_, _, let radius):
    print("circle area is \(radius * radius * 3.14)")
}
```

- raw value

#### raw values of all cases must are the same type, and unique

```swift
enum Path: String {
  case login = "/api/login"
  case logout = "/api/logout"
  case reset = "/api/reset"
}
```

- raw value with implicit assignment

```swift
enum Number: Int {
	case one = 1, two, three, four, five // 1, 2, 3, 4, 5
}
enum Graphic: String {
  case square, rectangle, circle // "square" ...
}

let number = Number.one.rawValue // number == 1. specifically, if number = Number.one, number is Number type
let graphic: String = Graphic.circle.rawValue // "circle"
let something = Number(rawValue: 11) // use raw value to initialize enum instance, return optional Int
if let something = something {
  switch something {
  case .one:
  	print("find one")
  default:
  	print("not one")
  }
} else {
  print("not found") 
} // "not found"
```



