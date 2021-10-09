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

For those new to the Swift language this object type may take some getting used to. If you didn't know already Swift doesn't believe in using `nil` as a value. Optionals provide a soloution to this essentially acting as a wrapper for any other object type. In Swift `nil` is a keyword that is used to equate whether or not an Optional has a value. An Optional may contain a value for the object it's wrapping although it also may very well be empty (it's fair enough to say that this object would make Schrodinger proud 🐱). 

<p align="center">
    <img src="https://media.giphy.com/media/IUm2IUJcZqKZnfWDDo/giphy.gif">
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

In this example there is no element in the array at index 10. Most languages would fail here with something in the form of an array out of bounds error. However this is not the case in Swift. Here the function `firstIndex()` returns an Optional (in this case an Optional `Int`). If the element exists then then value is returned neatly wrapped in an Optional, if not then an Optional equating to `nil` is returrned. Simple and neat 🎁.

# Decleration

An Optional `String` can be declared the following ways: 

``` swift 
var middleName1 = Optional("Gertrude") // Less used syntax 
var middleName2 : String? = "Gertrude" // More common
```  

We can also decare without a value (equating to `nil`):

``` swift 
var middleName1 : String?
var middleName2 : String? = nil // Does the same as above in a more explicit fashion
```  
