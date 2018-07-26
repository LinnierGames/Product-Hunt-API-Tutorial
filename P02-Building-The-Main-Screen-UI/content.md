---

title: "Building the Main Screen UI"
slug: main-screen-UI

---

In order to avoid context switching and to reduce development time, we'll be building this app **outside-in**—starting with the UI and layout of the app and then ending with the networking layer.

We will use the Storyboard in order to create the UI with very simple constraints. Try to resist the temptation to make complex or good looking UI this early, it'll only slow you down. You'll get an opportunity later on to polish and upgrade the app to your liking.

**Your first goal should be to get a working prototype**. The client wants the feed screen working first, so let's start with that.

# Creating the Main Screen

The `FeedView` consists of a table view with custom cells that display information about products retreived from Product Hunt.

The UI itself is simple, with the magic happening in code. For now let's just build the UI and connect it to a Swift file.

Here's what the result will look like:

![Preview FeedView](assets/feedview-preview.png)

This view is intentionally simple to make it quick to make.

We'll be using a navigation controller for this app.
>[action]
>Open Main.storyboard and click on the view controller.

1. A shortcut for adding a navigation controller in storyboard is to **embed** a viewcontroller in one.
    >[action]
    >On the menu bar select `Editor`, then `Embed in` and finally `Navigation Controller` to quickly wrap the view controller in a navigation controller.
    >![Embedded controller](assets/embed-controller.png)
1. >[action]
   >Set the view controller's title to `Feed`.
   >![Set title](assets/set-controller-title.png)
1. A perk of keeping things simple in the beginning is that you don't have to spend a lot of time configuring the UI.
   >[action]
   >Add a table view to the view controller and pin it to all 4 sides.
    >![Add table view](assets/pin-table-view.png)

## Prepare the Swift file

1. To adhere to good coding conventions, rename the view controller to `FeedViewController` to better reflect what its used for.
    >[action]
    >Change the name in the file inspector, for the class, and in the comments at the top of your file.
    > ![Rename view controller](assets/rename-viewcontroller.png)
2. We don't have any data yet, but let's add the functions we be using to update cells.
    >[action]
    >Add an **extension** for UITableViewDataSource at the bottom of `FeedViewController` to seperate code needed to fill the table view with data.
    >``` swift
    > class FeedViewController: UIViewController {
    >   ...
    >}
    >
    >// MARK: UITableViewDataSource
    >extension FeedViewController: UITableViewDataSource {
    >    /// Determines how many cells will be shown on the table view.
    >    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    >        return 1
    >    }
    >
    >    /// Creates and configures each cell.
    >    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    >        return UITableViewCell()
    >    }
    >}
    >```

    >[info]
    >Extensions add new functionality to an existing class, structure, enumeration, or protocol type.
    >This includes the ability to extend types for which you do not have access to the original source code (known as retroactive modeling).
    >Extensions are similar to categories in Objective-C. (Unlike Objective-C categories, Swift extensions do not have names.)

3. >[action]
    >Add an **extension** for `UITableViewDelegate` right below that.
    >``` swift
    >...
    >
    >// MARK: UITableViewDelegate
    >extension FeedViewController: UITableViewDelegate {
    >   // Code to handle cell events goes here...
    >}
    >```
    >We'll use this later to setup selecting and deleting cells.

4. >[action]
    >Connect the table view to `FeedViewController`.
    >![Connect outlet](assets/connect-outlet.png)

5. Because we **subclassed** `UITableViewDataSource` and `UITableViewDelegate` in **extentions** for the `FeedViewController`, we can set the `feedTableView`'s data source to be the `FeedViewController`.
    >[action]
    >Set the delegate and datasource for the table view by typing the following code in your `viewDidLoad`:
    >```swift
    >feedTableView.dataSource = self
    >feedTableView.delegate = self
    >```

The table view is fully connected and working. You can run the app to see the one cell created thanks to the lines of code we put in the UITableViewDataSource.

# Create Post Model

We will model the information we will be getting for each posting. The properties we need are the product's id, name, tagline, number of votes, and number of comments.

1. >[action]
    >Create a new Swift file and call it `Post.swift`.
2. We'll use a Struct to define the variables we want to get from the Product Hunt API.
    >[action]
    >Create a Struct called `Post`.
    >```
    >/// A product retreived from the Product Hunt API.
    >struct Post {
    >
    >}
    >```s
3. >[action]
    >Give `Post` variables to hold a product's id, name, tagline, number of votes, number of comments, and screenshot url.
    >```swift
    >struct Post {
    >    let id: Int
    >    let name: String
    >    let tagline: String
    >    let votesCount: Int
    >    let commentsCount: Int
    >    let previewImageUrl: URL
    >}
    >```

There's more we will need to add to this file to allow it to work effortlessly with network calls. We'll leave it like this for now and move on to create the UI that will show this data.