---
schema_version: "1.0.0"
document_id: "5fb14679e9a2c1a401c618d1005b06ae2571b194e9b12731f7f3b4c7d52ee8dc"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/172098648605"
published_at: "2018-03-21T11:34:17+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:329b494e75ec2c4997657c8991e889a47a7cd4efcaf33b8297bfdb90e979d72d"
---

# How we hacked our conference rooms to get more feedback [Case Study]

Beacons are often used in workplaces to make working more efficient or enjoyable. It’s no different here at Estimote - we love tackling various issues with our own tech.


We spend plenty of time in conference rooms, planning new features and iterating on existing ones. The habit of giving constant feedback is strongly embedded in our company culture. We do our best to give each other feedback on how we run meetings to make them more efficient and action-oriented. Often, though, there’s no time for that: once the meeting is finished, everyone disperses and moves on to the next task. Our leaders are then left hanging, wondering, “Did it go well? Could I have made it more concise? Was the agenda appropriate?”


A few weeks ago during an internal hackathon, we built an Android app to address this problem. We integrated it with the latest Estimote Proximity API and Google Calendar’s API. Stay with us as we share a quick step-by-step guide below (code snippets included)!


### How does the app work?


Our app asks you for feedback right as you are leaving the conference room. (Timing is key, right?). It sends a notification and takes you to a simple rating page. With a star rating system you can evaluate various aspects of the meeting and can also leave a note for the organizer! The app then prompts you to send the feedback to the facilitator’s inbox. For this use case, we placed one Proximity Beacon in every conference room and integrated with G Suites. (Everyone here, including our conference room, has its own calendar).


**Note** : We considered the fact that one can just fetch data from the calendar and push notifications the moment a meeting is scheduled to end. But, what if a meeting ends five minutes earlier than anticipated? You would get the notification when you’re already on to the next task and probably discard it for later. And if a meeting is running late, it’s even worse! There is nothing more distracting than everyone getting simultaneous notifications just as you’re wrapping up. To know exactly when you exit the room, we use Proximity Beacons that trigger a notification precisely when you leave the defined area.


Okay, back to the actual case study!


**1. Ask for the necessary permissions**


For this particular case, you need:


- access to user’s calendar*
- access to Bluetooth so that the app is able to interact with beacons


You can find a tutorial on asking for permissions available[here](https://developer.android.com/training/permissions/requesting.html) .


*For the purpose of building this use case, we assume that everyone has some calendar app, and also that their work calendar is synced.


**2. Get the list of calendars**


To get the events, you need to get the right calendar first. Android doesn’t make it easy to figure out which calendar you should be fetching. So, the first thing we did was get the list of all phone calendars thanks to[Google’s Calendar Provider](https://developer.android.com/guide/topics/providers/calendar-provider.html) . We logged the calendar id, display name, account name and owner. Remember to replace name@example.com etc with your desired data.


```text
private val PROJECTION_ID_INDEX = 0
private val PROJECTION_ACCOUNT_NAME_INDEX = 1
private val PROJECTION_DISPLAY_NAME_INDEX = 2
private val PROJECTION_OWNER_ACCOUNT_INDEX = 3


var cur: Cursor? = null
val cr = contentResolver
val uri = Calendars.CONTENT_URI
val selection = ("((" + Calendars.ACCOUNT_NAME + " = ?) AND ("
+ Calendars.ACCOUNT_TYPE + " = ?) AND ("
+ CalendarContract.Calendars.OWNER_ACCOUNT + " = ?))")
val selectionArgs = arrayOf("name@example.com",
"com.example","name@example.com")
cur = cr.query(uri, EVENT_PROJECTION, selection, selectionArgs, null)
cur?.let {
while (cur.moveToNext()) {
val calID = cur.getLong(PROJECTION_ID_INDEX)
val displayName = cur.getString(PROJECTION_DISPLAY_NAME_INDEX)
val accountName = cur.getString(PROJECTION_ACCOUNT_NAME_INDEX)
val ownerName: String = cur.getString(PROJECTION_OWNER_ACCOUNT_INDEX)


Log.i("Calendar", "ID: $calID DisplayName: $displayName AccountName: $accountName OwnerName: $ownerName")
}
}


```


**3. Find the right calendar and fetch the events**


From the list of calendars, we find the one matching the email address of the user and grab its ID. We use this ID (in our case it’s ‘3’) to fetch the list of upcoming events for a given user. It’s up to you how far ahead you look - in our case, we take the events for the next 14 days. That way, the app doesn’t need to to fetch events over and over again. Lastly, we log all events with its title, start date, end date and organizer.


```text
val calID = 3
val now = Date().time
ContentUris.appendId(builder, now - DateUtils.DAY_IN_MILLIS * 1)
ContentUris.appendId(builder, now + DateUtils.DAY_IN_MILLIS * 14)
val eventCursor = contentResolver.query(builder.build(),
arrayOf("title", "begin", "end", "allDay", CalendarContract.Events.ORGANIZER), "Calendar_id=" + calID, null, "endDay ASC, startMinute ASC")
if (eventCursor.count > 0) {
while (eventCursor.moveToNext()) {
val title = eventCursor.getString(0)
val begin = Date(eventCursor.getLong(1))
val end = Date(eventCursor.getLong(2))
val allDay = !eventCursor.getString(3).equals("0")
val organizer = eventCursor.getString(4)
Log.i("Event:", "Title: $title Begin: $begin End: $end All Day: $allDay Organizer: $organizer")
}
}


```


**4. Add the beacon scanner to the app & scan**


We want to know when exactly a person leaves the room. To get the most accurate results, we use[Estimote Proximity SDK](https://github.com/Estimote/Android-Proximity-SDK) . We place a Proximity Beacon in every conference room and start scanning for them.


```text
val cloudCredentials = EstimoteCloudCredentials(YOUR_APP_ID_HERE, YOUR_APP_TOKEN_HERE)
val proximityObserver = ProximityObserverBuilder(applicationContext, cloudCredentials)
.withBalancedPowerMode()
.withOnErrorAction { }
.build()


val venueZone = proximityObserver.zoneBuilder()
.forAttachmentKeyAndValue("room", "mint")
.inNearRange()
.withOnEnterAction { }
.withOnExitAction { showNotification() }
.create()


val observationHandler = proximityObserver
.addProximityZone(venueZone)
.start()


```


**5. Trigger a notification**


To push a notification to a user, several conditions have to be met:


#### • a user has a meeting that has just finished or is ending soon


We don’t want to send users notifications about events that finished several hours ago or will start next week, obviously. So, in our code, we only look for events that end 20 minutes before and after the current time. To give you an example, if it’s 3:25 PM, our app will only check if a calendar event ends between 3:05 PM and 3:45 PM and will ignore any others.


#### • a user enters the beacon’s range at least 15 minutes before and hasn’t left since


This condition eliminates all the passers-by who accidentally entered into the beacons range. We assume that if someone stays in the beacon’s range for 15 minutes or more, they’re attending a meeting.


#### • there is an exit event


Most use cases of beacons work based on enter/exit events. You define the[proximity zones](https://developer.estimote.com/proximity/android-tutorial/#define-proximity-zones) of e.g. 5 meters and then you[program specific actions](https://developer.estimote.com/proximity/android-tutorial/#create-proximity-zone-objects) to be performed when a user enters the range (enter event) or leaves it (exit event). In our case, we’re checking if anyone is leaving the range of our beacon placed in a conference room.


If all the conditions above are met and a user receives an exit event, that’s a clear sign this person has just left the meeting.


**6. Display a notification**


We use[Android Native Method](https://developer.android.com/training/notify-user/build-notification.html) and display the feeback screen:


**7. Redirect to an email client**


When the “send feedback” button is hit, we redirect a user to his/her default email client with a predetermined email.


```text
sendButton.setOnClickListener({
val intent = Intent(Intent.ACTION_SENDTO)
intent.type = "message/rfc822"
intent.putExtra(Intent.EXTRA_EMAIL, "name@example.com")
intent.data = Uri.parse("mailto:name@example.com")
intent.putExtra(Intent.EXTRA_SUBJECT, "Your meeting has a new rating")
if (!TextUtils.isEmpty(commentEditText.text)) {
intent.putExtra(Intent.EXTRA_TEXT, "Overall score: " + overallRateBar.rating + "\nPunctuality: " + punctualityRateBar.rating + "\nAgenda: " + AgendaRateBar.rating + "\n Comment: " + commentEditText.text)
} else {
intent.putExtra(Intent.EXTRA_TEXT, "Overall score: " + overallRateBar.rating + "\nPunctuality: " + punctualityRateBar.rating + "\nAgenda: " + AgendaRateBar.rating)
}
startActivity(Intent.createChooser(intent, "Send mail..."))
})


```


Because of the limitations of our hackday, we didn’t get to add the option of anonymizing the message user sends. But you can do it easily! If we had a few more hours, we would have also built a web dashboard that would be capable of gathering all this data and calculating average scores over time. It’s definitely a priority for next time!


**8. Send a user back to the app and display the “thank you” message.**


```text
val intent = Intent(this, ThankYouActivity::class.java)
startActivity(intent)


```


Following these steps, we were able to build a working prototype of our app in one day! And it worked pretty well, even in our crazy office environment with thousands of beacons.


### Some further inspiration


We know there are things we could improve in this app to make it even more reliable. Here are some ideas:


-


Our app doesn’t actually check which room you’re in. We only determine if you’re in a conference room or not. This could be easily changed with[Cloud Attachments API](http://blog.estimote.com/post/168674042150/estimote-cloud-attachments-api-store-context-in) . Your app could easily return the information about which particular room a user is in. It could also verify it against the room’s calendar to make sure you don’t get notifications for events you’re not involved in.


-


Our app sends feedback requests to the event organizer and there’s no point in them rating their own meetings, right? It’s a fairly simple fix! Just add one extra condition to the code and ignore the exit events if the person in question was an organizer.


-


To make the task easier, we had to assume that users have their @estimote.com account synced on their phones, otherwise we wouldn’t be able to fetch their events. It would be much more efficient to fetch the calendars of the conference rooms and compare the list of participants with the devices in range of particular beacons.


**There are tons of use cases for conference rooms** that should be fairly easy to build with a bit more time. Here are some of our ideas:


-


send a Slack notification to the organizer if there’s another meeting starting in X minutes


-


check if a room is occupied on a web dashboard or in an app


-


display, in an app, the meetings on deck for the room you’re in (in case you decide to have some focus time in a room without a reservation)


-


automatically ping people on Slack if they are late for a meeting (they’re not in the range of the respective beacon)


-


check where everyone is at a given time. Use our[Indoor Location](https://community.estimote.com/hc/en-us/articles/203493626-What-s-Estimote-Indoor-Location-) or[Proximity SDK](http://blog.estimote.com/post/166007374930/the-most-reliable-proximity-tech-now-with-a) .


There are probably many, many more! Have you built something similar that you would like to share with the world? Or maybe you would like to build a similar use case and don’t know how? Let us know!


*Crafted by Piotr Małek, Community Manager and Martyna Leniart, Android Developer.*
