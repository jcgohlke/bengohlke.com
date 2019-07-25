---
title: "Data Types and Mutability"
permalink: /tutorials/data-types-and-mutability/
excerpt: |
    This lesson provides an exploration of various data types. You’ll see how they differ, how mutability protects your data, and the difference between primitive and object data types in Objective-C.
---

## Data Types

So far, we've seen two different _data types_ in Objective-C, integers and strings. There are several others, and in this lesson we'll learn about a couple more. See the list below for a description of each:

* `int` - integer, a whole number: `1, 2, 3, 4, 5...` not something like `2.5` or `¾`
* `float` - floating-point number, meaning a whole or fractional number: `1.5`, `3.75`, `1.128`, `3.666666667`, `6.0000`, etc.
* `BOOL` - a Boolean value, of which there are two: `YES` and `NO` (effectively true or false)
* `NSString` - a sequence of characters: `Kathryn Janeway`, `awesome`, etc. Strings can contain any Unicode character, including spaces.

Let's take a quick look at a few more data type examples:

{% highlight objc linenos %}
int pennies = 4;
float chocolateBars = 6.25;
NSString *crewMember = @"Geordi LaForge";
BOOL isCaptain = false;
{% endhighlight %}

As you can see from the above example, when an identifier is declared it must be associated with a particular data type. `pennies` needs to be an integer so it can store whole numbers, `chocolateBars` should be a floating-point number, `crewMember` a string (`NSString`), and `isCaptain` a boolean.

## Primitives

Some of the data types we’ve seen are called _primitives_. Examples include: `int` (integers), `float` (floating-point or decimal numbers), `double` (larger floating-point numbers), and `BOOL` (true or false). Primitives are the simplest elements available in Objective-C. They have a defined size in memory, so that an `int` created in one part of the program is the same size as another `int` created elsewhere. Consequently, these primitive datatypes have limits on the size of the value they can store. For `int`, that means a number between about -2 billion to +2 billion. Larger (or smaller) numbers will not "fit" in an integer variable. The other primitive types have similar constraints to the size of their values. The `BOOL` type can only ever equal `YES` or `NO`, which are stand-ins for true and false. The computer actually stores these values as `1` for `YES` and `0` for `NO`. So technically, `BOOL` is actually just a special kind of integer that has a range between `[0-1]`.

## Objects

Other data types, things like: `NSString`, `NSNumber`, `NSArray`, or `NSDictionary` are called object types. This means they are more complicated than a primitive and frequently take up more space in memory. We’ll learn more about objects in a future lesson, but for now, the simple way to differentiate primitives and objects is to look for/use a `*` when declaring an object variable, and to leave it out when declaring a primitive.

## Mutability

We’ve seen both primitive data types and object data types in use, but what if we want to edit these values after we set them? If the value to be edited is a primitive, the value can be summarily changed with no restrictions.

{% highlight objc linenos %}
int apples = 5;
apples = apples + 1;
NSLog(@"%d", apples);
{% endhighlight %}

The above code block declares and initializes a variable `apples` to a value of `5` and then adds `1` to that value. The last line simply prints the value of apples to the console when this code is executed.

For object variables, the general rule of thumb is that an object is *immutable* unless it has the word "mutable" in its class name. For example, `NSString` is immutable. This means once it is initialized, the value of that string cannot change. An `NSMutableString`, however, can be changed after it is initialized. It’s a similar story for variables that are `NSMutableArray`’s, `NSMutableData`, etc. This isn’t necessarily a globally applicable rule, but Apple does tend to follow this convention, and third party software tends to follow Apple’s lead.

## Recap
Let's take a look at the concepts covered in this lesson:

* Primitive data types: `int`, `float`, `double`, and `BOOL`
* Object data types: `NSString`, `NSNumber`, `NSArray`, and `NSDictionary`
* Variable mutability