---
schema_version: "1.0.0"
document_id: "238636c2acc89bc69217dbcc85ab010ef2b8cb941ebfacf52b141badd53f40f3"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/accelerate-ios-development-using-deep-links"
published_at: "2023-03-13T09:18:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:593b67e2e0db37253092b612afce0fe8484a6af336049be84ef5c246750cb608"
---

# Accelerate iOS Development Using Deep Links

Do you work on a large app with lots of deep links and dynamic content? This article will show you how to manage and leverage those deep links so you can spend more time coding and less time navigating the app.


Here on the News team at Doximity, we have lots of content to manage, including articles, comments, and collections of articles. The problem compounds when considering that articles have a lot of different variations, and some of those variations are uncommon. The variety can make it hard to test the entire app, and a real pain when trying to debug a problem on a specific piece of content. Fortunately, most of that content is accessible via a deep link with the correct IDs. Unfortunately, that often means keeping a list of links in a notes document or sending a link to yourself in Slack. It’s even worse when using the simulator since you cannot access those tools easily.


That’s where[Alfred](https://www.alfredapp.com/) comes in. Alfred is an application launcher and productivity application that includes a rich set of tools for creating custom workflows. An Alfred workflow can organize deep links and launch them on any running simulator. Let’s use Apple Maps as an example, as it’s available on all simulators and supports deep linking to Guides. (Provided by Apple, Guides are curated collections of related points of interest. They can be accessed with a deeplink in the format` maps://?pg=<guide id>` ).


Apple Maps has many Guides with different combinations of features to test. Finding a particular guide using the in-app search can take a lot of time. The Alfred workflow becomes both documentation of the various guides and a fast way to navigate to them. In the demo below, a hotkey triggers the workflow. Note that the workflow prompts to select a simulator when there are multiple running.


The Alfred workflow consists of 4 work performing steps and some glue. The image below shows the Alfred workflow (which can be downloaded at the bottom of this article). The green steps cover selecting the deep link while the purple select the simulator.


### Step 1: List of Deep Links


The List Filter provides a UI for selecting a deep link. You can add new deep links to the list by editing this step. You’ll want to give the deep links useful names, as the list filter will search by name. For the Doximity workflow, the deep links are organized by environment and destination. For example, “QA Collection: Sean's Activity” opens the app to Sean’s activity collection and should be used with the QA environment.


The example list filter also includes an item that reads a GuideID from the clipboard and opens that guide. The “else” to the right of the list filter looks for the` guideId` deep link and creates an actual deep link using the clipboard contents (` maps://?pg={clipboard}` ). Add additional clipboard-based deep links by adding them to the list filter and expanding the else step that follows. A clipboard-based deep link that takes a fully formed web URL and converts it to a deep link (e.g.,[https://www.doximity.com](https://www.doximity.com/) becomes doximity://) can save a lot of time when getting links from web developers. Converting the link to doximity:// allows the link to open directly in the app instead of Safari.


### Steps 2 and 3: Select a Simulator


Step 2 gets the count of running simulators and skips to step 3 if there’s only one. Otherwise, step 3 produces a list of running simulators and presents them for selection.
The code to check simulators uses “xcrun” to generate a JSON formatted list of devices and “jq” to parse and filter those devices. Thanks to Jeff Mercado and his[Stack Overflow post](https://stackoverflow.com/questions/58806843/how-to-get-list-of-available-ios-simulators/58812505#58812505) for the foundations of the “jq” (see installation instructions at the bottom of this post) code used in both of these steps.


### Step 4: Launch Deep Link


This step launches the deep link (URL Scheme to be exact) via the terminal. In step 3,` booted` is replaced with the simulator’s UUID to select a specific simulator.


` xcrun simctl openurl booted maps://?pg=3913404366591299416`


The Doximity version of the workflow above has saved countless hours of clicking through the app. In addition, having that list of links is handy when other people ask how to get to a specific piece of content. If you have ideas on improving this workflow, please contactme . In the meantime, here are some ideas for future enhancements:


- It isn’t easy for multiple people to maintain the list as the deep links are embedded in the workflow .plist file. It’d be nice to populate the list from one or more JSON files checked into source control. It would be really nice to mix those JSON items with manually added items, as it’s handy to quickly create new deep links when debugging a problem.
- It would be great to recreate this workflow in Shortcuts, as not all developers have an Alfred license. It’s probably feasible but might not be as nice to use.


Workflow requirements:


- Alfred and a Powerpack license.
- jq (` brew install jq` )
- Download[here](https://drive.google.com/file/d/1GJikUHZVYOamI1RWsxenEZ0BfKStPne9/view?usp=sharing) and follow these[installation instructions](https://www.alfredapp.com/blog/tips-and-tricks/tutorial-importing-and-setting-up-alfred-workflows/)


It might seem daunting to start building the deep links you need for your app, but doing it a little at a time is easy. Next time you’re working on a bug on a specific piece of content, add the workflow, add the link, and start using it. You’ll thank your past self the next time you onboard a new teammate.


Special thanks to Jessica Emerson, Sean McMains, Bruno Miranda, and Jasdeep Saini for reading drafts of this blog post and for giving me their feedback.


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech/) if you'd like to be notified about new blog posts.
