---
title: "Understanding Data Flow in SwiftUI"
toc: true
toc_label: In this guide
toc_sticky: true
categories:
  - tech_education
tags: 
  - swiftui
  - property_wrappers
last_modified_at: 2020-02-09T09:00:00-05:00
---

All code presented here is compatible with Swift 5.1.
{: .notice--info}

## What is SwiftUI?

Introduced by Apple in the summer of 2019, [SwiftUI](https://developer.apple.com/tutorials/SwiftUI) represents a whole new way to design app interfaces.

You might be thinking, but I just got the hang of UIKit! Don't worry though. While SwiftUI might take a different approach, the knowledge you have from UIKit is still very applicable.

One of the biggest differences with SwiftUI is in how data flows through your app. In this tutorial, you'll get familiar with how property wrappers pass data, all while building an app to track the gifts you buy for friends and family, called Gifter.

While building Gifter, you'll learn:

* How to use the new property wrappers `@State` and `@Binding`
* How those differ from a third property wrapper called `@ObservedObject`
* The basics of building interfaces with SwiftUI's view objects

## Getting Started

First, download the starter and completed projects. Then open the project in Xcode.

[Download Xcode Project](){: .btn .btn--primary}

Xcode 11 or later is required and macOS 10.15 Catalina is recommended.
{: .notice--warning}

Build and run the starter project. It will show a navigation bar with a title and a simple list with a placeholder gift cell. You'll start by learning how to add new gifts to the app with a modal view.

<figure class="half">
    <img src="/assets/images/Gifter-1-starter-ui.png" alt="iOS screenshot of starter app UI" style="border: 12px solid #a0a0a0">
</figure>

{% capture notice-text %}
Wouldn't it be great to see a preview of your UI without running the app each time?

* Find and click the button in the upper right corner of your Xcode editor window with the tooltip *Adjust Editor Options*.
* Click the option labeled **Canvas** (if checked, the preview window will already be visible).
* Notice the keyboard shortcut of ⌥⌘⮐ (option-command-return).
* Hiding the preview is ⌘⮐ (command-return).
{% endcapture %}

<div class="notice--info">
  {{ notice-text | markdownify }}
</div>

## Introducing Property Wrappers

You may have noticed that the `GiftListView` is modeled as a `struct` and it contains a computed property called `body`. The body of a SwiftUI view creates all the UI seen within that view onscreen.

Properties modeled inside structs are immutable, since you might declare the struct immutable when initialized. Because SwiftUI views are structs, changes to any or all parts of the view require a redrawing of the entire view. Most views will likely present data to the user, and that data needs a place to live within the view struct.

It's natural to think that things will change as the user interacts with the view (or some other event occurs to change it, like data coming in from the web, or a sensor in the device reporting new values). If you apply a property wrapper called `@State` to your properties, changes to those values will trigger an automatic redraw of your UI.

Let's say you wanted a button to increase a count value each time the user taps it, maybe to count entrances by everyone's favorite crazy neighbor, Kramer on the show Seinfeld. In SwiftUI, you could express this with a view struct like so:

```swift
struct KramerView: View {
	@State var kramerCount: Int
	
	var body: some View {
		Button("Kramer Entrances: \(kramerCount)") {
			self.kramerCount += 1
		}
	}
}
```

Once tapped, the button's closure runs. This increases the `kramerCount` property. Because that property was declared with `@State`, SwiftUI will see the change and realize the body needs to be recomputed, since the button title references that same property.

The feedback loop here is tight and automatic. The button changes the count, and the button's title updates automatically with that new count when the view is redrawn. No need to manually change the title of the button yourself.

Serenity Now!

This represents one of the most powerful ideas within SwiftUI. The code is *declarative* rather than *imperative*. Meaning, you set up *what* you want the app to do, rather than *how* you want it to do it.

## Using `@State` to Drive UI Updates

In order to have some gifts to show in the list, you'll need to first provide a way to enter new gifts. A modal view will do nicely here, but how do you present one? Because SwiftUI uses a declarative syntax, you'll need a way to track whether the modal view is showing within `GiftListView`. You'll add your first `@State` property for this.

Add the following to `GiftListView`, under the `Properties` comment:

```swift
@State var showingAddGift = false
```

Then, add the following just after the closing brace of the `NavigationView`:

```swift
.sheet(isPresented: $showingAddGift) {
  NewGiftView()
}
```

You send two arguments to `sheet(isPresented:)`, a *two-way binding* and a closure which presents a new view. The view needs to watch `showingAddGift` for changes to trigger the sheet view, so the `$` ensures that. Because SwiftUI is declarative, instead of providing an exact set of steps to show the modal sheet, you just set the boolean value to `true`, and the sheet will show automatically. This is a big shift from the way you may have written code in the past, but with practice, it can be remarkably powerful.

Now that the app can display a modal sheet, you just need to provide a way to make `showingAddGift` true when the user wishes to add a gift.

Many apps show a button in the navigation bar for adding new records. The starter project already shows a navigation bar simply by wrapping `List` in a `NavigationView`. To add a button to the bar, simply add the following code just after the closing brace of the `List`:

```swift
// 1
.navigationBarItems(
  trailing: Button(action: {
    // 2
    self.showingAddGift = true
  }) {
    // 3
    Image(systemName: "plus.square")
      .font(.system(size: 22))
  }
)
```

1. Modifiers of `NavigationView` should modify its children, so in this case `List`. `navigationBarItems` has a couple different arguments, but here you're using the `trailing` argument to put the button on the right side of the bar.
2. The action doesn't directly cause the new gift modal view to show. Setting the boolean to `true` will cause the sheet modifier from the previous step to show the view.
3. The value passed into the closure can be any kind of `View`. You're passing in a `Button` object, which itself takes two arguments: an action and a view. The view is composed of an image, and here you're using an image from Apple's collection of icons called SF Symbols.

Build and run the app. Tap the plus button in the upper right corner of the screen and it will present a modal sheet with the text *Add a new gift* right in the middle.

[#4 Insert screenshot of app with modal view]

## Collecting Data from the User

To have gifts to show, you'll need the modal view to prompt the user for information about each new gift. Find the **NewGiftView.swift** file in the navigator of Xcode and click on it to show the source code.

Add the following properties above the body variable:

```swift
// 1
@State private var recipient = ""
// 2
@State private var description = ""
// 3
@State private var price = ""
// 4
@State private var purchased = false
```

1. The person the gift is for
2. The description or name of the gift
3. The gift's purchase price
4. A boolean to store whether the user has purchased the gift yet

All the above hold information you'll collect from the user when they create a new gift. The next step is to create places in the UI for the user to enter it.

The file already has a `NavigationView` added, so replace `Text("Add a gift")` with the following code:

```swift
// 1
Form {
  // 2
  TextField("Recipient's Name", text: $recipient)
  TextField("Description", text: $description)
  TextField("Price", text: $price)
  // 3
  Toggle(isOn: $purchased) { Text("Purchased") }
}
// 4
.navigationBarTitle("New Gift", displayMode: .inline)
```

1. A `Form` view makes it super easy to create a table-based form for collecting data.
2. A `TextField` needs two things: a title to use for placeholder text, and a `@State` variable to store the text entered by the user. The `$` used here is called a *two-way binding*. So, the string held in `recipient` shows in the text field, and text entered by the user is written back to `recipient`.
3. A `Toggle`: in this case it appears as a boolean switch. The `purchased` variable will track the state of the toggle, and the `Text` view shows to the left of the toggle in the form.
4. The navigation bar title helps the user identify the purpose of this screen.

Build and run the app. When you tap `+`, it will now show a view with a table and the fields you've specified in the above code. The text fields are tappable, and you can enter text. It's pretty neat how easily forms can be built with SwiftUI.

[#5 Insert screenshot of form]

This is great, but there is no obvious way to close this modal view. Since iOS 13 the user can swipe down, but it's a new gesture, and it might not be obvious to everyone. It would be nice to have a button. But remember, this view showed because of a boolean value stored within the previous view. To reference that value in this screen, you'll need a new kind of property wrapper, called a `@Binding`.

## Minimizing Sources of Truth

Any given piece of data in your app can sometimes appear in multiple places. These are called sources of truth. To prevent bugs caused by these sources getting out of sync with each other, it's important to minimize them when possible.

The state of the new gift view showing or not is stored in the `showingAddGift` variable inside the `GiftListView` object. To dismiss the view, that boolean value needs to be set to `false`. Remember, views in SwiftUI are considered *a function of the app's state*. To access the boolean, you'll need a new property inside `NewGiftView`.

Add the following property to `NewGiftView`:

```swift
@Binding var isPresented: Bool
```

There are two main differences between this property and the others you've added:

1. `@Binding` works similarly to `@State`, but it doesn't actually store the value itself. It's just references back to the original. Any changes to this property will be seen by both the binding and the original property.
2. Since this property connects to another that actually holds the value, you don't need to specify an initial value.

The presentation status of the modal view is now accessible inside `NewGiftView`.

You'll need to modify the preview code at the bottom of `NewGiftView` because of this property addition. Since it has no initial value, the preview provider needs a value for the property to render the preview. Modify the line inside the `previews` computed property as follows:

```swift
NewGiftView(isPresented: .constant(true))
```

Since `isPresented` is a binding, the value passed must be a binding. The syntax you see above allows you to create a constant value *as a binding*. The value isn't mutable, but you just need to pass something here to render the preview in the canvas, so a constant is fine.

Next, create a button that sets this boolean to `false`. Place the following code just after the `Toggle` view inside the form:

```swift
Button(action: {
  // 1
  self.isPresented = false
  }) {
  // 2
  HStack {
    Image(systemName: "plus.square")
    Text("Add Gift")
  }
}
// 3
.disabled(recipient.count == 0 || description.count == 0 || price.count == 0)
```

1. The button only needs to set the boolean to `false`, and the sheet will see the change through the `@Binding` and automatically dismiss this view.
2. By creating an `HStack` here, you provide both an icon and a small description to indicate what this button will do.
3. This modifier will check the state of each conditional passed in. If any are 'false', the button will disable itself. This ensures the user can't add a new gift until they've entered values into each field.

You created an error in `GiftListView` because of these changes. Open **GiftListView.swift** and change the call to `NewGiftView()` to include the following argument:

```swift
NewGiftView(isPresented: self.$showingAddGift)
```

You need to use `self` because the call is inside a closure. Here, `isPresented` passes the `@State` variable as a binding.

Build and run to test your new button. Once the new gift view is visible, the button won't turn on until you enter text into all three fields. The user can't make new gift objects without entering all the required values. Once enabled, tap the *add gift* button to dismiss the modal view. You just configured your first SwiftUI binding!

[#6 Insert screenshot of new gift view with data entered]

## Modeling More Complex Data

Now that the user can add new gifts, you need a way to pass it back so it shows up in the list. The `Gift` model already exists in the starter project, and it's a standard Swift model with just a few properties. The only new concept here is the `Identifiable` protocol, which requires a property declared as `id` with a value that is unique.

These gift objects need a place to live in the app. To create a place, add a new Swift file to the project called **GiftList.swift**. Then enter the following in the new file:

```swift
class GiftList: ObservableObject {
  var items = [Gift]()
}
```

Unlike the other files in this project, you declare `GiftList` as a class. This is because it will persist the gift objects across objects and generally be longer-lived in the app.

Open **GiftListView.swift** and add a `GiftList` property:

```swift
@ObservedObject var giftList = GiftList()
```

You may have noticed when testing before, if you added a gift, the list view didn't show the gift just entered. That's because the app had nowhere to store the data. You'll remedy that bug right now with the new `giftList` you just created.

To fix the bug, `NewGiftView` needs to store new gift objects in this list. The problem is `GiftList` is a class rather than a struct, so `@State` won't work. SwiftUI has you covered though with a different property wrapper called `@ObservedObject`. It's useful for class instances where you want to control what properties to watch.

To set the observed properties within `GiftList`, open **GiftList.swift** and make the following changes:

```swift
// 1
class GiftList: ObservableObject {
  // 2
  @Published var items = [Gift]()
  
  // 3
  var purchasedItems: [Gift] {
    return items.filter { $0.isPurchased }
  }
}
```

1. To use `@ObservedObject`, the type needs to conform to the `ObservableObject` protocol.
2. The `@Published` property wrapper is used to indicate this property will *publish* its changes as part of the observed object.
3. A computed property for determining which items are purchased; you'll use this later in the tutorial.

This allows you to control which properties within a class instance to watch. Just add `@Published` to any property you want to observe externally.

Jump back to **GiftListView.swift** and set up the `List` view to show cells populated from gift objects.

Replace the contents of `List` with the following:

```swift
// 1
ForEach(giftList.items) { index in
	// 2
  GiftRowView(gift: self.$giftList.items[index])
}
```

1. A `ForEach` will iterate over a collection and create a view for each element.
2. The index of each item is used to look up the item in the `giftList`, which is passed into the new `GiftRowView`.

Line 2 above will throw an error because that view doesn't exist yet. Create a new SwiftUI view file called **GiftRowView.swift** and open it.

Edit `GiftRowView` to look like the following:

```swift
struct GiftRowView: View {
  // 1
  @Binding var gift: Gift
  
  var body: some View {
    HStack(spacing: 12) {
      // 2
      Button(action: {
        self.gift.isPurchased.toggle()
      }) {
        Image(systemName: gift.isPurchased ? "gift.fill" : "gift")
          .foregroundColor(gift.isPurchased ? .green : .black)
          .font(.system(size: 28, weight: .regular))
      }
      
      // 3
      VStack(alignment: .leading, spacing: 4) {
        Text(gift.description)
          .font(.title)
        Text(gift.recipient)
          .font(.headline)
      }
      
      // 4
      Spacer()
      
      // 5
      Text("$\(gift.price, specifier: "%.2f")")
        .font(.headline)
    }
  }
}
```

This is a lot of code, so let's break it down:

1. The gift object for this row is passed in, and the user may modify it (by marking it as purchased for example, or deleting it), so it's marked with the `@Binding` property wrapper. This allows it to be modified, and those changes will propagate back to the `GiftList` where all the gifts live.
2. The button uses a gift icon, either filled in or just an outline, depending on whether the gift is purchased. Tapping the button toggles the `isPurchased` boolean.
3. This `VStack` (a vertical collection of views) presents the description and recipient's name.
4. A `Spacer` view pushes views apart, and uses up all the available space.
5. A `Text` view to show the gift's price and formats it to appear as dollars and cents.

You're probably eager to see what this looks like. To get a quick preview, go to the bottom of this file and modify the `previews` variable to look like this:

```swift
GiftRowView(gift: .constant(
      Gift(description: "PS4 Controller",
           recipient: "Doug",
           price: 38.99)))
```

The code within this variable tells Xcode how to create a preview in the canvas. Since it needs a `Gift` object to populate the row, you need to provide one. Enter a gift of your choice here. This is just dummy data for the Xcode preview. It won't show up when the app is run on a simulator or device.

Click the resume button in your canvas to see a preview of this view with the example gift you entered above. You could build and run, but currently there's no gifts to display, so the `List` view would be empty. The canvas preview is actually a better way to check out the cell design at this stage.

Don't worry about the content of your cell bumping into the edges of the view. When displayed inside a `List` view, the system will automatically add a little padding to inset your content. Another example of allowing the system to figure out the *how*.

[#7 Insert screenshot of cell view preview]

## Using `@ObservedObject`

You'll use the observed `giftList` to collect and store new gifts added in `NewGiftView`. Open **NewGiftView.swift** and add a property to the view struct:

```swift
@ObservedObject var giftList: GiftList
```

This lets you pass the gift list into this view, and add new gifts created here to the list of gifts tracked by the app.

Be sure to update the preview provider at the bottom of the file. It only needs an instance of `GiftList`, so you can pass a blank one just for the preview, like so:

```swift
NewGiftView(isPresented: .constant(true),
                giftList: GiftList())
```

Add the following to the `Button` action closure:

```swift
self.giftList.items.append(
            Gift(description: self.description,
                 recipient: self.recipient,
                 price: Double(self.price) ?? 0.0,
                 isPurchased: self.purchased))
```

This creates a `Gift` object from the values in the form, and then adds that gift to the `items` array.

Lastly, open **GiftListView.swift** and edit `sheet` to pass in the gift list property to `NewGiftView`:

```swift
NewGiftView(isPresented: self.$showingAddGift, giftList: self.giftList)
```

Build and run the app. It will now display gifts added in the new gift view.

[#8 Insert screenshot of app showing cell of gift]

## All Done!

[Download Xcode Project](){: .btn .btn--primary}

Congratulations on building Gifter and taking your first steps to understanding data flow in SwiftUI!

SwiftUI is very different if you're used to UIKit on iOS, but with practice, it will become just as familiar.

If you want to see apps built in both UIKit and SwiftUI, check out my App Review series.

Until next time!