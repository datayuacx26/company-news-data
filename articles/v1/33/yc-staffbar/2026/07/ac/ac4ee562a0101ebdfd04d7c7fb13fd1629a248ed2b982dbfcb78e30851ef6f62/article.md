---
schema_version: "1.0.0"
document_id: "ac4ee562a0101ebdfd04d7c7fb13fd1629a248ed2b982dbfcb78e30851ef6f62"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/how-to-integrate-siri-and-apple-intelligence-into-your-app-to-query-onscreen"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:fe5cfac9bb7bb52acd84126fb8ddf0a991f42aa89399f9e0d8562fdd8ae41688"
---

# How to integrate Siri and Apple Intelligence into your app to query onscreen content

Since the announcement of Apple Intelligence, developers have been in a bit of holding pattern with iOS 18. Many of the marquee APIs to hook into Siri and Apple Intelligence weren't available with its initial release. For example, hooking into onscreen content.


The ability to have Siri perform tasks from your app, or shuttle data from it to another one, based on your personal context is a game-changer. We've always been able to ask Siri to perform tasks in our apps, but Siri couldn't understand the specifics of what you were currently looking at.


In Apple's W.W.D.C. session "[Bring your app to Siri](https://developer.apple.com/videos/play/wwdc2024/10133) ," the presenter asks Siri to favorite a photo. Apple Intelligence makes this flow better starting with iOS 18.2, since developers can expose their app's onscreen content. Basically, this opens up two things for developers:


1. Primarily, you can create entities that Siri can understand, simply from them being visible on screen. The entity from the example above was a photo, and it was recognized when the presenter said "Hey Siri, move this photo to the California album."
2. And, you can create intents that Siri can use with that content, or from content coming to your app from somewhere else. Moving the photo to another album from the example would be the intent at play.


What's key to understand here is that the photo is an entity Siri can use and moving it to an album was an intent. And, as a bonus — you can shuttle the data around easier with` Transferable` . This isn't *new* but is *important* . Again, from the example, this is how Siri took the photo and sent it in an email.


Today, I'll show you how it works and break down the key parts you'll need. However, there's an important catch: Siri can't actually perform these tasks yet. While the APIs are available to developers, the personal context features aren't live. Once they are, everything will 'just work.' For now, Apple seems to be giving us the tools today so we can be ready for tomorrow.


## The APIs required to expose onscreen content to Siri


Unsurprisingly, much of the API needed to make this work can be found in the App Intents framework. After a bit of search, I think that it's best understood by a brief overview of each component you'll be using:


1. **App Entity** : The app entity is the "what" behind all of this. What are we working on? A photo, document, text file, etc. It can also be one of your app's proprietary models.
2. **App Intent** : The "how", what will we do with that app entity? This is where you invoke your app's logic to operate on something.
3. **User Activity** : A` NSUserActivity` will help make your entities discoverable to Siri *when they are onscreen* .
4. **Transferable** : Finally, a` Transferable` representation of your entity means that other apps can understand it, which can help Siri "chain" commands together. For example, if your entity is a text document — then your` Transferable` implementation can expose it as such so any app that operates on text could use it. This is not required, but it allows Siri to use your data across the system — so in my mind, if you have an app entity, then you should also have a` Transferable` representation too.


Now, on top of that, there are two more critical components —[assistant schemas and entities](https://developer.apple.com/documentation/appintents/integrating-actions-with-siri-and-apple-intelligence) . These build *on top of* either existing app entities and intents, or they can be used to make new entities and intents from scratch.


Let's go back to Apple's introduction example of favoriting a photo. Before iOS 18.2, we could always make an app intent to do that:


```text
struct   FavoritePhotoIntent:   AppIntent   {
static   var   title   =   LocalizedStringResource  (  "  Favorite Photo  "  )
static   var   description   =   IntentDescription  (  "  Adds a photo to the Favorites album.  "  )


@  Parameter  (title  :   "  Photo  "  )
var   photo: [PhotoEntity]


func   perform  ()   async   throws   ->   some   IntentResult {
let   manager   =   PhotoUtil.  shared
manager.set(  photo,   favorite  :   true  )
return   .  result  ()
}
}
```


But now, if we use a predefined schema, we can have Siri respond to the request much more accurately since it's trained its own models on these specific tasks (assistant schema) and data types (assistant entity). So the above would change to something like this, where we could favorite the photo and then some — because the model behind` .photos.updateAsset` is laser focused on doing that job specifically. Here's the difference, as shown in Apple's sample code[available here](https://developer.apple.com/documentation/appintents/making-your-app-s-functionality-available-to-siri) :


```text
@  AssistantIntent  (schema  :   .  photos  .  updateAsset  )
struct   UpdateAssetIntent:   AppIntent   {
var   target: [AssetEntity]
var   name:   String  ?
var   isHidden:   Bool  ?
var   isFavorite:   Bool  ?


@  Dependency
var   library: MediaLibrary


func   perform  ()   async   throws   ->   some   IntentResult {
let   assets   =   await   library.  assets  (  for  : target.  map  (  \.  id  ))
for   asset   in   assets {
if   let   isHidden {
try   await   asset.  setIsHidden  (  isHidden  )
}
if   let   isFavorite {
try   await   asset.  setIsFavorite  (  isFavorite  )
}
}
return   .  result  ()
}
}
```


Notice the` @AssistantIntent(schema: .photos.updateAsset)` above the intent. The` AssetEntity` model is also marked with an assistant entity macro:


```text
@  AssistantEntity  (schema  :   .  photos  .  asset  )
struct   AssetEntity:   IndexedEntity   {
// Rest of code
}
```


These schemas fall under certain "domains", and there are several of them ready to go. Plus, more will be added in the future.


Domains available for Apple Intelligence as of iOS 18.


...and within those, tons of schemas for actions and entities.


Examples of available schemas from iOS 18 domains.


This is the critical (and new) idea to understand. We decorate our existing (or new) app intents and entities with these *assistant* intents and entities. Each of them come from a broad domain, and within each one, there are individual actions and data models we can use (i.e. the schemas). This helps Siri understand what something is, and in turn — what can be done with it.


These are all big ideas, and if you're new to any of them — some of this is hard to follow. For a refresher on how powerful app intents are and how to use them, see the related post on App Intents tutorials for iOS developers.


## Implementation flow


While that's a bit of boilerplate to understand, it is important to grasp. Without it, looking at the nascent documentation for Apple Intelligence can be hard to follow. Before we jump into our own code, let's recap all of the steps we'd take to make our app's onscreen contents available to Siri and Apple Intelligence:


1. **Associate our task to an existing domain:** This means we'd tag an existing app intent, or create a new one, with the assistant macro above its definition:` @AssistantIntent(schema: .theDomain.theSchema)` . This tells Siri what kind of stuff we can do, and the kinds of data we can do it on.
2. **Associate app data with an existing domain:** For any app data, we'd associate it to a domain's schema too, much like we did with the app intent. Remember, this hooks into Apple's trained models for the given domain, which means it's hyper-focused for the task or model being used:` @AssistantEntity(schema: .theDomain.theSchema)` . This is how Siri knows what kind of "thing", i.e. entity, is at play.
3. **Inform Siri & Apple Intelligence of the data being onscreen:** This is where` NSUserActivity` comes in. It tells Siri that "Hey, this data is on the screen", and by associating a user activity with` EntityIdentifier` it knows what that data is shaped like.
4. **Making content shareable:** Finally,` Transferable` means that we can shuttle our data in known formats so other apps and services can use it too. If our data is simply a text document, then` Transferable` is what lets Siri take its contents and put it into a third party email client.


Those are the big ideas. Domains and their associated schemas represent the data type and what to do with it, while` NSUserActivity` makes it discoverable and` Transferable` makes it shareable (for lack of a better term).


## Journaling example


Again, before we proceed — it's important to note that at the time of this writing, the APIs to set this up exist in Xcode Beta 16.2, but Siri's personal context *isn't* ready yet to handle the requests. When that's no longer the case, this post will be updated. So — right now, Apple has these APIs released so we can be ready to go, but unfortunately we can't test very much.


With that said, here's an example you can copy and paste into Xcode 16.2 or above. We'll make a journal entry that you can ask Siri questions about, or vend to another 3rd party app or service.


In this code, you can write some contents in a mock journal entry and then ask Siri things like, "What's in this journal?", "When was this written", or any other sensible questions you'd have about a journal entry. Remember the setup: create an entity, use an assistant schema and make Siri aware that it's onscreen:


```text
// Make the entity, use an assistant schema
@  AssistantEntity  (schema  :   .  journal  .  entry  )
struct   JournalEntryEntity {
struct   JournalEntryEntityQuery:   EntityStringQuery   {
func   entities  (  for   identifiers  : [JournalEntryEntity.ID]  )   async   throws   ->   [JournalEntryEntity] { [] }
func   entities  (  matching   string  :   String  )   async   throws   ->   [JournalEntryEntity] { [] }
}


static   var   defaultQuery   =   JournalEntryEntityQuery  ()
var   displayRepresentation: DisplayRepresentation { .  init  (  title  :   "  Unnamed Entry  "  ,   subtitle  :   "  No contents  "  ) }
let   id:   String
var   title:   String  ?
var   message: AttributedString  ?
var   mediaItems: [IntentFile]
var   entryDate: Date  ?
var   location: CLPlacemark  ?


var   entryText:   String   {
guard   let   postContents   =   message  ?  .asNLGValue   else   {   return   ""   }
return   postContents
}
}
```


Xcode autocomplete for schemas.


Even better, it'll fill out boilerplate code for you. In fact, the only part manually typed in the example above was this:


```text
var   entryText:   String   {
return   postContents
}
```


Now that we have an entity using a schema, let's create a lightweight interface to edit it. First, we'll make a view model for the journal entry:


```text
struct   LightweightJournalEntry:   Equatable  ,   Identifiable  ,   Codable   {
var   date: Date   =   .  now
var   contents:   String   =   ""
var   id:   String   =   "  mockStableIdentifier  "


func   asEntity  ()   ->   JournalEntryEntity {
let   entity   =   JournalEntryEntity  (  id  :   self  .  id  )
entity.  entryDate   =   self  .  date
entity.  message   =   .  init  (  stringLiteral  : contents  )
return   entity
}
}
```


And then, a simple form to edit the date and contents:


```text
struct   ContentView:   View   {
@  State   var   entry: LightweightJournalEntry   =   .  init  ()


var   body:   some   View {
NavigationStack   {
Form   {
Section  (  "  Date  "  ) {
DatePicker  (  "  Entry Date  "  ,   selection  : $entry.  date  )
}
Section  (  "  Contents  "  ) {
TextEditor  (  text  : $entry.  contents  )
}
NavigationLink  (  "  View Entry  "  ,   destination  : {
JournalOnscreenExampleView  (  entry  : entry  )
}  )
}
}
}
}


struct   JournalOnscreenExampleView:   View   {
let   entry: LightweightJournalEntry


var   body:   some   View {
Form   {
Section  (  "  Details  "  ) {
Text  (  "  Written at   \(  entry.  date  .  formatted  (  .  dateTime  )  )  "  )
.  font  (  .  caption  )
Text  (  entry.  contents  )
}
Section  (  "  Instructions  "  ) {
Text  (  "  Now, bring up Siri. Siri should know we're looking at a journal entry, so you could ask it something like   \"  When was this created?  \"  "  )
}
}
}
}
```


With this setup, you can enter in some text, change the date of the journal entry and then bring up a preview interface. This is where we can ask Siri about it, but to do that — we'll need to use a user activity to let Siri know it's onscreen. In` JournalOnscreenExampleView` , add this modifier at the end of the` Form` :


```text
.  userActivity  (  "  com.example.journal.entry  "  ,   element  : entry.  asEntity  ()) { entity, activity   in
activity.  title   =   "  Viewing a journal entry  "
activity.  appEntityIdentifier   =   .  init  (  for  : entity  )
}
```


This modifier vends you a` NSUserActivity` to be associated with your` AppEntity` which is using an` AssistantSchema` . You link the two together via the` appEntityIdentifier` on the user activity.


You're all set! Once Apple enables the personal context features of Siri and Apple Intelligence, you can start asking Siri all sorts of questions about the journal entry. One last enhancement though — what if we want to shuttle this data around? For example, "Copy this journal entry to my clipboard" or something similar. This is where` Transferable` comes in:


```text
import   CoreTransferable


extension   JournalEntryEntity:   Transferable   {
public   static   var   transferRepresentation:   some   TransferRepresentation {
ProxyRepresentation  (  exporting  : \.  entryText  )
}
}
```


With that simple implementation, Siri would be able to use the` entryText` value and send it around the system. That's an elementary example, though.` Transferable` can bend, change, and import data in just about any way imaginable. Refer to Apple's[docs](https://developer.apple.com/documentation/coretransferable) for more (or this wonderful W.W.D.C. session over the framework).


## Wrapping up


Apple Intelligence is only becoming more prominent in the Apple ecosystem. While concepts like app intents and app entities have only been around since iOS 16, and domains and their schemas with iOS 18.2 — everything is pointing towards those becoming critical components to any app. Once you get the basics down, a lot of fun possibilities start to open up.


If you're new, try to start with making a simple app intent. Expose some of your app's model layer as an app entity. From there, try to sneak in some of those concepts to an existing domain if it fits into one. Apple has been on record saying that these domains hook into "Siri and *future* Apple Intelligence features", so who knows where else it could take us in upcoming iOS releases.


I've enjoyed peeling back how these APIs work, and I hope this guide has been helpful. If you need help in other important ways, such as growing your app's revenue or taking advantage of the best paywall testing suite in the game, why not sign up for a[free](https://superwall.com/sign-up) Superwall account today?
