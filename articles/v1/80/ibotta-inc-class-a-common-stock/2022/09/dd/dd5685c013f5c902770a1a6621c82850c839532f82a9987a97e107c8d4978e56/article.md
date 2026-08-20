---
schema_version: "1.0.0"
document_id: "dd5685c013f5c902770a1a6621c82850c839532f82a9987a97e107c8d4978e56"
company_key: "ibotta-inc-class-a-common-stock"
company: "Ibotta Inc."
source_id: "ibotta-inc-class-a-common-stock-rss-dcf741155171"
canonical_url: "https://medium.com/building-ibotta/using-diffable-data-source-to-revamp-earnings-in-the-ibotta-ios-app-85b30ddd74bc"
published_at: "2022-09-08T19:05:06+00:00"
first_seen_at: "2026-07-25T01:07:04.216753+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:a296ee8fe2df848f559a20b7a51c778a1e54c091f89484f83d13d7abd2a4c1dc"
---

# Using UICollectionView to revamp Earnings in the Ibotta iOS app

# Using UICollectionView to revamp Earnings in the Ibotta iOS app


[Brett Markowitz](https://medium.com/@brett.markowitz?source=post_page---byline--85b30ddd74bc---------------------------------------)


7 min read


·


Jul 6, 2022


--


[Our mission at Ibotta](https://home.ibotta.com/about/company/) has always been to *“Make every purchase rewarding.”*


One of the ways we do that is by constantly asking ourselves how we can improve the ways in which our Savers earn cash back.


This desire to never settle extends across the company, including our mobile apps, where we recently took the opportunity to rethink the earnings detail screen.


This screen is incredibly important for Savers because it allows them to see the cash back they’ve earned from a specific transaction.


Press enter or click to view image in full size


The outgoing earnings detail design, in light & dark mode


At a glance, it looks sort of like a receipt. The store and purchase date are at the top, followed by the offers we matched and any relevant bonuses.


Savers can also provide feedback on the transaction, view images of the receipt they submitted, and get help if needed.


With all of the information we’re displaying here, it’s super important that this screen is as organized as possible. We want to make sure that Savers can easily see the cash back they’ve earned and be excited about it as well.


As a result, the Redemption Experience squad set out to completely redesign it with clarity, utility, and a little bit of playfulness in mind.


Our awesome designers came to us in April to show it off, and by mid-June, we released the new experience to all of our Savers.


Press enter or click to view image in full size


The redesigned earnings detail screen, in light & dark mode


Because this screen was set to receive more than just a fresh coat of paint, it was the perfect time to rethink the underlying techniques used to bring all of this information to life.


On the iOS side, we were already well into the process of replacing a majority of our list-based interfaces — built previously with a highly-custom component — with vanilla`[UICollectionView](https://developer.apple.com/documentation/uikit/uicollectionview)` s.


One notable aspect of the new design was that the components labeled ***Offer earnings*** and ***Extra earnings*** ** were designed to be expandable and collapsible.


At Ibotta, we prefer (and enjoy!) using Apple’s latest APIs whenever possible.`[UICollectionViewDiffableDataSource](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource)` includes support for these types of sections right out of the box.


And of course, like many layouts these days, this one would be simple and straightforward to achieve with`[UICollectionViewCompositionalLayout](https://developer.apple.com/documentation/uikit/uicollectionviewcompositionallayout)` .


Let’s take a look at how the view is pieced together.


The foundation of every view in the Ibotta iOS app is a` struct` we refer to as “view properties”.


The view properties contain the values the view will display.


For example, here are the view properties for the table “rows” in the redesign that contain the purchase date and cash back earned:


Within our architecture, the` Presenter` is the entity that creates and fills in the view properties using data from the network and other services.


It also maintains the state of the screen and can generate different view properties depending on that state.


The` Presenter` hands the properties it builds down to the view, at which point the view will use them to configure its relevant subviews.


Here’s how those same “rows”, which are just collection view cells, configure their subviews based on the view properties:


This is part of what makes our architecture so powerful.


In most cases, the view simply uses what it’s given to configure itself — it doesn’t need to be concerned with the business logic that it took to reach those final property values.


Let’s shift gears to the collection view.


One of the first things we do when creating a new collection view is to define the sections. We do this with an` enum` that conforms to`[CaseIterable](https://developer.apple.com/documentation/swift/caseiterable)` :


The`[CaseIterable](https://developer.apple.com/documentation/swift/caseiterable)` conformance gives us access to an` allCases` property for the` enum` which produces an array containing each case.


Then, when we generate our collection view’s compositional layout, we can have a nice, readable` switch` statement that creates a specific layout for each section.


The most interesting part of the redesign is the fact that the` offersMatched` and` extraEarnings` sections can expand and collapse to show and hide their contents.


Implementing this behavior requires that you use Apple’s`[UICollectionLayoutListConfiguration](https://developer.apple.com/documentation/uikit/uicollectionlayoutlistconfiguration)` API, which makes it easy to build a section with a`[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` -like appearance.


You can simply create a configuration with the desired appearance and provide it to the` list` method on`[NSCollectionLayoutSection](https://developer.apple.com/documentation/uikit/nscollectionlayoutsection)` . This is key, because without this, we won’t get the behavior that we want.


The other requirement for achieving expandable sections is to use`[UICollectionViewDiffableDataSource](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource)` , which includes support for them via`[NSDiffableDataSourceSectionSnapshot](https://developer.apple.com/documentation/uikit/nsdiffabledatasourcesectionsnapshot)`[s](https://developer.apple.com/documentation/uikit/nsdiffabledatasourcesectionsnapshot) .


To start, we declare our diffable data source like so:


For the data source’s item identifier, we built an` Item` type, which is where some of the magic starts to happen.


` Item` is a`[Hashable](https://developer.apple.com/documentation/swift/hashable)`` enum` that contains the different cell possibilities for our collection view. Each case has an associated value containing the data necessary to configure that` Item` , such as the relevant cell’s view properties.


You’ll notice we have an` expandable` case with an associated value of type` ExpandableItem` , which is another custom type that contains three properties:


- A` type` to let us distinguish between our two expandable sections
- View properties for the top level cell that says` Offers matched ()` or` Extra earnings ()`
- An array of` subItems` , which will contain` Item` cases for the section content, i.e. the matched offers or related bonuses


Now, we have all of the scaffolding that we need to start building the data for our collection view to display.


Inside our` Presenter` , we map the data we fetch from the network to our` ExpandableItem` and` Item` types.


We start out by creating the view properties for the header item that will display the count. Then, we create a` matchedOffer`` Item` case for each matched offer on the receipt.


Finally, everything is packaged up into an` ExpandableItem` with a` type` of` offersMatched` and passed down to the view for display.


By creating these intermediary types, it makes the collection view flexible, easy to compose, and simple to reason about.


As mentioned earlier, we still need to create our diffable data source snapshots to populate the collection view.


As part of our architecture’s state machine, a new snapshot will be built and applied to the collection view’s data source whenever new properties are provided to the view.


On line 2, we create a brand new, main snapshot and append all of the necessary sections to it.


On line 5, we append the transaction detail` Item` s to the relevant section. This happens to be a non-expandable section that contains the celebration text at the top, transaction date, total amount earned, and more.


Currently, there’s no way to combine a regular`[NSDiffableDataSourceSnapshot](https://developer.apple.com/documentation/uikit/nsdiffabledatasourcesnapshot)` with a section snapshot.


As a result, we apply the regular snapshot, then create the section snapshot and apply it separately.


Then, on line 9, we match the` offersMatchedItem` that we created in the previous example to the` expandable` case.


Doing so gives us access to the` ExpandableItem` associated value, which contains the sub-items to display in the section.


Once that’s done, we create an empty`[NSDiffableDataSourceSectionSnapshot](https://developer.apple.com/documentation/uikit/nsdiffabledatasourcesectionsnapshot)` and append the` offersMatchedItem` to a` nil` section, which essentially places it at the root (the very top level of the expandable hierarchy).


When this item is configured later, it will be in charge of expanding and collapsing the section, as well as displaying the number of offers matched.


Finally, we append the sub-items — the matched offers — to that` offersMatchedItem` and apply the snapshot to the` offersMatched` section.


The ability to append an array of item identifiers to another item identifier is exclusive to`[NSDiffableDataSourceSectionSnapshot](https://developer.apple.com/documentation/uikit/nsdiffabledatasourcesectionsnapshot)` s and is what creates a hierarchy of data and the expand and collapse behavior.


Press enter or click to view image in full size


The expand and collapse animations


In addition to a diffable data source, we’re using[cell registrations](https://developer.apple.com/documentation/uikit/uicollectionview/cellregistration) — introduced in iOS 14 — to configure our collection view cells.


Cell registrations aren’t required for diffable data source; you can still dequeue cells as normal, albeit within the data source’s`[cellProvider](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource/cellprovider)` closure instead of the familiar`[UICollectionViewDataSource](https://developer.apple.com/documentation/uikit/uicollectionviewdatasource)``[cellForItemAt](https://developer.apple.com/documentation/uikit/uicollectionviewdatasource/1618029-collectionview)` callback.


However, cell registrations are nice because they combine registering a cell and configuring it into a single step.


Here’s how it looks for the expandable` offersMatched` section:


The process is pretty straightforward. Create a cell registration, then dequeue the cell inside the data source’s`[cellProvider](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource/cellprovider)` closure, passing in the cell registration, index path, and item identifier.


The cell registration’s closure serves the same purpose as the`[cellForItemAt](https://developer.apple.com/documentation/uikit/uicollectionviewdatasource/1618029-collectionview)` method, except it’s intended only for a specific cell type.


Some other neat APIs are`[willExpandItem](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource/sectionsnapshothandlers/3600963-willexpanditem)` and`[willCollapseItem](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource/sectionsnapshothandlers/3600962-willcollapseitem)` , both of which are available on the`[sectionSnapshotHandlers](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource/3600966-sectionsnapshothandlers)` property of`[UICollectionViewDiffableDataSource](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource)` and`[UITableViewDiffableDataSource](https://developer.apple.com/documentation/uikit/uitableviewdiffabledatasource)` .


They’re essentially callbacks that will be triggered — like the names suggest — when a section is expanded or collapsed, so we can run custom logic in response to those events.


You can read more about them[here](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource/sectionsnapshothandlers) , as well as the`[shouldExpand](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource/sectionsnapshothandlers/3600960-shouldexpanditem)` and`[shouldCollapse](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource/sectionsnapshothandlers/3600959-shouldcollapseitem)` variants of those methods.


So, that’s a bit about how our redesign of the earnings detail came together.


We’re pretty excited about it, not only because it provides a much better experience for our Savers, but it will also serve as a jumping off point for even more helpful features coming soon.


*Interested in working at Ibotta? Check out*[https://ibotta.com/careers](https://ibotta.com/careers) *to browse openings and learn more about us!*
