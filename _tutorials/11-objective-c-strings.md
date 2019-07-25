---
title: "Strings"
permalink: /tutorials/objective-c-strings/
excerpt: |
    This lesson discusses the NSString class and the various ways you can manipulate it.
---

## `NSString`

Strings are something you’ve dealt with your whole life, even if you didn’t know it. A string, in the world of programming, is a collection of characters. Any collection of characters, in fact. Every sentence of this lesson is a string. In Objective-C, we use and manipulate strings using a specific class, `NSString`. It is part of the Foundation framework and was written by Apple for use in iOS and macOS applications. [^nsstring] Let’s see how to create a new `NSString` object using the following example:

{% highlight objc linenos %}
NSString *timeTraveler = @"Marty McFly";
{% endhighlight %}

`"Marty McFly"` is a string literal and it has been assigned to a variable called `timeTraveler`. To create a string literal in code, we use the syntax that begins with an `@` at-symbol and is followed by a set of `""` double-quotes. String literals can contain any Unicode characters, so feel free to explore your keyboard a bit.

Now that we have a string, what can we do with it? How about we combine our traveler’s name with a pre-written sentence to describe his journey? To do so in Objective-C, we use a *class method* on the `NSString` class called `stringWithFormat:`. See the example below:

{% highlight objc linenos %}
NSString *bttf = [NSString stringWithFormat:@"%@ is back in time.", timeTraveler];
NSLog(@"%@", bttf);
{% endhighlight %}

### String Concatenation

At line 2, the following string would actually print to the console, `"Marty McFly is back in time."` Line 1 represents an example of *string concatenation*, or combining one string with another. In this case, we’re combining `"Marty McFly"` with `" is back in time."` To use `stringWithFormat:`, you provide a formatting string, which is what follows the `:` in the method name, and then a comma separated list of the variables you’d like to insert into the formatting string. `%@` is a special set of characters that act as a wildcard for inserting an object in its place when this line of code is run. `%@` can be used for any object. If you want to insert a number, be it an integer or a floating-point number, there are other wildcard character sets that need to be used. See some examples below:

{% highlight objc linenos %}
int theSecondOne = 2;
NSString *bttf2 = [NSString stringWithFormat:@"Back to the Future %d", theSecondOne];
NSLog(@"%@", bttf2);
int theThirdOne = 3;
NSString *title = @"Back to the Future";
NSString *bttf3 = [NSString stringWithFormat:@"%@ %d", title, theThirdOne];
{% endhighlight %}

In lines 1 and 2, we want to concatenate the integer `2` onto the end of the title, which we’ve defined literally in the `stringWithFormat:` method. Lines 4-6 show a similar approach, but in this case we’ve stored the raw title in its own `NSString` variable, rather than as a literal in the `stringWithFormat:` method. Since two of the three items we’d like to add are stored in variables, there isn’t much to the format string at line 6. We use the object wildcard `%@` to insert the title, and then we add a space character to separate the two items so it reads properly. Lastly, we use the integer wildcard `%d` to insert the integer value from our `theThirdOne` variable. Inserting a `float` would require use of the `%f` wildcard.

### String Separation

Sometimes you want to split a string into pieces rather than combine multiple elements into a single string. The `NSString` class has a method for this as well. The difference is this method acts on `NSString` objects rather than directly on the `NSString` class. Such methods are called *instance* methods. See below for a simple example.

{% highlight objc linenos %}
NSString *timeTravelEquipment = "Time Circuits, Flux Capacitor, Mr. Fusion";
NSArray *individualParts = [timeTravelEquipment componentsSeparatedByString:@", "];
{% endhighlight %}

As you can see at line 1, we have a comma separated list of equipment necessary for time travel. Line 2 shows how you can separate this single string into an `NSArray` of string objects, where we’re using the string `", "` to separate each element. We need to include the comma and the space character in the separator string as that matches the characters that separate each piece of equipment in the larger string.

How many elements would we have in the array `individualParts` after we call the `componentsSeparatedByString:` method?

**Answer:** There are 3 strings separated by the `", "` separator string, so the count of the array would be 3.
{: .notice--success}

## Recap
Let's take a look at the concepts covered in this lesson:
* The `NSString` class and how to create them
* String concatenation with `stringWithFormat:` method
* String separation with `componentsSeparatedbyString:` method

[^nsstring]: [https://developer.apple.com/documentation/foundation/nsstring](https://developer.apple.com/documentation/foundation/nsstring)