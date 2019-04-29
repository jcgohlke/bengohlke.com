---
layout: page
title: "Swift Collections"
---

This lesson shows how objects can be grouped using different collection data types, like arrays and dictionaries.

__All code presented here is compatible with Swift 4.2.__

## Collections

Now that you can create and work with different pieces of data, it can be helpful to group these bits of data together. This allows you to move them around as a group, and even perform sorting/filtering on them. To accomplish these tasks we use *collections*. The two main collection types in Swift are *arrays* and *dictionaries*.

### Arrays

An array is an ordered list of objects. These could be a list of strings, integers, boolean values, etc. To create an array in Swift, you wrap the list with `[]` brackets. See an example below:

```swift
var shipCaptains = ["Malcolm Reynolds", "Jean-Luc Picard", "James T. Kirk", "Han Solo"]
```

An array can hold any kind of object, as long as each element are of the same type. Mixing data types in an array is not allowed in Swift.

To access a member of an array, simply reference it by its index value. Remember, arrays in most languages, including Swift, are indexed starting at 0. So the third element's index is actually 2.

```swift
let oldSchoolCap = shipCaptains[2]
```

`oldSchoolCap` would have a value of `James T. Kirk` after assignment. The same technique can be used to add a value to an array at a specific index, provided the array itself has been declared as a variable and not a constant (`var` vs. `let`).

```swift
shipCaptains[4] = "Kathryn Janeway"
```

The `shipCaptains` array now has 5 elements, with Janeway being added at the end.

#### Array Comparison

Now that we have an array, let's build another one so we can perform some comparison.

```swift
let engineers = ["Montgomery Scott", "Geordi LaForge", "B'Elanna Torres"]
```

To check if these two arrays are equivalent, we can use the *equality operator* `==`. A single `=` symbol is called the *assignment operator*, and it is used to assign a value on the right to a variable or constant on the left. Two `=` symbols check values on either side for equality.

```swift
shipCaptains == engineers
```

The above expression will evaluate to a boolean answer, true or false. True, if the values are equal, and false if they are not. For arrays, equality means "equivalent objects, in the same order". Looking at the two arrays above, how would the expression be evaluated?

Since the objects in the two arrays are neither the equivalent nor in the same order, it would evaluate to `false`. Different objects respond differently to the `==` equality operator, so keep that in mind when using it.

### Dictionaries

Dictionaries are the other major collection type in Swift. They are known by other names in other languages: hashes in Ruby and Perl, maps in Java and Go, and hash tables in Lisp. The concept is the same in every language though. Dictionaries can be described as a collection of *key-value* pairs. Rather than referencing a value with an index like in an array, a dictionary *value* is fetched using a *key*. See the example below:

```swift
var occupations = ["Jean-Luc Picard": "Captain", "Geordi LaForge": "Chief Engineer"]
let geordisJob = occupations["Geordi LaForge"]
occupations["Beverly Crusher"] = "Chief Medical Officer"
```

The first line above shows how to create a new dictionary from scratch. Just like with arrays `[]` brackets are used to bound the dictionary elements. Also like an array, each key-value pair is separated by a comma. Unique to a dictionary however is the formation of the pair itself. The object on the left of the `:` is the key, and the right-side object is the value. Just like with arrays, the datatypes for each can be any object, as long as they are consistent throughout the dictionary. So a dictionary with `String` keys and `Int` values must contain pairs only with these two specific types.

Fetching a value from the dictionary can be done as shown in the line that fetches the job for `Geordi LaForge`. Similar to object fetching in an array, you simply wrap the key associated with the desired value in `[]` brackets and place it after the name of the dictionary. Also similar to arrays, adding a key-value pair is accomplished like you see where we add the job for `Beverly Crusher`. Just be sure that the dictionary has been declared as variable, not a constant.

## Recap
Let's take a look at the concepts covered in this lesson:

* What is a collection and why is it useful?
* Arrays: ordered lists of like data
* Dictionaries: unordered collection of data accessed with a particular key
