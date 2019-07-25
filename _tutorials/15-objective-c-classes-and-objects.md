---
title: "Classes and Objects"
permalink: /tutorials/objective-c-classes-and-objects/
excerpt: |
    This lesson dives into the principles of classes and objects, concepts fundamental to object-oriented programming. You'll learn how to build your own classes in Objective-C and then use those classes in other parts of your code as objects.
---

## Classes

*Functions* are great. They allow the programmer to group instructions into logical units and perform them as a whole by simply calling the function by its name. Functions can't store information longterm however. Each time they run, the information contained within is only stored in memory for the life of the function. Once it finishes (i.e. *returns*), the data it stored is gone. To understand classes and how they fit into the *object-oriented* paradigm, imagine a real-world item you'd like to model with code. Let's use a car as an example. To properly model it, you'd need the computer to store things about the car that define what the car *is*, as well as what the car *does*. The object-oriented paradigm defines these things as *properties* and *methods*. A property is something *about* the object. The fuel level of a car could be a property. The action of driving could be a method (another name for a function).

A *class* is a grouping of code used to create *objects*. You can think of the class as the blueprint, and the object as the thing made using the blueprint as a guide. Using our above example, a specific car is the object, say a blue Honda Civic. The blueprint we use to build that car is a class named `Car`. Let's look at a code example:

{% highlight objc linenos %}
@interface Car : NSObject

@property (strong, nonatomic) NSString *make;
@property (strong, nonatomic) NSString *model;
@property (strong, nonatomic) NSString *color;

@end

@implementation Car

- (instancetype)initWithMake:(NSString *)make model:(NSString *)model andColor:(NSString *)color
{
    self = [super init];
    if (self) {
        self.make = make;
        self.model = model;
        self.color = color;
    }
    return self;
}

@end
{% endhighlight %}

Lines 1-22 define a class called `Car`. This is a blueprint for creating `Car` objects. The `Car` class has three properties, `make`, `model`, and `color`. These are defined in something called the *interface*. Typically, this block of code (lines 1-7) would be placed in its own file, called a *header*. Lines 9-21 would go in a separate but related file, called the *implementation*. The pair would have the same name, that of the class, and have file extensions `.h` and `.m`, respectively. So for this class, you’d create two files: `Car.h` and `Car.m`.

{% highlight objc linenos %}
#import "Car.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Car *hondaCivic = [[Car alloc] initWithMake:@"Honda" model:"Civic" color:"black"];
    }
    return 0;
}
{% endhighlight %}

To create an *instance* of the `Car` class (another way to describe an object), you must provide all three attributes, which we do when we *instantiate* a `Car` object at line 4. The computer knows how to set the various arguments to the appropriate properties of the object created because of the `init` function at lines 11-21 in the class. This function maps the arguments that are *passed into* it to the *instance properties* of the object. Again, understanding every bit of the above example isn't necessary right now. The important concept here is that real-world systems can be modeled in code with classes to define what it *is* and what it *does*. The compiler can then use this blueprint to create *instances* of the class (objects) using specific data (line 4).

### Drive My Car

The above class allows us to create as many `Car` objects as we'd like, but once created, they can't *do* anything. That's because we haven't defined any methods. To get this car moving, we'll add a `drive` function.

Class and instantiation example fully rewritten below for clarity.
{: .notice--info}

{% highlight objc linenos %}
@interface Car : NSObject {
    double fuelLevel;
    int odometer;
}

@property (strong, nonatomic) NSString *make;
@property (strong, nonatomic) NSString *model;
@property (strong, nonatomic) NSString *color;

@end

@implementation Car

- (instancetype)initWithMake:(NSString *)make model:(NSString *)model andColor:(NSString *)color {
    self = [super init];
    if (self) {
        self.make = make;
        self.model = model;
        self.color = color;
        fuelLevel = 1.0;
        odometer = 0;
    }
    return self;
}

- (BOOL)drive {
    BOOL hasDriven = NO;
    if (fuelLevel >= 0.2) {
        fuelLevel -= 0.2;
        odometer += 5;
        hasDriven = YES;
    }

    return hasDriven;
}

@end
{% endhighlight %}

{% highlight objc linenos %}`
#import "Car.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Car *hondaCivic = [[Car alloc] initWithMake:@"Honda" model:"Civic" color:"black"];
        [hondaCivic drive];
    }
    return 0;
}
{% endhighlight %}

The code above is the same as the previous example, with the following additions:

* `fuelLevel`: a `Double` *instance variable* with an initial value of `1.0`
* `odometer`: an `Int` instance variable with an initial value of `0`
* `drive` method to allow the car to move

As you can see in the `drive` method, the car can only move if it has at least 20% of its gas left, and if so, uses a 20% portion of gas and adds 5 miles to the odometer. This is not a very fuel efficient vehicle, but it'll do for our purposes. The cool thing about custom classes and methods is you can model your real-world object however you'd like. In this case, we only care about fuel usage and miles driven, so we can model those aspects and leave out the other attributes that make up a real life car. We'll be using classes and objects throughout, so don't worry if these concepts are still a little foggy.

Again, this example isn’t meant to be fully understood yet, as there are lots of concepts we’re just touching on here to provide an introduction. Additional understanding will come with future lessons.

## Recap
Let's take a look at the concepts covered in this lesson:

* Using objects to model real-world elements in code
* Instructing the computer on how to build objects using its class as a blueprint