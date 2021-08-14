# is and as

- is: examine type
- as: downcast class type

```swift
class Media {
  var name: String
  init(name: String) {
    self.name = name
  }
}
class Song: Media {
  var artist: String
  init(name: String, artist: String) {
    self.artist = artist
    super.init(name: name)
  }
}
class Movie: Media {
  var director: String
  init(name: String, director: String) {
    self.director = director
    super.init(name: name)
  }
}
var mediaItem = [Song(name: "love", artist: "adele"), Media(name: "super"), Movie(name: "happy", director: "someone")]
for item in mediaItem {
  if item is Song {
    let song = item as! Song
    print(song.name)
  } else {
    if let others = item as? Song {
      print(others.artist) // nil, down cast failure
    } else {
        print("down cast failure")
    }
  }
}
```

# Any and AnyObject

- Any can represent any type, including optional and function type
- AnyObject represents any class instance type