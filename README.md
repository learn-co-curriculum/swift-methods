# Swift — Methods

## Objectives

1. Distinguish methods from functions.
2. Write methods attributed to a custom base class.
3. Call a method on the current instance using the `self.` keyword.
4. Limit the visibility of a method with the `private` keyword.
5. Write a method that provides a return.
6. Write a method that accepts an argument.

## Introduction

In Object-Oriented Programming, methods are, in essence, functions that an object performs on itself. In Swift, this is painstakingly obvious since the only primary distinction in the syntax between writing a function and writing a method is that a method is placed within the scope of a class definition. That's it.

There are, however, additional keywords available to functions, such as `private` and `public` which alter visibility, `override` which is necessary when overriding inherited methods, `class` which defines the method as being performed by the class object, and a few other uncommon options.

## Maritime Methods

If we take a `Boat` class that's already set up with properties and initializers:

```swift
//  Boat.swift

import Foundation

class Boat {
    let name: String
    var sailors: [String]
    var maxSpeedKnots: Double
    var speedKnots: Double = 0
    
    init(name: String, sailors: [String], maxSpeedKnots: Double) {
        self.name = name
        self.sailors = sailors
        self.maxSpeedKnots = maxSpeedKnots
    }
    
    convenience init(name: String, maxSpeedKnots: Double) {
        self.init(name: name, sailors: [String](), maxSpeedKnots: maxSpeedKnots)
    }
}
```
We can add a few methods to the class like this:

```swift
//  Boat.swift

class Boat {
	...

    func fullSpeedAhead() {
        speedKnots = maxSpeedKnots
        print("Full speed ahead, aye!")
    }
    
    func fullStop() {
        speedKnots = 0
        print("Full stop, aye!")
    }
    
    func aheadOneHalfSpeed() {
        speedKnots = maxSpeedKnots / 2
        print("Ahead one half speed, aye!")
    }
}
```

They sure look a lot like functions.

Now from the AppDelegate we can create an instance of our class and call these methods on it:

```swift
let sailors = ["The Skipper", "Gilligan", "Mary-anne"]
let minnow = Boat(name: "SS Minnow", sailors: sailors, maxSpeedKnots: 25.0)

minnow.fullSpeedAhead()
print("speed (knots): \(minnow.speedKnots)")
        
minnow.fullStop()
print("speed (knots): \(minnow.speedKnots)")
        
minnow.aheadOneHalfSpeed()
print("speed (knots): \(minnow.speedKnots)")
```
This will print:

```
Full speed ahead, aye!
speed (knots): 25.0
Full stop, aye!
speed (knots): 0.0
Ahead one half speed, aye!
speed (knots): 12.5
```

### Using `self.` With Methods

Within a class definition, we can call a method with the `self.` keyword to instruct the current instance to perform this method in itself. Since we want to display the boat's speed after every method call to change it, we can create a method that prints the speed:

```swift
    func printSpeedKnots() {
        print("speed (knots): \(speedKnots)")
    }
```
We can then call it on `self.` from the other three methods:

```swift
    func fullSpeedAhead() {
        speedKnots = maxSpeedKnots
        print("Full speed ahead, aye!")
        self.printSpeedKnots()
    }
    
    func fullStop() {
        speedKnots = 0
        print("Full stop, aye!")
        self.printSpeedKnots()
    }
    
    func aheadOneHalfSpeed() {
        speedKnots = maxSpeedKnots / 2
        print("Ahead one half speed, aye!")
        self.printSpeedKnots()
    }
```
We can then safely remove our `print()` statements from within the AppDelegate since we've moved this functionality into the boat's behavior:

```swift
let sailors = ["The Skipper", "Gilligan", "Mary-anne"]
let minnow = Boat(name: "SS Minnow", sailors: sailors, maxSpeedKnots: 25.0)

minnow.fullSpeedAhead()
minnow.fullStop()
minnow.aheadOneHalfSpeed()
```
This will still print:

```
Full speed ahead, aye!
speed (knots): 25.0
Full stop, aye!
speed (knots): 0.0
Ahead one half speed, aye!
speed (knots): 12.5
```

### Using the `private` Keyword

Preceding the `func` keyword with the `private` keyword tells the compiler to limit the visibility of that method to **within the current file.** This means that you cannot call a private method from elsewhere in your module (nor from a testing suite). It is a rather strict limitation of the method's visibility that should be used with discretion. 

**Objective-C:** *Since Objective-C only has the two visibility levels public & private, Swift's default "internal" visibility level is oftentimes sufficiently private for most purposes.*

To make our `printSpeedKnots()` function private, we simply precede its `func` keyword with the `private` keyword:

```swift
    private func printSpeedKnots() {
        print("speed (knots): \(speedKnots)")
    }
```
This still allows our calls of the method within `Boat.swift` to be made using the `self.` keyword:

```swift
let sailors = ["The Skipper", "Gilligan", "Mary-anne"]
let minnow = Boat(name: "SS Minnow", sailors: sailors, maxSpeedKnots: 25.0)

minnow.fullSpeedAhead()
minnow.fullStop()
minnow.aheadOneHalfSpeed()
```
This will still print:

```
Full speed ahead, aye!
speed (knots): 25.0
Full stop, aye!
speed (knots): 0.0
Ahead one half speed, aye!
speed (knots): 12.5
```
However, we cannot access the method directly from the AppDelegate:

```swift
let sailors = ["The Skipper", "Gilligan", "Mary-anne"]
let minnow = Boat(name: "SS Minnow", sailors: sailors, maxSpeedKnots: 25.0)

minnow.fullSpeedAhead()
minnow.fullStop()
minnow.aheadOneHalfSpeed()

minnow.printSpeedKnots()  // error
```

![](https://curriculum-content.s3.amazonaws.com/swift/swift-methods/error_private_method_not_visible.png)

### Methods With Returns

Since method syntax also matches function syntax in regards to returns, we write can write a method that provides a returns using the same familiar syntax:

```swift
//  Boat.swift

class Boat {
    ...
    
    func soundRollCall() -> [String] {
        var rollCall = [String]()
        for sailor in sailors {
            rollCall.append("\(sailor) is present!")
        }
        return rollCall
    }
}
```
We can then call our returning method from the AppDelegate and print the result:

```swift
let sailors = ["The Skipper", "Gilligan", "Mary-anne"]
let minnow = Boat(name: "SS Minnow", sailors: sailors, maxSpeedKnots: 25.0)
        
print(minnow.soundRollCall())
```
This will print:

```
["The Skipper is present!", "Gilligan is present!", "Mary-anne is present!"]
```

### Methods With Arguments

The same goes for methods with arguments:

```swift
//  Boat.swift

class Boat {
    ...

    func maroonSailorAtIndex(index: Int) {
        let marooned = sailors.removeAtIndex(index)
        print("\(marooned) has been marooned, sir!")
    }
}
```

We can then call this method while supplying it with an appropriate argument:

```swift
let sailors = ["The Skipper", "Gilligan", "Mary-anne"]
let minnow = Boat(name: "SS Minnow", sailors: sailors, maxSpeedKnots: 25.0)
        
minnow.maroonSailorAtIndex(1)
        
print(minnow.soundRollCall())
```
This will print:

```
Gilligan has been marooned, sir!
["The Skipper is present!", "Mary-anne is present!"]
```
Poor Gilligan!

![](https://curriculum-content.s3.amazonaws.com/swift/swift-methods/gilligan_meh.gif)