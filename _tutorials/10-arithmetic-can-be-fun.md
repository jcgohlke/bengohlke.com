---
title: "Arithmetic Can be Fun"
permalink: /tutorials/arithmetic-can-be-fun/
excerpt: |
    This lesson provides additional exploration of various data types. You'll learn how to decide which type to use, how to manipulate different values and how to convert between types.
---

## What is a Literal?

Before we get further into data types, let's briefly discuss what a literal is. Remember in the previous lesson when we saw the strings `@"Kathryn Janeway"` and `@"awesome"`? Those are string *literals*. Essentially, a literal is mostly what it sounds like, a collection of characters that have no symbolic value to the programming language. See below for a couple examples:

{% highlight objc linenos %}
42 // integer literal
3.14159 // float literal
@"Hello, World!" // string literal
{% endhighlight %}

Something like `int`, `float`, or `NSString` are collections of characters that have special meaning to the Objective-C compiler. They inform the compiler about the characters and/or words that follow them. Literals, on the other hand, simply represent the literal value of the characters that make them up. `42` is the whole number 42.

## Basic Math

So now that we've seen two different number types, `int` and `float`, let's try to perform some basic arithmetic.

To add, subtract, multiply, or divide, we use `+`, `-`, `*`, and `/` respectively. See some examples below:

{% highlight objc linenos %}
int a = 5;
int b = a + 3; // b is 8
int c = b - a; // c is 3
int d = c * 5; // d is 15
float e = d / 3.2; // e is 4.6875
int myMod = 9 % 4; // myMod is 1

int x1 = 1;
x1 += 5; // x1 is now 6
x1 -= 2; // x1 is now 4
x1 *= 5; // x1 is now 20
x1 /= 4; // x1 is now 5
{% endhighlight %}

There is a lot to unpack here, so we should start with the first 4 lines. Line 1 is simply declaring and initializing a variable `a` with the value `5`. Lines 2-4 perform some arithmetic with previous values, and create new values from the outcomes of that arithmetic.

Line 5 is a little more complicated. Looking at line 4, we see that the variable `d` is of type `int`. At line 5 though, we want to use the value of `d` to divide it by the literal value `3.2`. The problem is, this value is of type `float`. It is not possible to perform arithmetic on values of different datatypes. However, Objective-C will automatically *promote* the value of `d` from integer to floating-point without any instruction from the programmer. Once this is done, the arithmetic can be evaluated and we'll end up with a `float` value stored in `e`.

Line 6 performs a *modulo* operation[^modulo]. Basically, the mod character `%` performs a division calculation but rather than returning the quotient, it returns the remainder. Thus in the expression `9 % 4`, it performs 9 divided by 4, which is 2 with a remainder of 1. It discards the 2 and returns the remainder.

Lines 8-12 are showing how the addition, subtraction, multiplication, and division characters can be paired with the assignment operator `=` to take a value from a variable, perform arithmetic on it, and re-assign the outcome back to the same variable. Hence, `x1 += 5` is equivalent to `x1 = x1 + 5`. In both cases, the value of `x1` would be `6` after the line is executed.


**Warning:** A quick final note regarding simple math -- do not divide by `0`, as it produces an undefined result. This will cause your app to crash. If you are unsure as to the value of your divisor, check for `0` before performing the division.
{: .notice--warning}

## Combining Numbers with Strings

{% highlight objc linenos %}
NSString *label = @"The table is ";
int width = 96;
NSString *widthLabel = [NSString stringWithFormat:@"%@ %d inches long.", label, width];
{% endhighlight %}

To combine a string with a number, it is necessary to convert the integer at line 2 into a string. This is done as shown in line 3. Again, there is a lot going on in line 3, and it is not necessary to understand every character. The important parts are that `stringWithFormat` allows us to use a string literal (this part: `@"%@ %d inches long."`) as a formatting string to create another string. The `%@` characters mean that a string should be inserted here, which in this case is the value of `label`. The `%d` is used when an integer should be inserted, or the value of `width` from line 2. What we'll end up with in the variable `widthLabel` after line 3 executes is "The table is 96 inches long."

## Recap
Let's take a look at the concepts covered in this lesson:

* Literal values
* `int` and `float` data types
* Simple arithmetic with these types: `+`, `-`, `*`, `/`
* *modulo* operator, `%`
* String formatting using wildcard characters and the `stringWithFormat:` method.

[^modulo]: [https://en.wikipedia.org/wiki/Modulo_operation](https://en.wikipedia.org/wiki/Modulo_operation)