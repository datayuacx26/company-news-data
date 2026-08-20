---
schema_version: "1.0.0"
document_id: "6bb50cdd33ef35d0a1ba0d1084cb3ff6908d4c934e0e642ed450f0a0b0881a6c"
company_key: "yc-directed-edge"
company: "Directed Edge"
source_id: "yc-directed-edge-rss-7c8e8fe81473"
canonical_url: "https://blog.directededge.com/2012/10/29/terror-in-recommendations-ville/"
published_at: "2012-10-29T21:01:33+00:00"
first_seen_at: "2026-07-27T01:55:50.875550+00:00"
fetched_at: "2026-08-20T02:30:11.821086+00:00"
content_hash: "sha256:cc0625b01f2488eb63aead80a3fdce5156be527c32fa7ac1d694d95c997e23ad"
---

# Terror in recommendations-ville

**UPDATE:** Now, 10 months later, we’ve not had a single additional instance of data corruption, which is a spot we’re glad to be back in.


---


Every startup seems to have one of those technical meltdown stories. Up until today, our worst had been back in 2009 when we were still in beta, and it was more embarassing than critical. (Lesson learned then: never deploy just before sleeping.)


Since then we’ve done pretty well. On average we have about 4-8 hours of downtime per year. So today was especially nasty.


### The first sign of trouble


Around 11:01 p.m. GMT I get a mail about an unexpected reboot. One of our other guys had caught an issue and was looking into it, but I hadn’t noticed the SMS he’d sent me saying such was coming up.


We hop on Skype and start going through what’s going on. Our web services process was alternating between crashing and hanging every few minutes. Usually that means some corrupted indexes in one of our databases. We get that once or twice a year. It’s an annoyance (and let me tell you about the fun of trying to track down a bug that happens twice a year), but we have a script that can rebuild the indexes, depending on the size of the database, in somewhere between 10 seconds and 10 minutes.


### A Heisenbug


The trick is figuring out which database is causing the problem. Aside from our enterprise customers, which are usually on their own hardware, we run a bunch of our small and medium sized databases off of the same hardware. Our web services setup is heavily multithreaded, so there are usually some 40-ish threads that are writing to the logs in parallel; figuring out which request was the one triggering the problem can sometimes be a bit tricky.


But we’ve been there before too. We have a switch we can use to flip on function tracing which usuall makes it clear pretty quickly — or at least has in every instance up until now.


This time it didn’t. We kept watching our logs, trying to catch the offending item and guessing the wrong one. We have a script to pull down databases from the production to our development machines, where we check the indexes for corruption. For about an hour we kept missing.


**We hadn’t been offline for a full hour in more than a year** , so we were getting antsy at that point. We’d thought about this situation before too though — what if we couldn’t find the offending database?


### A not-so-super super-tool


The last time we had a similar issue (which was sorted out in under 15 minutes), I wrote a small program to go through and check every single database on a system. It was, as we soon will learn, not so aptly called, “IntegrityChecker”. We’d never run it in production, but it had been tested on dev machines. Basically it would disable an account for a few seconds, try to get a file lock on its files, and then run the integrity checker that we usually use offline to test such things.


Except that for some reason it was given a file lock when the OS should have rejected it. So, now we have mmap’ed files that are being written from two process. Not good. In fact, very, very bad.


Unfortunately, we didn’t notice this immediately. In fact, things looked great — it identified the database with a corrupt index, we rebuilt it, waited 15 minutes to make sure everything was kosher, and headed off to snooze. It was about 4:00 a.m. local time.


### This isn’t supposed to happen


**We have two redundant monitoring systems** that live in a separate network and check all of our servers once per minute. Both of them send out a barage of SMSes if the servers stop responding to requests. But most of the API requests that were coming in were actually being handled — probably 80% or so — and our monitoring systems retry a second time if there’s a failure just so that we don’t get notified every time there’s a minor network glitch somewhere. So we weren’t notified that our servers were still sickly.


### The score


Now, if you’re keeping score, here are the points so far of this perfect storm:


- Relatively minor issue of database index corruption
- IntegrityChecker that’s actually corrupting databases (and not just their indexes)
- Monitoring not catching the problems


### Digging deeper


9:30 a.m. rolls around and we’re up and trying to see if there’s anything we can piece together about the issues from the night before. Pretty soon one of the other guys that’s working with me on the problem notices that the server process actually kept crashing all night, albeit at a somewhat reduced frequency (every 10 minutes or so).


Since the IntegrityChecker had worked so well the night before (as we still thought), I gave it another go. It turned up this time six corrupt databases. We’d never seen that. I rebuilt them, and again, in perfect storm fashion, ran it again. Two more. Something was obviously very wrong.


A little while later the database process is back to restarting every minute. Panic ensues.


### Our monitoring turns on itself


See, we not only have out-of-band monitoring, **we also have monit running to restart things on our machines if something goes wrong** .


We didn’t notice for about an hour that the errors in the monit log weren’t the usual ones. They were 404 — “not found” — errors. That was weird. It turned out the account that we use for status checking was corrupt. We fixed that and were back down to crashes every 10 minutes or so, but still had no idea what could possibly be causing the problems.


A couple of heavily caffinated hours of shooting in the dark passed. Eventually we started to suspect the IntegrityChecker. And we noticed that a few accounts (about a dozen — the ones that had the misfortune of writing while the IntegrityChecker was checking them) had been effectively nulled out. We started getting a sense of how widespread the damage was.


### The low-down


There was a lot that came together to make this as bad as it was. To recap, we started off with a relatively minor issue, which was made into a major issue by a tool that was built specifically for dealing with the exact situation we were facing, and those large issues were not caught by our generally quite good monitoring, which eventually even became part of the problem itself.


### The last resort: the backups


Fortunately we have backups, so we started going through accounts manually, offline this time, checking almost every database to try to find corruption. We found a few more.


At that point we started restoring the broken databases to their last known good state — about 17 hours before all of the mess started. We’ve done that now and things look pretty stable. It’s now been an 90 minutes since we had a crash or hang and all accounts are back online. In some cases we’re trying to merge data that came in later in the day where we can get to it, but that’s a bit spotty.


### An apology


**This is the first time in three and a half years of being in production that we lost data and we feel terrible about it** . We’ll be doing some work on our internal systems to help us monitor, diagnose and fix these issues in the future. Thanks so much for your patience.


Edit: One additional note: if you’re one of our customers that uses our Shopify app and your data seems to be out of date, it’ll automatically be updated the next time that we pull your shop’s data in with our importer. There was no data loss for Shopify customers.
