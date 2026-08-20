---
schema_version: "1.0.0"
document_id: "44f821ad8bb031d71105ab60b50bf5dfae27abe46566783bc78f23e68eaa9d09"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-mobbin-moved-from-firebase-to-supabase/"
published_at: "2021-10-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:dcacbb8ed55b3fa6599426b5197ed099e3371f73f30dbb4365707bd44a5132a1"
---

# How Mobbin moved from Firebase to Supabase

I had the opportunity to chat with Liau Jian Jie, CTO & co-founder of[Mobbin](https://mobbin.com/) and talk about their migration from Firebase to SQL on Supabase earlier this year. Mobbin is a tool for designers to see and track UI flows from mobile applications to help with real-world inspiration for their own design work. I used it personally back when I was working on a location based mobile app at Asurion, and it was unbelievably helpful for pattern discovery.


I reached out to Liau after seeing this tweet from their Twitter account:


[https://twitter.com/MobbinDesign/status/1420569346858909696?s=20](https://twitter.com/MobbinDesign/status/1420569346858909696?s=20)


Not many companies have made this switch, and at Basedash we’ve been hearing a lot of requests to support Firebase in the future. We already support Supabase as well as any other Postgres databases, and even use it for all of our mock databases.


This post is not an endorsement of one database over the other, but as Supabase is the new kid on the block, and has a very active community, we thought it would be worthwhile to share the perspective of one team that’s decided it was right for them.


Thanks again to Liau for taking the time to talk through his process and hope it’s helpful to others debating the switch themselves.


---


## Here’s the interview


Audio and transcript:


[https://share.descript.com/view/evRIGoLER5a](https://share.descript.com/view/evRIGoLER5a)


## **Tell me a little bit about yourself and what kind of team you’re working on.**


I work on Mobbin, where I pretty much lead all technical and engineering decisions over here. We’re really small team actually, and we’re based out of Singapore. So yeah, in the past, something more relevant to what you guys are doing is that we actually moved from Firebase.


At the same time, we relaunched the[Mobbin.design](https://mobbin.com/) product at the time. And the reason for that migration was because we hit some limitations on Firebase really while building that new iteration of Mobbin.design and we made the painful decision to drop whatever we were doing and start migrating over to Supabase.


## **What was the first thing that made your team start talking about that switch?**


It was really indexing, right? We have a very read-heavy kind of application, and so indexing is really important. The thing about Firebase is that if you want to do queries with relationships, you have to do a lot of round trips or you got to try to work around the limitations on the indexing, which is actually really limited and there are some hard limitations on indexing.


For example, there’s a[fixed maximum number](https://firebase.google.com/docs/firestore/query-data/index-overview#indexing_limits) of composite indexes on Firebase. Whereas for Postgres, you can have as many indexes as you like, as long as you can just scale the underlying storage space of your database.


## **Was there something that triggered that switch to move it over?**


We hit a hard constraint that we couldn’t work around on Firebase, while we were trying to build a better search experience. If you look at the new Mobbin, you can have very complex compound queries. You can look for screens that have buttons, sliders, and are also part of a sign-up flow or part of our settings flow. You can do all sorts of crazy complex operations, which, as a designer, is what you want to do.


You want to filter by category? What are all the finance apps doing for the login flow when they have a certain element on the screen? But with Firebase, we couldn’t do that.


## **How long was it before you decided to make the switch? How long before you said “we should do this” to “we actually are going to do this”.**


Pretty quick, actually.


I made the decision overnight because I couldn’t really sleep. When we hit the hard constraint, well, it was basically a moment of chaos for us because we basically couldn’t continue building the product. Like, if we can’t build out the search experience to our specifications on Firebase, then we can’t do anything.


Before that, I had already played with Supabase in my own free time. And within that same night, I pretty much figured it out how we could get the whole migration going.


It was a huge risk for sure. It’s going to take up quite a bit of time, but we managed to put it on a timeline that could work within our launch period. And actually the upside was that with Supabase it was so much easier to work with.


## **In what ways is it easier to build with?**


There’s nothing proprietary, it’s just SQL. So if you’re familiar with SQL, then it’s going to be a breeze, even when it comes to, let’s say access control. With Firebase they have this access control tab inside of Firestore and you have to learn that proprietary syntax to encode rules, like who can view what data.


But Supabase is just SQL. They’re using this feature called role-level security to omit or include what information the end user can see, and this is just all SQL— so it’s one language and it works for pretty much everything.


## **How long did it take you to migrate everything over?**


Maybe four to six weeks. Yeah, because we were building our features concurrently. So we were trying to balance shipping and also doing the migration.


I’d say the biggest challenge when migrating was actually validating the data before we could insert it into Supabase, because Firestore is NoSQL, and the adherence to the schema is not guaranteed by the database.


Usually you have this validation logic, but let’s say if you’re doing a migration, then you don’t get that database provider relation on the database as well. You have to write up additional logic to perform the migration.


Mobbin has been around for two years, so our data is pretty dirty. We had to insert additional metadata here and there, and our migration script was pretty huge because we had to validate that data and make sure that we could fit them into a schema.


## **Could you tell me a little bit about how you migrated your data?**


First off we had to understand how Supabase works. The good news is that is that it’s completely open source.


You can just poke around into the database. If you’re looking into the auth schema, you can see the entire table. It took a little bit to figure out how I can turn Firebase auth users into Supabase auth users, but that was pretty easy actually. The difficulty was things that are related to our application data.


We use this feature on Firebase—Firestore collections—and we had sub collections. Now the tricky part about Firestore collections is that if you do not have your preexisting schema in Firestore in a way that lets you conveniently perform compound queries that traverses the subcollections. It gets really difficult to actually pull out the data and associate it to the user and make it into Postgres rows. That was really a huge challenge. We had to do many round trips.


For example, when we were trying to migrate our “libraries” from the old Mobbin to this new Mobbin “collection” we had to first pull our user data and pull all the Firebase collections, then the users’ subcollection, which is like an O(n*m) operation, so to pull that amount of data for 200,000 users took us like four hours—if we had one mistake in the migration, we have to redo it all over again.


That was a very painful process.


## **Did you find any other companies that have also made this switch or something in forums or other blog posts or anything that was helpful?**


There are users who tried to migrate, but the difficult bits were really unique to our own scenario. So far the part where we migrated all of the user data, we actually open sourced that migration script, if you want to export your Firebase auth users, it’s pretty simple. You can just download my migration script. The caveat is that though at Mobbin, we were lucky to not support password or phone sign ins, so we never had to migrate that.


But if you have to migrate that it’s a little bit trickier. If you’re already migrating your users over from Firebase to Supabase and you’re using password sign-in, you have to force your users to reset their passwords.


## **What benefits are you seeing if any, so far?**


First of all the developer experience is just so much better. Way better.


And the fact that it is open source really makes it so much easier. I can poke into the codebase and see how everything works. And Supabase is built on top of Postgres, right? Which is really great. It is a great piece of software. You can do pretty much, most things that you ever need to without having to step into writing SQL views.


The next would be definitely performance. So if you remember using the old Mobbin, it would take five seconds for the page to load. Right now, it’s almost instant, the whole page loads in less than a second.


Purely because it’s SQL. Which is that whole decade of engineering expertise going to shaping Postgres to what it is today.


I think the confidence of scalability, that’s the next one I would say is the biggest upside, which is now that we are on Postgres and on Supabase, we feel really confident to do much more complex operations and provide more value for our customers.


## **Did it change how you structured your data ?**


Oh, for sure. From NoSQL to SQL, it’s much easier, right? First of all, you can avoid writing hacks or having to compromise. If you started using Firestore collections and you want to do complex queries, you’ll find that, very quickly, you’d need to compromise on many things. You can either compromise on speed or you could choose to abuse the Firestore indexes just to get what you want.


Whereas with SQL, it’s so much easier. If you’re going to do relationships, they are so well-documented, and you get a lot more data types. You can also perform operations that are tailored to those data types. Also, you have access to all the Postgres extensions.


For example: Full-text search. That’s something that’s been pretty well explored in Postgres and obviously it’s near native support, we have TS vectors.


Whereas with Firebase, you can’t even do that. You have to use a third-party service, like Elastic Search or Algolia.


## **Is there anything that you miss from Firebase?**


Honestly, not really. I don’t regret it.


It was a huge sense of relief when we moved. Because it was just basically technical debt at that point in time.


## **Was there a cost savings with the switch?**


Oh, yeah, for sure. But the thing is that it’s hard to calculate it. Firebase also includes Firebase storage, at this point of time I’m not really sure what the pricing of Supabase is. They only charge for traffic and not for object storage.


In terms of the storage there is huge cost savings, and also for hosting the data itself. I would say the cost got reduced to a negligible cost.


## **What are your thoughts on SQL versus NoSQL?**


I would say with NoSQL we started with Firebase because it was NoSQL and it really helped us launch a product really quickly.


When your product requirements are clear, then \[SQL\] is definitely the way to go. There’s no point trying to maintain the shape of the data in runtime, the database can just do all the heavy lifting for you.


## **If you were to start something fresh, do you think you would also want that flexibility or would you go straight to Supabase?**


I would go straight to Supabase because the dashboard UI is great. You get the flexibility just with the dashboard. So I would say there’s almost no reason with starting a new project on Firebase anymore now that we have Supabase.


## **Do you have any other closing thoughts?**


I’ll say to anyone out there who’s trying to build a new product or who is thinking of investing in a database, between Firebase or Supabase, I say, just skip Firebase. Your investment in SQL or Supabase would pay off.


The headache of Firebase is just not worth it.


## **Do you have any thoughts for people who are contemplating a switch from Firebase?**


If your product isn’t going to change much, if you don’t intend to invest in the foundation layer, that is the database, then I think it’s fine. You can just stick with Firebase.


But if you have an intention to scale to do much more complex operations, then definitely consider migrating over to some something that’s SQL or Supabase.


And there’s a huge community behind it, so you can always leverage on the community experience.


It was really helpful and I appreciate what you’re working on and hope you the best. Hopefully we’ll talk again soon!


## Helpful links


[Mobbin - The world’s largest mobile app design reference library](https://mobbin.design/)


[GitHub - liaujianjie/firebase-to-supabase-auth-migrator](https://github.com/liaujianjie/firebase-to-supabase-auth-migrator)


[The Open Source Firebase Alternative | Supabase](https://supabase.io/)


[Firebase](https://firebase.google.com/)


This video was transcribed and edited with:


[Descript | All-in-one audio/video editing, as easy as a doc.](https://www.descript.com/)
