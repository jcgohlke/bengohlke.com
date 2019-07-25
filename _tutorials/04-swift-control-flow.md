---
title: "Swift Control Flow"
permalink: /tutorials/swift-control-flow/
excerpt: |
    You'll learn how to control the flow of your code with things like if-statements and for-loops.
---

All code presented here is compatible with Swift 4.2.
{: .notice--info}

Now that you have an idea of how to write basic instructions (creating and manipulating constants and variables) you will want to be able to form more complicated blocks of instructions. Enter control flow. The concepts that follow allow the program order of execution to be modified at *runtime*. This means that you can set up different forks in the road and the computer will choose a particular road when the program executes. This is commonly used when you might have two or more sets of instructions you'd like to run, but you won't know which is appropriate until the application is running and it contains live data. Let's take a look at the first control flow mechanism, the *if statement*.

## If... or Else

Let's say you have some code that should only run *if* certain conditions are met at runtime. To do that, you wrap those instructions in an `if` block.

{% highlight swift linenos %}
var shipCaptains = ["Malcolm Reynolds", "Jean-Luc Picard", "James T. Kirk", "Han Solo"]
let engineers = ["Montgomery Scott", "Geordi LaForge", "B'Elanna Torres"]
if shipCaptains == engineers
{
  print("Arrays are equal")
}
else
{
  print("Arrays are not equal")
}
{% endhighlight %}

You might recognize the first couple lines from the previous lesson. Let’s use them to decide what to print. You'll notice in `if` statement block we use something called a *conditional statement*. This statement must evaluate to a boolean answer, i.e. `true` or `false`. Since we're checking here whether the `shipCaptains` array is equal to the `engineers` array with the equality operator, the statement will be evaluated to a boolean answer. What follows this line is a set of `{}` where you place all instructions that need to be run if the conditional line evaluates to `true`. The `else` block is optional. `if` blocks are not required to have an `else` clause, but you may use one if you have instructions that should run only if the conditional statement is `false`. In this case, we want a sentence to print either way, so we have an `if` clause and an `else` clause. Based on what we know of the two arrays referenced here, which print statement will execute at runtime?

The two arrays are not equal, so the instructions inside the `if` clause are skipped and the instructions inside `else` are run instead.

**NOTE:** several languages demand that a set of `()` parentheses be used to wrap the conditional expression after the word `if`. In Swift these parentheses are allowed but not required. So feel free to use them if they help you and skip them if they don't.
{: .notice--warning}

## Switch it Up

Believe it or not, most control flow issues can actually be resolved with if-else blocks. You might need to chain several of them together, but you can create a pretty complicated code path using just `if`'s. The problem is many nested `if`'s can be very difficult to follow and debug. Because of that, what follows is a set of control flow concepts that allow for both more complicated `switch`ing as well as looping.

If you have several different paths of code, but only want to execute one of them at runtime, use a `switch` block. See below for an example:

{% highlight swift linenos %}
let a = 3
switch a
{
  case 1:
    print("Got 1")
  case 2:
    print("Got 2")
  case 3, 4, 5:
    print("Got 3 or 4 or 5")
  case 6...22:
    print("Got 6 to 22")
  default:
    print("Got Default")
}
{% endhighlight %}

As you can see above, the `switch` block is bounded by a set of `{}` braces. The value that you're "switching" with is listed after the reserved keyword `switch`. Each possible outcome is listed inside the braces as a `case`. A few notes about cases: values can be chained together (see the third case), they can be a range (every number between the lower and upper bound, see the fourth case), and Swift requires every `switch` block to include a `default` case (see the final case). The default is a catch-all to cover possible values that haven’t been enumerated by specific cases. 

**NOTE:** the value switched can be of any object type, a difference with some other languages that restrict the data types possible.
{: .notice--warning}

Bottom line: many different cases can be listed (and each case can have multiple lines of instructions), but at runtime, since the value of the switched variable or constant (in this example, `a`) will only be satisfied by one case, only one block of instructions will run. All other cases will be skipped.

## Looping

We've been talking about deciding which block of code to run based on certain runtime conditional values, but what if we wanted to run a block of code repeatedly a certain number of times? Or run until certain conditions are met? For that we have looping. Let's take a look at `for` loops first.

### `for`

These loops allow you to run a set of instructions a certain finite number of times. The most common way to bound a `for` loop is to provide it with a collection of data and have it iterate over each element in that collection. Thus if the collection has 10 items, the `for` loop will run its block 10 times.

{% highlight swift linenos %}
let names = ["Han Solo", "Kathryn Janeway", "James Kirk", "Jean-Luc Picard", "Malcolm Reynolds"]
 
for name in names
{
  print("Hello Captain \(name.split(" ")[1])")
}
{% endhighlight %}

The above loop looks at each name in turn from the names array and prints out the message "Hello Captain " and places the captain's last name at the end of the string using string interpolation. The method `split` operates on a string and allows for separating the string into an array of components based on the separator value passed in. Here, we pass in the space character and then ask the array returned for the second element (index 1), which would be the captain's last name.

### `while`

`while` loops are used to perform an instruction or set of instructions until some condition is met. The loop runs an undefined number of times rather than a predefined number like with a `for` loop. Let's say we wanted to count to 100 by a factor of 5 each time. We could arrange a loop such that it will end after we reach 100. See below for an example:

{% highlight swift linenos %}
var n = 0
while n <= 100
{
  n += 5
  print(n)
}
{% endhighlight %}

The above code block will print each number between 5 and 100, jumping by 5 each time. The block is evaluated as follows: the expression after the reserved keyword `while` evaluates to `true` or `false` depending on the value of `n`. As long as the value is less than 100, the block inside the braces is evaluated. The value of `n` is increased by 5 and then printed. Once the end of the block is reached, the loop begins again, as long as `n` is still less than 100. At some point the value of `n` will be increased beyond 100, and when the loop begins again at that point, the conditional will evaluate to `false`, thus ending the loop.

### `repeat-while`

If you’d like to ensure the instructions inside your loop run *at-least* once, you can use an alternate form of the `while` loop, something called `repeat-while`. See the example below:

{% highlight swift linenos %}
var m = 0
repeat
{
  m+=5
} while m <= 100
{% endhighlight %}

The above block is essentially the same as the previous `while` example, with one important difference. The block inside the brackets always executes at-least once, and then again in a loop if the expression is `true`. `while`  loops can run 0 or more times, whereas `repeat-while` loops can run 1 or more times. Deciding which to use in your code is a decision that will be based on the specific problem you’re trying to solve.

**Did You Know?** The `repeat-while` construct exists in other languages, where it’s known as do-while. In fact, until Version 2, it was known as do-while even in Swift. Apple changed the name to increase readability and user-friendliness of the concept. This is something they’ve done all over the language and will continue to do for a while. Be sure to keep pace with Apple’s changes as some of them might break compatibility of your source code. Look [here](https://developer.apple.com/swift/) for all things Swift.
{: .notice}

## Recap
Let's take a look at the concepts covered in this lesson:

* Controlling the flow of code with `if-else` and `switch` blocks
* Running the same block of code multiple times with loops: `for` , `while`, and `repeat-while`