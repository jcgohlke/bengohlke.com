---
title: "Your First Objective-C Program"
permalink: /tutorials/your-first-objective-c-program/
excerpt: |
    Get your first look at some of the foundational concepts of the Objective-C programming language with the following explanations and examples.
---

## What is Objective-C and where is it used?

The Objective-C programming language was developed in the early 1980s and has predominately been used by Apple for iOS and macOS app development. It is based on the C language, but adds object-oriented concepts and a messaging system styled after similar concepts from the Smalltalk language.[^obj-c]

Objective-C is a very powerful language and is fully interoperable with the C language, meaning you can mix Objective-C and C source files in the same project. Let's take our first look at Objective-C code by implementing a traditional greeting used when exploring a new programming language, the famous "Hello, World!" program.

{% highlight objc linenos %}
#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        NSLog(@"Hello, World!");
    }
    return 0;
}
{% endhighlight %}

The above *code block* (multiple lines of related code) can be executed when placed in a file called "main.m". This is a complete program and will cause the characters between the double-quotes `""` to be printed to the screen. We'll unpack more of the syntax of this code in later lessons, but for now, simply know that the instruction doing the work of printing the message is on line 5. The rest is what we call *boilerplate* code to set up the correct environment to allow our instruction at line 5 to run.

**Did You Know?** The first version of OS X was essentially a modified version of another OS already on the market, called NextStep. This OS, built by Steve Jobs' company Next, was included in the purchase of Next by Apple in the late 90s and used as the basis for what we know as macOS today. iOS is also an extension of that original work. Hence the prefix "NS" you see in many names in the iOS and macOS SDKs.
{: .notice}

## Variables
 
{% highlight objc linenos %}
NSString *captain = @"Kathryn Janeway";
NSLog(captain);
{% endhighlight %}

You can think of variables kind of like aliases or nicknames for bits of data you want to use in multiple places. Now, instead of typing `@"Kathryn Janeway"` everywhere to refer to her, we can instead use the variable identifier `captain`, like we do in the second line to print her name.

From line 1 to the end of the program, the _identifier_ `captain` will always refer to the string `Kathryn Janeway`, unless the variable `captain` is set to another string at some later point.

When you _declare_ a variable, which is what we did at line 1 above, you start the line with a data type for the value you'll be using. We'll learn more about data types in the next lesson. After the data type, you provide an identifier, which can be anything you'd like, as long as it conforms to the following requirements:

* The first character cannot be any digit [0-9]
* The space character is not allowed in any position
* Any character [A-Z], [a-z], [0-9], or `_` is allowed in any position of the name (except for a number in the first position)

Here are some examples of valid identifiers:

{% highlight objc linenos %}
NSString *coding = @"awesome", *perfect = @"100%", *_travelBack = @"1985";
{% endhighlight %}

**NOTE**: We are declaring 3 variables here: `coding`, `perfect`, and `_travelBack`. They all happen to be of type `NSString` so we can use a *compound declaration* with a comma-separated list.
{: .notice--warning}

Notice that none of the identifiers have a digit as the first character, but that digits are allowed in other positions. Though not a requirement, the naming convention for variable identifiers is to use camel case.[^camel-case] Since spaces are not allowed, if an identifier is really a short phrase, the first letter of each word is capitalized to help with readability, except for the first character of the first word.

The example at line 1 above is also showing how variables can be *initialized*. Initialization is the process of assigning a value to a variable. So at line 1 we are both declaring and initializing those 3 variables. Tasks that while not required, are frequently done together.

Let's do some simple math with variables:

{% highlight objc linenos %}
int students = 30
int teachers = 4
int totalPeople = students + teachers
{% endhighlight %}

In the above block, we declare two different variables, `students` and `teachers` and initialize them to the values `30` and `4`, respectively. We then add them together and store the sum in a new variable called `totalPeople`. This new variable should have a value `34` after the addition.

## Recap
Let's take a look at the concepts covered in this lesson:

* Hello, World! program written in Objective-C (an important milestone for learning any programming language)
* Variables and how to create them

[^obj-c]: [https://en.wikipedia.org/wiki/Objective-C](https://en.wikipedia.org/wiki/Objective-C)
[^camel-case]: [https://en.wikipedia.org/wiki/CamelCase](https://en.wikipedia.org/wiki/CamelCase)