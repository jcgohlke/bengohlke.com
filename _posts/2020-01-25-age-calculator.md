---
title: "App Review: An Age Calculator In Three Iterations"
toc: true
toc_label: In this guide
toc_sticky: true
categories:
  - tech_education
  - app_review
tags: 
  - swift
  - swiftui
  - objective-c
  - uikit
last_modified_at: 2020-01-25T09:00:00-05:00
---

All Swift code presented here is compatible with Swift 5.1.
{: .notice--info}

## And Now, For Something (Kinda) Different


The world of iOS development continues to shift and there are more and more paths to building an app. Objective-C, Swift, UIKit, SwiftUI, Combine -- there are so many combinations of choices, but which should you choose? This blog series hopes to showcase each by looking at one simple app idea, and then building it in three iterations. Comparing and contrasting each approach along the way.

## But, Why?

<iframe src="https://giphy.com/embed/1M9fmo1WAFVK0" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

As the options for developing apps on Apple platforms gets more diverse, it's important to understand when and how to use each different set of tooling for the job at-hand.

At this point, an app can be built using Objective-C and UIKit, Swift and UIKit, or Swift and SwiftUI. Especially when starting out with programming, it can be difficult to compare approaches. Most of the time you have to compare codebase A in one approach with codebase B in another. What if you could see the exact same app implemented three different ways, and playing to the strengths of each language/framework?

That's what this new series, _App Review_, is all about.

Let's dive into our first app, called Age Calculator, to see what each approach has to offer.

**Get the Code:** Download a [zip file](https://github.com/jcgohlke/app_review-age_calculator/archive/master.zip), or visit the [GitHub repo](https://github.com/jcgohlke/app_review-age_calculator) to clone a copy. Folders in the repo labeled with either "-ObjC", "-Swift", or "-SwiftUI" contain each separate Xcode project.
{: .notice--info}

## What are We Building?

Here's the problem description:

>Create an application that presents the user with an interface to choose their birthdate. Based on that info, calculate and display both their age and the date of their next birthday.

With that in mind, let's imagine a UI that would satisfy these requirements. We'll need:

* A Date Picker to present a wheel of dates to choose a birthdate
* Labels to show today's date, the current age of the user, and one to show the date of the user's next birthday

We'll use this design for all three iterations. That way we can focus the rest of the exploration on the differences and similarities between the code for each.

The UI will look just like this in each iteration:

<figure class="half">
    <img src="/assets/images/age-calculator-ui.png" alt="iOS screenshot of app design" style="border: 12px solid #a0a0a0">
</figure>

## Objective-C and UIKit

There are many things unique to an Objective-C based iOS project, but I'd like to highlight just a couple here. These are things that clearly differentiate the implementation from the Swift and SwiftUI versions.

### File Structure

To start off, Objective-C classes are composed of two files, one ending in `.h` and the other ending in `.m`. The first is called the class' _header_ file, and the other is the known as the _implementation_ file. The header contains only declarations of public properties and methods, and the implementation file contains the algorithms that make up the business logic of the class.

The `.m` file can contain its own interface section, and this is a place to declare properties and methods private to the class. Here's an example of this from the Age Calculator app:

{% highlight objc linenos %}
@interface AgeViewController ()

@property (nonatomic, weak) IBOutlet UIDatePicker *birthdatePicker;
@property (nonatomic, weak) IBOutlet UILabel *currentDateLabel;
@property (nonatomic, weak) IBOutlet UILabel *ageLabel;
@property (nonatomic, weak) IBOutlet UILabel *nextBirthdayLabel;

@property (nonatomic, strong) NSDateFormatter *dateFormatter;

- (IBAction)birthdateSelected:(UIDatePicker *)sender;

@end
{% endhighlight %}

If you're familiar with Swift properties, you can see these are similar, but do look a little different. A big difference is the way types are listed. In Objective-C, the type is listed _before_ the variable name. You also use `@property` to tell the compiler this variable to synthesize getters and setters for this value.

### Object Initialization

Another big difference between Swift and Objective-C is in how objects are initialized:

{% highlight objc %}
self.dateFormatter = [[NSDateFormatter alloc] init];
{% endhighlight %}

Objective-C requires a two-step process for creating objects. First, memory is allocated for the object with the `alloc` call, and then the result of that is fed into the call to `init` the object. The final object created is stored in the date formatter property. Also notice that when referencing properties in Objective-C, you must reference `self` first. Compare the above with the equivalent call in Swift:

{% highlight swift %}
dateFormatter = DateFormatter()
{% endhighlight %}

There are many more differences between the languages, but if your frame of reference is only using Swift, use these as a place to begin your study of Objective-C.

## Swift and UIKit

### Enums

A strong difference between Swift and Objective-C is in how they handle enumerations. An `enum` in Objective-C is limited to being represented by an integer type. So all enum cases are ultimately integers. This is not true in Swift however. The `enum` type was promoted to be a first-class member of the language, alongside `class` and `struct`. So the raw type of a Swift `enum` can be anything. This provides a lot more flexibility when using enums in Swift.

In addition, cases look different in Swift than they do in Objective-C. Take the configuration of the date formatter for example. Here's what it looks like in Swift:

{% highlight swift linenos %}
dateFormatter.dateStyle = .medium    
dateFormatter.timeZone = .none
{% endhighlight %}

`.medium` and `.none` are both examples of enum cases. Due to Swift's type inference feature, the enum's name can be omitted. This makes it very easy to use in context when writing code. Simply type the `.` and let Xcode suggest the valid enum cases to assign to this property. Compare the above line with the equivalent Objective-C code:

{% highlight objc linenos %}
self.dateFormatter.dateStyle = NSDateFormatterMediumStyle;
self.dateFormatter.timeStyle = NSDateFormatterNoStyle;
{% endhighlight %}

The equivalent enum case names are much longer, and require at least some knowledge of the naming convention to guess at them when writing code. My advice for this is if you're writing Objective-C code, use the Xcode documentation to look up the enum you'll be using and switch the doc window to display Objective-C syntax.

### Member Accessibility

Another interesting difference with Swift compared to Objective-C is in the declaration of methods. In Swift, an object's properties and methods are by default considered to be public to the module that contains them. This usually means that properties and methods are public to the entire iOS project. If a property or method should be restricted to only internal uses within the type, Swift requires you to add a `private` keyword to the property or method declaration. Let's look at an example from the view controller in the Age Calculator project:

{% highlight swift %}
private func updateViews() {
    //...
}
{% endhighlight %}

The above method may only be called from within the view controller it's defined in. This is handy for both methods and properties that you don't want to expose to other objects. Maybe this method modifies internal state for this type and therefore should be handled as an internal matter.

In Objective-C, the public/private access of a property or method is handled by the presence or absence of its declaration in the header file (see [_File Structure_](#file-structure) above).

## SwiftUI

You might think that because SwiftUI uses the Swift language, the learning curve would be lower than switching from say Objective-C to Swift. You'd be right in some ways, but the additional wrinkle that SwiftUI introduces is a shift in how you think about structuring your code.

### A Different Approach

Swift and UIKit or Objective-C and UIKit are written in what's called an [_imperative_ style](https://en.wikipedia.org/wiki/Imperative_programming). This means that changes made by the code you write affect the _state_ of the application. Conversely, SwiftUI adopts a [_declarative_ style](https://en.wikipedia.org/wiki/Declarative_programming) of programming. The difference represents a fairly fundamental shift in how you think about an app's structure.

With a declarative codebase, you _describe_ the way you want the UI to look, and how you want it to react to input or stimulus, but you don't control the flow of how that code executes.

It can be difficult to describe in the abstract, so let's look at an example from the Age Calculator SwiftUI project and see how it compares to its Swift and Objective-C counterparts:

{% highlight swift linenos %}
@State var birthdate: Date = {
    var dateComponents = DateComponents()
    dateComponents.year = 1990
    dateComponents.month = 6
    dateComponents.day = 1
    return Calendar.current.date(from: dateComponents) ?? Date()
  }()

//...

DatePicker("", selection: $birthdate, displayedComponents: .date)
{% endhighlight %}

The above code was lifted from the `ContentView.swift` file. The `body` property, where the `DatePicker` line came from, represents a big departure from how we've all become used to architecting a view on iOS. The `body` property contains all the subviews that make up the UI of the app's main screen, but rather than being initialized on load and then updated individually when a state change occurs, SwiftUI recomputes and redraws the _entire_ `body` whenever a UI element needs to be updated. This is described like so: "The UI is a function of its state". State changes to models automatically trigger UI updates. This shortens the feedback loop between models and views. It also effectively removes the need for _view controllers_ as a part of the app's architecture. The view is now able to interact directly with its associated models.

### Property Wrappers

Notice how `birthdate` is marked with a `@State` keyword at the beginning of its declaration. This is called a `propery wrapper` and indicates that the property will be used to manage some piece of state. In this case, it represents the user's birthdate. An initial value is computed and the date is used to configure the `DatePicker` view onscreen.

The `$` in front of `birthdate` at line 11 works in conjunction with the `@State` property wrapper. What's really cool about this also speaks to the declarative nature of SwiftUI. The date forms a two-way relationship with the picker. If the date is changed, the picker will update to display that date in its UI. Conversely, if the user changes the displayed date in the picker, the `birthdate` property will automatically update to match. With this declarative style, you're not creating UI and then instructing the app with code exactly what to do in all possible outcomes. Instead you're _describing_ what the UI looks like, and how it connects to model data, but you leave the implementation details of those connections to SwiftUI itself.

### A Freeing Concept

The above approach might seem foreign and burdensome at first, but once you start to realign how you think about composing your code into a declarative style, it really frees you to work through problems more efficiently.

If you open the Age Calculator SwiftUI project and check out the `ContentView.swift` file, you'll notice quite a bit of the _work_ of this app is done in computed properties that just _automatically_ figure out the age and next birthday of the user once they configure their birthdate with the picker. By comparison, the _work_ of this app needs to be more explicitly orchestrated in the Objective-C and Swift versions.

## In Summary

You've made it to the end! Congrats! There is a lot more to explore with these three styles of building iOS apps, and I'll be tackling those in future articles.

I encourage you to check out the full Xcode projects for each of these iterations and compare and contrast the approaches. The goal with this series is to let you see one app built three different ways so the code blocks can be directly compared.

Let me know what you think, and send me suggestions for other app ideas to build on Twitter [@ferrousguy](https://twitter.com/ferrousguy).

**Get the Code:** Download a [zip file](https://github.com/jcgohlke/app_review-age_calculator/archive/master.zip), or visit the [GitHub repo](https://github.com/jcgohlke/app_review-age_calculator) to clone a copy. Folders in the repo labeled with either "-ObjC", "-Swift", or "-SwiftUI" contain each separate Xcode project.
{: .notice--info}