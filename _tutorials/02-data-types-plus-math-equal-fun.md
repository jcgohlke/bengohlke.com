---
title: "Data Types + Math = Fun"
permalink: /tutorials/data-types-plus-math-equal-fun/
tags: 
  - swift
excerpt: |
    This lesson provides additional exploration of various data types. You'll learn how to decide which type to use, how to manipulate different values and how to convert between types.
---

All code presented here is compatible with Swift 4.2.
{: .notice--info}

## What is a Literal?

Before we get further into data types, let's briefly discuss what a literal is. Remember in the previous lesson when we saw the strings `Kathryn Janeway` and `Coding is fun`? Those are string literals. Essentially, a literal is mostly what it sounds like, a collection of characters that have no symbolic value to the programming language. See below for a couple examples:

```swift
42 // integer literal
3.14159 // double literal
"Hello, World!" // string literal
```
Something like `var`, `let`, or `Int` are collections of characters that have special meaning to the Swift compiler. They inform the compiler about the characters and/or words that follow them. Literals, on the other hand, simply represent the literal value of the characters that make them up. `42` is the whole number 42.

## Basic Math

So now that we've seen two different number types, `Int` and `Double`, let's try to perform some basic arithmetic.

To add, subtract, multiply, or divide, we use `+`, `-`, `*`, and `/` respectively. See some examples below:

{% highlight swift linenos %}
var a = 5
var b = a + 3 // b is 8
var c = b - a // c is 3
var d = c * 5 // d is 15
var e = Double(d) / 3.2 // e is 4.6875
var myRemainder = 9 % 4 // myRemainder is 1

var x1 = 1
x1 += 5 // x1 is now 6
x1 -= 2 // x1 is now 4
x1 *= 5 // x1 is now 20
x1 /= 4 // x1 is now 5
{% endhighlight %}

There is a lot to unpack here, so we should start with the first 4 lines. Line 1 is simply declaring and initializing a variable `a` with the value `5`. Lines 2-4 perform some arithmetic with previous values, and create new values from the outcomes of that arithmetic.

Line 5 is a little more complicated. As we covered in the previous lesson, Swift is *type safe*, meaning that it will not automatically convert one data type to another. Looking at line 4, we see that the variable `d` is of type `Int`, because of *type inference*. We know that because all the literal values that appear in the preceding lines are integers, thus all variables `a-d` are of type `Int`. At line 5 though, we want to use the value of `d` and divide it by the literal value `3.2`. This value is of type `Double`. It is not possible in Swift to divide an integer by a double, because the values are mixed types. We can however, change the data type of one of the values to match the other and then perform the calculation. So in this example, we convert the value of `d` to a `Double` and then perform the arithmetic. This will provide us with a `Double` answer which will be stored in the variable `e`.

Line 6 performs a [*remainder* operation](https://en.wikipedia.org/wiki/Modulo_operation). Basically, the remainder character `%` performs a division calculation but rather than returning the quotient, it returns the remainder. Thus in the expression `9 % 4`, it performs 9 divided by 4, which is 2 with a remainder of 1. It discards the 2 and returns the remainder.

Lines 7-11 are showing how the addition, subtraction, multiplication, and division characters can be paired with the assignment operator `=` to take a value from a variable, perform arithmetic on it, and re-assign the outcome back to the same variable. Hence, `x1 += 5` is equivalent to `x1 = x1 + 5`. In both cases, the value of `x1` would be `6` after the line is executed.

A quick final note regarding simple math -- do not divide by 0, as it produces an undefined result. This will cause your app to crash. If you are unsure as to the value of your divisor, check for 0 before performing the division.

## Type Conversion

Here are a few more examples of type conversion. It is necessary to convert types whenever you want to combine values of different types, whether that be to perform an arithmetic expression, or to combine string and number types.

### Conversion to `Int`

{% highlight swift linenos %}
var f = 10
var g = 3.6
var h = f + Int(g) // h is 13
{% endhighlight %}

So, why does the expression in line 3 evaluate to 13? Shouldn't it be 13.6 or at least 14 if rounded? It evaluates to 13 because when a `Double` is converted to an `Int`, the remainder is simply dropped, without rounding. Keep this in mind if you perform mixed arithmetic with integers and floating-point numbers. It is usually best to promote everything to floating-point, and then convert back to an integer if necessary, possibly rounding if desired.

### Combining Numbers with Strings

{% highlight swift linenos %}
let label = "The table is "
let width = 96
let widthLabel = label + String(width) + "inches long."
{% endhighlight %}

To combine a string with a number, it is necessary to convert the integer at line 2 into a string. This is done as shown in line 3. Notice that the `+` symbol is used here to combined two strings. This is called *string concatenation*. In this case, the `+` operator has a different meaning than when used to add two numbers. It is important to remember that symbols may have different meanings to the compiler depending on the context in which they are used.

While this method will work, it can be difficult to read. Another technique can be used that is just as effective for combining strings with other data types, but is more readable by the programmer.

{% highlight swift linenos %}
let anotherWidthLabel = "The width is \(width)"
{% endhighlight %}

Using the same `width` constant from the previous example, this code combines the string `"The width is "` with the value of the `width` constant, `94`. Thus, the value of `anotherWidthLabel` after the line is executed is `"The width is 94"`. This technique is called *string interpolation*. It can be used to combine virtually any type with a string literal. Simply place inside the double quotes a backslash followed by two parens, `\()`. Then you can place any variable or constant inside the parens and the value will be converted to a string and placed inline in the string literal. You can even perform arithmetic or call a method inside the parens. It will be evaluated and then converted into a string.

## Recap
Let's take a look at the concepts covered in this lesson:

* Literal values
* `Int` and `Double` data types
* Simple arithmetic with these types: `+`, `-`, `*`, `/`
* *remainder* operator, `%`
* Type conversion for integer and floating-point numbers
* String concatenation and interpolation