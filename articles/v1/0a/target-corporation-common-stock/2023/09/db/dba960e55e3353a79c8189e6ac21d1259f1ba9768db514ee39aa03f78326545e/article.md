---
schema_version: "1.0.0"
document_id: "dba960e55e3353a79c8189e6ac21d1259f1ba9768db514ee39aa03f78326545e"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/powering-target-drive-up"
published_at: "2023-09-07T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:d74f1234e07fd647a7e3556427fe2ac1bd05e09e7ec6f252affc32326c4b488e"
---

# Building a Flexible Platform to Power Target Drive Up

In recent years, our Target technology team built solutions from the ground up to make shopping easier for our guests, regardless of where they shop: in-store, or online via Target.com or the Target mobile apps. These technologies have become even more vital in ensuring our guests have a seamless shopping experience with consumer habits changing through the pandemic. The technical foundation we built serves us well in preparing for changes in guest behavior and ultimately was the key to our flexibility that allowed us to adapt and pivot when needed at the outset of the COVID-19 pandemic.


While Target guests see the simple Drive Up interface in their mobile apps, there is obviously much more that goes on below the surface to enable it. We wanted to share how we developed and built our Drive Up technology and some lessons we learned along the way.


How it Started


In 2017, a small team formed with the challenge to figure out how to pilot a drive-up service. We were tasked with creating an end-to-end experience with which we could test our offerings and gather feedback and data about usage to help inform our future work. This was quite a big request, given that there was no precedent, and we’d be needing to build this from the ground up with our small tech team, in partnership with the Supply Chain and Stores teams. There were only a few of us – three engineers, a designer, and a product manager. Though the team was small, our goals were huge. We set out to achieve something big.


We started small, with about 65 orders per day in a handful of select stores in the Twin Cities market in Minnesota. We leveraged feedback from our team members in the area, from stores, and from our corporate headquarters, to help test and learn from our early iterations.


Once our leadership saw that things were running smoothly in our handful of test stores, we expanded our reach and extended the test into 52 stores in the Twin Cities market. As we expanded, our data and learnings increased exponentially as well, as we saw how our technology was responding to larger scale and more orders. We began noticing helpful patterns that informed us on ways we could improve our app and the overall experience for users.


One pattern we noticed was multiple orders. We had many guests who would create an initial order in our system and would then place an additional order for a forgotten item, resulting in duplicate records as guests pulled in to pick up their orders. This created a user experience that was not ideal, as they had to tap separately on each order to alert the store that they were arriving. This led us to rethink our whole approach and modify the Eventing API that we initially built based off “orders” as the unique indicator. This wasn’t the only learning that ended up changing what we built.


Target Drive Up Eventing architecture, v1


When we first released our Drive Up experience, this is how it worked. Note the number of different touchpoints needed from each order, and how the guest data would feed into the store team member's app. The dependency on orders meant a guest had to alert the store separately for each of their orders in their app. Initially, this was fine to work with but as Drive Up expanded and gained popularity we saw an uptick in the number of guests who would have multiple orders, and this caused too many touch points in the app on both the guest and team member side of things.


What We Built


Once we took “orders” out of the equation and centered the individual guest as our main touchpoint for the app experience, it opened the door to build the API to handle additional guest touchpoints like returns and Starbucks orders, as well as future that have yet to be released.


To accomplish this, we first built an API that would fetch all the active guest orders, aggregate them and assign them to a trip in the backend of the app. On the guest side, we understand that there are active orders that are ready for pickup for the guest at a store, so we engage our Eventing API and start a new Drive Up trip. “Eventing” is the concept that each event is triggered by a specific action by the guest. On our team member app, we take the same action or “trip” and use it to alert the store team member that the guest is on their way to pick up the order. The team member app was updated to handle completing multiple orders that belonged to the same trip.


Using the feedback from team members and guests, we got to work at modifying our Eventing API to handle all digital touchpoints in our Drive Up locations. This API would be the foundation upon which Drive Up would be built, so we needed to get it right. It also provides a vital link between the guest’s actions and a store team member’s awareness of these actions, so that added additional complexity for us to consider when building.


When researching and testing our initial build, we determined that orders are the consequence of a guest trip, so when building our API, we decided it made sense to split events into five discrete steps around each trip. This is where our live tests by our team members and local guests were key – they helped us determine which steps were most important in the process, and which could be combined with others.


Target Drive Up Eventing architecture, v2


Here are the five significant events we determined that would complete a Drive Up trip, along with a look at the underlying architecture of the process that we streamlined:


On The Way


: In this initial event, the guest is notified when their order is ready for pick up. The guest self-selects “I’m on my way” in the app on their mobile device, which triggers a notification to the store team member’s device. The guest can also share their car make, model and color, and the location where they’d like their order to be placed in the car (i.e., trunk, back passenger side, front passenger side, etc.), and can now even order from Starbucks to have fresh food and beverages added to their Drive Up order.


Live ETA


: For guests who have opted in to location sharing, every 500 meters the guest moves, the estimated time of their arrival to the store is updated in the team member-facing app on their store devices. The moving event is sent to the store seamlessly through their hand-held devices, indicating the status of the guest’s location as they approach the store.


I'll Come Later


: This event will trigger if a guest has decided to come to the store later, when they make that selection in their app. This will pause any movement on the trip.


I'm Here


: This event indicates the guest has arrived at the store and is in the order pickup bay awaiting their order. On the guest’s end, a selection will be made in the app to show they have arrived and then allows them to select which parking spot they are in to allow for easier identification by the team member. The store team member receives a notification on their hand-held device to indicate the arrival.


Trip Complete


: The final event confirms the order pickup on the team member’s end, closing the loop and signaling the last of the events for this trip. The guest will see a confirmation on their end, allowing them to click to verify that their items have been picked up.


What We Learned


We learned so much during the pandemic. We noticed quickly that there were changes we needed to make to ensure a safer pickup process for both guests and team members. One such change was one of necessity – the removal of the bar code scan and signature from the guest at the end of the transaction. Previously, team members would ask guests to roll down their windows to allow them to scan a unique code on the guest’s mobile device to complete the transaction (the final “event” in our API). Once COVID restrictions nationwide limited face-to-face transactions, our team realized we’d need to pivot quickly and modify the steps needed at pick-up.


At this point, we introduced the four-digit short code as a replacement to the bar code that previously needed to be scanned to allow us to validate the guest’s identity. While this addressed the safety concerns during the pandemic, the 4-digit short code also improved the security of this feature. The short code can only be generated for an authenticated guest on the Target app, and this is then directly accessed in the team member app via the Eventing API. This helped us make the guest identification process much easier, safer, and secure.


We also added the bag placement option for guests to specify where their items should be placed in their vehicles. We were able to release these new features quickly and easily due to the way our existing API was built. That flexibility has paid dividends throughout this process, allowing for quick iteration and pivots when we’ve needed them the most.


In retrospect, our decision to focus on “trips” vs. “orders” allowed us room to grow and evolve to later allow for more features such as Returns with Drive Up and Starbucks orders that we are excited to offer today. Quick iteration was essential, as was building upon the framework of events that we previously laid out. It’s important to note that we ran tests for a couple of months before even writing a single line of code. User testing with the help of Design and Research teams at Target were vital to the success of our experience, and we have the benefit of thousands of team members in the Twin Cities of Minnesota who helped us with essential feedback and testing that allowed us to gather the information we needed. Using our internal A/B testing tool Sapphire and integrating the features into the Target app, we ran different versions of the experience and treatments on both the guest and team member apps over a period of six months, iterating as we went to make the changes needed to launch the successful app you all use today.


We have team member Slack channels where we have an active ongoing discussion with internal users of our app to better meet the needs of our public guests. Team members share their experiences with Drive Up and report any steps in the process that didn’t go as smoothly as hoped, and the engineers on our team respond in real time to get more information about the reported issues and offer solutions where possible. This feedback loop is what makes us better at collaborating with our store teams in making the experience as seamless as possible for both store team members and guests alike.


What it Looks Like today


Today, Drive Up accounts for most of the orders placed in our Target app. This represents astronomical growth with no sign of slowing. Improvements continue to be made to the process and features have been added due to feedback from guests and team members. In fact, we rely heavily on the feedback we gather from guests to help guide our roadmap moving forward, sometimes even creating new features altogether based on their requests. Good examples of these added features are the new


[Drive Up with Starbucks](https://corporate.target.com/article/2023/08/starbucks-drive-up) offering where guests can order a fresh Starbucks beverage to be delivered to their vehicle with their order and being able to drop off returns at Drive Up, both of which were requested frequently by guests in our testing.


One of the most rewarding parts about being a member of Target’s technology team is the ability to work on products that we can see ourselves in near real time. It’s so validating to go shop at a store myself and see the technology at work, and to be able to gather real input from guests on how they are using the features we build. The team and I look forward to continuing our work on Drive Up to ensure that things work smoothly for our team members and guests and that we can continue to add features that respond to future needs due to the flexibility of the API foundation we’ve built.


## RELATED POSTS


### Product Engineering @ Target: Engaging With Our ‘Why'


By Nancy King, May 2, 2022


Our product engineering organization builds a large proportion of the applications that our millions of guests and 350,000+ team members use, from the mobile app and website to our point-of-sale software, to the HQ systems supporting our operations. These are the technologies that help bring Target’s purpose to life and grow our business online and across nearly 2,000 stores nationwide. The key to success is really knowing and supporting our users across the country.


### Target’s ECCO Platform: Achieving Resiliency and High Availability


By Shesadri Parthasarathy, December 1, 2022


A deep dive on Target's Enterprise Cart & Checkout (ECCO) platform - the underlying technology that powers our checkout experience across Target.com, the Target mobile app, and nearly 2,000 stores.
