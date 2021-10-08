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

### Introduction 

For those new to the Swift language this object type may take some getting used to. If you didn't know already Swift doesn't allow for regular object types to be assigned a value of nil. This is where Optionals come in essentially acting as a wrapper for any other object type. It may contain the object it's claiming to hold although it also may very well be empty (it's fair enough to say that this object would make Schrodinger proud 🐱). 

### Scenario

So first of all we need to answer the question, what is the need for this Object Type? Well... let me present the following scenario. Imagine you are developing an application that requires the user to input their name. Your first thought is to understand what components make up a name (first-name, middle-name and surname) and how you might represent these in your program (here's hoping you though with Strings 🤞). However it dawns on you that it is entirely possible that your user may not have a middle name or that they do but it's far too embarrassing for them to disclose. 

Now yes I agree, we could just use an empty String (not all scenarios are perfect but we're rolling with it) however that would not truly convey the lack of value. How do you represent this in language that doesn't allow you for the free use of nil 🤨? 

### Usage

Well that's where the hero of our story enters - [The Optional](https://www.youtube.com/watch?v=A_HjMIjzyMU) 🦸‍♂️. As alluded to before an Optional allows you to wrap another object, like so: 

``` swift 
var middleName = Optional("Gertrude")
```  

Or the more common syntax: 

``` swift 
var middleName : String? = "Gertrude"
```  

Or in the case where you want to represent 'lack of value':

``` swift 
var middleName : String? = nil
```  
