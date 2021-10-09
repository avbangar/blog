---
title: "Optional Type"
date: 2021-10-04T21:15:14+01:00
description: "The basics of the Optional type in Swift"
tags: ["swift", "swift5"]
ShowBreadCrumbs: true
draft: false
ShowToc: false
---

I once heard Swift's Optional being described as an "enum on steroids 💪". Surprisingly enough that description was right on the money!

``` swift 
public enum Optional<Wrapped> : ExpressibleByNilLiteral {
    case none
    case some(Wrapped)
}
```

# Introduction 

For those new to the Swift language this object type may take some getting used to. If you didn't know already Swift doesn't believe in using `nil` as a value. Optionals provide a soloution to this essentially acting as a wrapper for any other object type. In Swift `nil` is a keyword that is used to equate whether or not an Optional has a value. 

An Optional may contain a value for the object it's wrapping although it also may very well be empty (it's fair enough to say that this object would make Schrodinger proud). 

<p align="center">
    <img width="30%" height="auto" src="https://media.giphy.com/media/IUm2IUJcZqKZnfWDDo/giphy.gif">
</p>
    
# Why? 

So first of all we need to answer the question, what is the need for this Object Type? Well... let me present the following:

``` swift 
let array = [1,2,3,4,5]
let x = array.firstIndex(of:10)
if x != nil {
    // Do something useful 
}
``` 

In this example there is no element in the array at index 10. Most languages would fail here with something in the form of an array out of bounds error. However this is not the case in Swift. Here the function `firstIndex()` returns an Optional (in this case an Optional `Int`). If the element exists then then value is returned neatly wrapped up in an Optional, if not then an Optional equating to `nil` is returrned. Simple and neat.

# Decleration

An Optional `String` can be declared the following ways: 

``` swift 
var middleName1 = Optional("Gertrude") // Less common syntax 
var middleName2 : String? = "Gertrude" // Most common
```  

We can also decare without a value (equating to `nil`):

``` swift 
var middleName1 : String?
var middleName2 : String? = nil // Does the same as above in a more explicit fashion
```  

## Implicit

Swift does allow for another method of Optional decleration which results in a *special type* of Optional - an **Implicitly Unwrapped** one: 

``` swift 
var middleName : String! = "Gertrude" 
```  

This kind of Optional can be passed into functions that expect it's unwrapped type without having to explicitly unwrap it (process shown in the section below). It's a slightly lazy way to use an Optional however in certain situations it may be good as it could improve readability. You're effectively saying to the compiler, "sorry I know I forgot to unwrap but do us a favour and don't crash".

# Unwrapping

Once we have an Optional how do we obtain the value which it's wrapping? 

The first example we are going to show is **force unwrapping**: 

``` swift 
var wrappedName : String? = "Gertrude"
let unwrappedName = wrappedMiddleName!
unwrappedName.uppercased() // unwrappedName = "GERTRUDE"
print(unwrappedName) 
```  

This is the most dangerous way to unwrap an Optional and if you choose to use this as your main technique you should get used to seeing - `Fatal error: Unexpectedly found nil while unwrapping an Optional value`. Now (if it isn't obvious from the error message) it is not possible to unwrap an optional when it is equatable to `nil` (i.e. holding no value).

Now the obvious way to avoid this is by doing the following: 

``` swift 
var wrappedName : String? = "Gertrude"

if wrappedName != nil {
    let unwrappedName = wrappedMiddleName!
    unwrappedName.uppercased() // unwrappedName = "GERTRUDE"
    print(unwrappedName) 
}
```  
However if you know anything about Swift, most of the time there's a cleaner solution... 

``` swift 
var wrappedName : String? = "Gertrude"

if let unwrappedName = wrappedMiddleName {
    unwrappedName.uppercased() // unwrappedName = "GERTRUDE"
    print(unwrappedName) 
}
```  

This is known as the **if let** unwrap. Here `unwrappedName` is used in the scope of the `if` statement if a value can be unwrapped from `wrappedMiddleName`. Another alternative is the **gaurd let** unwrap:

``` swift 
var wrappedName : String?

guard let unwrappedName = wrappedMiddleName else {
    print("You didn't assign wrappedName the value of Gertrude!")
    exit() 
}

unwrappedName.uppercased() // unwrappedName = "GERTRUDE"
print(unwrappedName) 
```  

The key difference here is that **guard let** allows you to use your unwrapped optional after the gaurd code (in the outer scope). Hopefully from the above you can understand how where this type of unwrapping got it's name... 

# Chaining

In the examples above we wish to apply the `uppercased()` function to the String before printing. Now we can actually shorten this process through the use of chaining: 

``` swift 
var wrappedName : String? = "Gertrude"
let unwrappedName = wrappedMiddleName!.uppercased()
print(unwrappedName) 
```  

This acts as a shorthand for **force unwrapping**, although just as before this may result in an issue if `wrappedName` does not hold a value. However thankfully there is a soloution for this, *unwrapping optionally*:

``` swift 
var wrappedName : String? = "Gertrude"
var unwrappedName = wrappedMiddleName?.uppercased()
print(unwrappedName) 
```  

This method works in the same manner as before if `wrappedName` has a value. However it differs in the situation where `wrappedName` has not been initialised. Here the use of `?` ensures that an Optional `String` is returned (instead of a `String` as above). Therefore (you guessed it) it is possible for `unwrappedName` to be empty (equatable to `nil`). 

# Map

Both functions `map()` and `flatMap()` can be called by an optional and you may find them to be quite useful. These can be used in the case where you don't want to unwrap an Optional just yet but you want to modify it's wrapped value. 

`map()` can be passed an anonymous function which is applied to the wrapped value: 

``` swift 
var wrappedName : String? = "Gertrude"
wrappedName.map{ $0.uppercased() }
print(wrappedName!) // Prints - GERTRUDE
```  

This also allows the change to be made in a safe manner. If `wrappedName` initially equates to `nil` then `uppercased()` is not applied and `wrappedName` maintains it's lack of value (equallity to `nil`). 

`flatMap()` is used in the situation where the anonymous function passed in produces an Optional. For example when coersing a `String` to an `Int`:

``` swift 
var wrappedString : String? = "42"
var wrappedInt = wrappedString.flatMap{ Int($0) } // Int($0) produces an Optional Int (Int?)
```  

Here `flatMap()` unwraps the value of the anonymous function before wrapping again. If `map()` was used in the example above we would end up with a double-wrapped optional (`Int??`).

# Comparison 

In an equality comparison Swift allows for the use of the Optional value in the comparison (thereby allowing the compairson to be made as if the Optional had been unwrapped). This means that the following comparison would equate to `true`:

``` swift 
var wrappedInt : Int? = 42
if wrappedInt == 42 {
    print("true")
}
```  

However this comparison only works with the `==` operator. It is not possible to do other comparisons such as (`<`, `>`, `<=`, `>=`, etc) without first unwrapping your optional. 

# Thanks

Time to wrap up 😉 

I do hope you found this blog post useful and feel free to reach out with any feedback!

Blog posts like these help me to solidify my knowledge but I also hope that they can be used to teach someone something new (optimistic I know). 

Anyways thanks for taking the time to read, I hope you enjoyed it 👋
