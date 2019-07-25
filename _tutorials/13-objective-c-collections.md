---
title: "Collections"
permalink: /tutorials/objective-c-collections/
excerpt: |
    This lesson shows how objects can be grouped using different collection data types, like arrays and dictionaries.
---

Now that you can create and work with different pieces of data, it can be helpful to group those pieces together. This allows you to move them around as a group, and even perform sorting/filtering on them. To accomplish these tasks we use *collections*. The two main collection types in Objective-C are *arrays* and *dictionaries*.

## Arrays

An `NSArray` is an ordered list of objects. These could be a list of strings, numbers, dictionaries, view controllers, etc. To create an array in Objective-C, you must create an `NSArray` object (or `NSMutableArray` if you want to edit the contents over time) and fill it with elements. See an example below:

{% highlight objc linenos %}
NSMutableArray *shipCaptains = [[NSMutableArray alloc] initWithObjects:@"Malcolm Reynolds", @"Han Solo", @"James T. Kirk", @"Jean-Luc Picard"];
{% endhighlight %}

An array can hold any kind of object, so long as they are actually objects. Primitive types (ints, floats, BOOLs, etc) are not allowed in arrays in Objective-C. Don’t worry about understanding all the syntax in the above example. We’ll cover objects in more depth in a future lesson.

To access a member of an array, simply reference it by its index value. Remember, arrays in most languages, including Objective-C, are indexed starting at 0. So the third element's index is actually `2`.

{% highlight objc linenos %}
NSString *oldSchoolCap = shipCaptains[2];
{% endhighlight %}

`oldSchoolCap` would have a value of `James T. Kirk` after line 1. The same technique can be used to add a value to an array at a specific index (assuming the array is mutable).

{% highlight objc linenos %}
shipCaptains[4] = @"Kathryn Janeway";
{% endhighlight %}

The `shipCaptains` array now has 5 elements, with `Kathryn Janeway` appended to the end.

The problem with the initialization of the array is that it uses more characters than we’d like. Something many programmers strive for is to express an instruction in fewer characters, since it requires fewer keystrokes and therefore is more efficient. Fortunately, Apple has provided a shortcut for us to create `NSArray` objects.

{% highlight objc linenos %}
NSArray *moreCaptains = @[@"Benjamin Sisko", @"Spock", @"Hikaru Sulu"];
{% endhighlight %}

The above example is equivalent to the way we declared and initialized `shipCaptains` in the first example. This is just fewer characters. To create an `NSArray` literal, simply wrap the object list in `[]` and prefix an `@` on the front. Note that this only works to create `NSArray` objects, not its mutable counterpart `NSMutableArray`.

**Did You Know?** The above shortcut in syntax is known generally as a *syntactic sugar*. Sugars exist in many languages and they are primarily designed to create a shorthand for writing certain instructions. They're kind of like keyboard shortcuts in an app; not something you need to use, but they can power up your workflow and make code easier to write. The downside is they are sometimes more difficult to understand, since you need to know the shorthand symbols. The increase in speed and efficiency is usually worth the decrease in approachability however.
{: .notice}

### Array Comparison

Now that we have an array, let's build another one so we can perform some comparison. We’ll use our shorthand initialization from now on when building `NSArray` objects.

{% highlight objc linenos %}
NSArray *engineers = @[@"Montgomery Scott", @"Geordi LaForge", @"B’Elanna Torres"];
{% endhighlight %}

To check if these two arrays are equivalent, we should be able to use the *equality operator* `==`. A single `=` symbol is called the *assignment operator*, and is used to assign a value on the right to a variable on the left. Two `=` symbols check values on either side for equality.

{% highlight objc linenos %}
if (shipCaptains == engineers) {
  NSLog("Arrays are equal!")
}
{% endhighlight %}

The above conditional expression is comparing the two objects, but not in the way you might think. As you learned in a previous lesson, all objects in Objective-C are pointers, or arrows to a spot in memory where the actual value of the object is stored. When you compare two objects using the equality operator `==`, you are actually comparing the pointers of each object. So in the example above, the `shipCaptains` array was created and assigned a spot in memory to store its data. When the `engineers` array was created, it was assigned a different spot in memory to store _its_ data. Each pointer will therefore point to different spots in memory. Hence, the conditional above will always evaluate to `NO` and the console statement will never print.

We don’t want to compare the pointers for each array, but rather the contents of those arrays. In fact, equality for arrays means "equivalent objects, in the same order". Fortunately for us, Apple has written a _method_ for `NSArray` that does just that. Below is the actual expression that should be used to compare two arrays for equality:

 {% highlight objc linenos %}
if ([shipCaptains isEqualToArray:engineers]) {
  NSLog("Arrays are equal!")
}
{% endhighlight %}

The above expression will evaluate to a boolean answer, `YES` or `NO`. Looking at the two arrays above, which would it be?

**Answer:** Since the objects in the two arrays are neither equivalent nor in the same order, it would evaluate to `NO`. 
{: .notice--success}

So, what is `==` good for in Objective-C? It works just fine when comparing primitive types (`int`, `float`, `BOOL`, etc). Something like the following shows a perfectly acceptable use case:

{% highlight objc linenos %}
int five = 5;
int six = 6;
BOOL equalNumbers = five == six;
{% endhighlight %}

What is the value of `equalNumbers`  after line 3?

**Answer:** Five is not equal to six, so it’s `NO`.
{: .notice--success}

## Dictionaries

Dictionaries are the other major collection type in Objective-C. They are known by other names in other languages: *hashes* in Ruby and Perl, *maps* in Java and Go, and *hash tables* in Lisp. The concept is the same in every language though. Dictionaries can be described as a collection of *key-value* pairs. Rather than referencing a value with an index like in an array, a dictionary *value* is fetched using a *key*. See the example below:

{% highlight objc linenos %}
NSMutableDictionary *occupations = [[NSMutableDictionary alloc] initWithObjects:@[@"pilot", @"jedi", @"sith lord"] forKeys:@[@"Han Solo", @"Luke Skywalker", "Darth Vader"]];
NSDictionary *moreOccupations = @{@"Jean-Luc Picard": @"Captain", @"Geordi LaForge": @"Chief Engineer"};
let darkSideJob = occupations[@"Darth Vader"];
occupations[@"Leia Organa"] = @"Senator";
{% endhighlight %}

Line 1 shows how to create a new dictionary from scratch. Like with `NSArray`, the formal way to initialize an `NSDictionary` is to use the *alloc-init* pattern. We’re creating a mutable version here so we can edit its contents later. As you can see, it’s quite a lot of extra typing. Also like `NSArray`, Apple has created a sugar for creating `NSDictionary` literals. As shown in line 2, the entire list is encapsulated in `{}` with an `@` prefixed to the front. Inside the list, each key-value pair is separated by a comma. As for the pair itself, the object left of the `:` is the key, and the right-side object is the value. Just like with arrays, the data types for each can be any object, so long as they are objects and not primitives. The same caveats apply here as they did to `NSArray`. The sugar syntax is only valid for creating immutable dictionaries. If you want to create an `NSMutableDictionary`, you have to use the longhand initializer, like the one at line 1.

Fetching a value from the dictionary can be done as shown in line 3. Similar to object fetching in an array, you simply wrap the key associated with the desired value in `[]` brackets and place it after the name of the dictionary. Also similar to arrays, adding a key-value pair is accomplished like you see in line 4. Just be sure that the dictionary has been declared as an instance of `NSMutableDictionary`.

## Recap
Let's take a look at the concepts covered in this lesson:

* What is a collection and why is it useful?
* Arrays: ordered lists of like data
* Dictionaries: unordered collection of data accessed with a particular key