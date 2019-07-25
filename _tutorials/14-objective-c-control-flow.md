---
title: "Control Flow"
permalink: /tutorials/objective-c-control-flow/
excerpt: |
    You'll learn how to control the flow of your code with things like if-statements and for-loops.
---

Now that you have an idea of how to write basic instructions, you will want to form more complicated blocks of instructions. Enter control flow. The concepts that follow allow the programmer to modify the execution order of instructions at *runtime*. This means that you can set up different forks in the road and the computer will choose a particular road when the program executes. This is commonly used when you might have two or more sets of instructions you'd like to run, but you won't know which is appropriate until the application is running and it contains live data. Let's take a look at the first control flow mechanism, the *if statement*.

## If... or Else

Let's say you have some code that should only run *if* certain conditions are met at runtime. To do that, you wrap those instructions in an `if` block.

{% highlight objc linenos %}
NSArray *shipCaptains = @[@"Malcolm Reynolds", @"Jean-Luc Picard", @"James T. Kirk", @"Han Solo"];
NSArray *engineers = @[@"Montgomery Scott", @"Geordi LaForge", @"B’Elanna Torres"];
if ([shipCaptains isEqualToArray:engineers]) {
    NSLog(@"Arrays are equal");
} else {
    NSLog(@"Arrays are not equal");
}
{% endhighlight %}

You might recognize lines 1 and 2 from the previous lesson. Let’s use them to decide what to print. You'll notice in line 3 we type `if` followed by something called a *conditional statement*. This statement must evaluate to a boolean answer, i.e. `YES` or `NO`. Since we're checking here whether `shipCaptains`  is equal to `engineers` with the NSArray-supplied equality method, the statement will be evaluated to a boolean answer. What follows this line is a set of `{}` where you place all instructions that need to be run if the conditional line evaluates to `YES`. Lines 5-7 are optional. `if` blocks are not required to have an `else` clause, but you may use one if you have instructions that should run only if the conditional statement is `NO`. In this case, we want a sentence to print either way, so we have an `if` clause and an `else` clause.

Based on what we know of the two arrays referenced here, which `NSLog` statement will execute at runtime?

**Answer:** The two arrays are not equal, so the instructions inside the `if` clause are skipped and the instructions inside `else` are run instead.
{: .notice--success}

## Switch it Up

Believe it or not, most control flow issues can actually be resolved with if-else blocks. You might need to chain several of them together, but you can create a pretty complicated code path using just `if`'s. The problem is many nested `if`'s can be very difficult to follow and debug. Because of that, what follows is a set of control flow concepts that allow for both more complicated switching as well as looping.

If you have several different paths of code, but only want to execute one of them at runtime, use a `switch` block. See below for an example:

{% highlight objc linenos %}
int a = 3;
switch (a) {
case 1:
    NSLog(@"Got 1");
    break;
case 2:
    NSLog(@"Got 2");
    break;
case 3:
    NSLog(@"Got 3");
    break;
default:
    NSLog(@"Got Default");
    break;
}
{% endhighlight %}

As you can see above, the `switch` block is bounded by a set of `{}` braces. The value that you're "switching" from is listed in `()` parentheses after the reserved keyword `switch`. Possible outcomes are listed inside the braces as `case` statement blocks. The default case is a catch-all to cover possible values that haven’t been enumerated by specific cases. Two concerns to keep in mind when using `switch`: you must provide a `break` instruction at the end of each case, or execution will *fall through* to the next set of lines until a `break` is found; and the value switched on must be an integer type.

Bottom line: many different cases can be listed (and each case can have multiple lines of instructions), but at runtime, since the value of the switched variable (in this example, `a`) will only be satisfied by one case, only one instruction block will run. All other cases will be skipped.

## Looping

We've been talking about deciding which block of code to run based on certain runtime conditional values, but what if we wanted to run a block of code repeatedly a certain number of times? Or run until certain conditions are met? For that we have looping. Let's take a look at `for` loops first.

### `for`

These loops allow you to run a set of instructions a certain finite number of times. The most common way to bound a `for` loop is to provide it with a collection of data and have it iterate over each element in that collection. Thus if the collection has 10 items, the `for` loop will run its block 10 times.

{% highlight objc linenos %}
NSArray *names = @[@"Han Solo", @"Kathryn Janeway", @"James Kirk", @"Jean-Luc Piard", @"Malcolm Reynolds"];

for (NSString *name in names) {
    NSLog(@"Hello Captain %@", [name componentsSeparatedByString:@" "][1]);
}
{% endhighlight %}

The above loop looks at each name in turn from the names array and prints out the message "Hello Captain " and places the captain's last name at the end of the string. See the [this Apple Documentation page](https://developer.apple.com/documentation/foundation/nsstring/1413214-componentsseparatedbystring?language=objc) if you want to learn more about the `componentsSeparatedByString:` method.

### `while`

`while` loops are used to perform an instruction or set of instructions until some condition is met. The loop runs an undefined number of times rather than a predefined number like with a `for` loop. Let's say we wanted to count to 100 by a factor of 5 each time. We could arrange a loop such that it will end after we reach 100. See below for an example:

{% highlight objc linenos %}
int n = 0;
while (n <= 100) {
    n += 5;
    NSLog(@"%d", n);
}
{% endhighlight %}

The above code block will print each number between 5 and 100, jumping by 5 each time. The block is evaluated as follows: line 2 evaluates to `YES` or `NO` depending on the value of `n`. As long as the value is less than 100, lines 3 and 4 are evaluated. The value of `n` is increased by 5 and then printed. Once line 5 is reached, the loop begins again, as long as `n` is still less than 100. At some point the value of `n` will be increased beyond 100, and when the loop begins again at that point, the conditional on line 2 will evaluate to `NO`, thus ending the loop.

### `do-while`

If you’d like to ensure the instructions inside your loop run *at-least* once, you can use an alternate form of the `while` loop, something called `do-while`. See the example below:

{% highlight objc linenos %}
int m = 0;
do {
    m+=5
} while (m <= 100);
{% endhighlight %}

The above block is essentially the same as the previous `while` example, with one important difference. The instruction at line 3 always executes at-least once, and then again in a loop if the expression at line 4 is `YES`. `while`  loops can run 0 or more times, whereas `do-while` loops can run 1 or more times. Deciding which to use in your code is a decision that will be based on the specific problem you’re trying to solve.

## Recap
Let's take a look at the concepts covered in this lesson:

* Controlling the flow of code with `if-else` and `switch` blocks
* Running the same block of code multiple times with loops: `for` , `while`, and `do-while`