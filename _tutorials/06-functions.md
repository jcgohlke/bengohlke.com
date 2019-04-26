---
title: "Swift Functions"
---

In this lesson, you'll get an introduction on how to group instructions into logical units (functions) and then execute these groups on demand.

__All code presented here is compatible with Swift 4.2.__

The best part of computer programming is the ability to leverage the strengths of the computer to perform tasks that would be tedious and error prone if done by a human. Take, for example, calculating a tip on a restaurant bill. The math isn't too difficult, but one of the computer's biggest strengths is fast and reliable arithmetic. Let's write some code to calculate the tip on a bill.

```swift
let subtotal = 25.95
let tipPercentage = 0.2 
let tipAmount = subtotal * tipPercentage
let total = subtotal + tipAmount
print(total)
```

A simple implementation of the description above, but one we can't use again. The logic for calculating the tip is commingled with the specific numbers from this bill. What if we wanted to calculate another bill's tip? Or change the percentage of this bill's tip? Let's modify this code so we can use it repeatedly, by placing it inside a *function*. A function is a block of code that can be *called* in other parts of the program, thus making it reusable.

```swift
func calculateTip(subtotal: Double, tipPercentage: Double) -> String
{
  let tipAmount = subtotal * tipPercentage
  return String(format: "%d%% of $%0.2f is $%0.2f", Int(tipPercentage * 100), subtotal, tipAmount)
}

calculateTip(25.95, tipPercentage: 0.2)
```

There is a lot going on here so we'll break it down. The logic of the function is contained within the `{}`, and its name is `calculateTip`. The first line specifies it to be a function with the reserved keyword `func`. The name can be any string of characters you want, but the convention is to use *camel case* and to make it short and descriptive. The items in the parentheses are called *parameters*, and the data type following the `->` arrow is called the *return type*, or the kind of item the function will return to its caller. The first instruction of the function is essentially the same as line 3 from the previous example. The `return` statement line creates a string and returns the information in a human-readable format. Using the *function call* at the bottom of the example, the function would return the following string:

`20% of $25.95 is $5.19`

Understanding every bit of the above example isn't necessary right now. The point is that with this function, we can calculate a tip whenever we desire without needing to know how. We instead just call the function with the appropriate *arguments* (another name for parameters). The best functions are black boxes, they do what we ask without fail each time we call them. As you delve deeper into programming, you will no doubt call functions for which you can't even see the implementation. That's why proper function naming, parameter selection, and documentation surrounding your code is crucial -- so others can use your code without needing to inspect the implementation themselves to understand the functionality.

## Recap

Let's take a look at the concepts covered in this lesson:

* Using functions to group together related instructions