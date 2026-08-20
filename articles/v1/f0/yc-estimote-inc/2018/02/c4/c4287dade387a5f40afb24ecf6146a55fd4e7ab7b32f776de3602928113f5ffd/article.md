---
schema_version: "1.0.0"
document_id: "c4287dade387a5f40afb24ecf6146a55fd4e7ab7b32f776de3602928113f5ffd"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/168674042150"
published_at: "2018-02-14T13:06:23+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:57769c716979e79019310b0bc524dee35e018a9282c8a85d27a46ea1bc697504"
---

# Estimote Cloud Attachments API: store context in the cloud, less in your app!

In 2016, we introduced the[Beacon Storage API](http://blog.estimote.com/post/151748617310/our-latest-innovation-beacon-storage-api) allowing developers the opportunity to store small amounts of data within the built-in memory of each Estimote Location Beacon. This feature lets you share data with nearby customers, and display text or images coming directly from the beacon. It’s a perfect solution where internet connectivity is spotty and you don’t need to store a lot of data. An alternative is to have your app either include that data, or fetch it remotely from the cloud. That means either hard-coding the info in the app—which doesn’t scale—or relying on an external service to store it. We’re constantly looking to improve the experience for our developers, shorten the development time, and make maintaining apps a breeze. Today we’re helping with that: introducing our Estimote Cloud Attachments API.


If you haven’t used external cloud services (e.g. Firebase or Deployd) or your own backend before, you can use this API to associate your own data with a beacon, and then use this contextual information to enhance the experience for your users. Embedding Cloud Attachments API service could also decrease the complexity of your application — fewer dependencies are always better. It can also save you time by maintaining smaller number of external services. Another advantage of using Cloud Attachments API is that you don’t need to hardcode data in your app anymore. It helps with maintenance and updates.


Here is a snippet of code showing how to use attachments:


```text
// create an attachment
curl -X POST 'https://cloud.estimote.com/v3/attachments' \
-u YOUR_APP_ID:YOUR_APP_TOKEN -H "Content-Type: application/json" \
-d '{"data": {"payload": {"key1": "1","key2": "text"},
"identifier": "YOUR_DEVICE_IDENTIFIER"}}'


// fetch an attachment
curl -X GET 'https://cloud.estimote.com/v3/attachments/ATTACHMENT_ID' \
-u YOUR_SDK_APP_ID:YOUR_SDK_APP_TOKEN -H "Accept: application/json"


```


If you want to utilize them inside Estimote Proximity SDK take a look at code snippets for[iOS](https://gist.github.com/bartlomiejolma/13197fecb86ce498b8e8142b3a0b783f) and[Android](https://gist.github.com/bartlomiejolma/e2070ecf106016a7b91cd8db1a07e42d)


We’re often asked why Estimote hasn’t built our own beacon CMS (Content Management System). The answer is simple: with such a vast landscape of use-cases, we cannot create a service which will be perfectly suited for all our users. Therefore we believe that it’s better to deliver similar services as flexible APIs. This way our customers can use their domain-specific expertise to create their own magical experiences.


If you’d like to create your own specialized beacon CMS, you can use[Estimote Proximity SDK](http://blog.estimote.com/post/166007374930/the-most-reliable-proximity-tech-now-with-a) and Cloud Attachments API to do so. However, if you’re just prototyping, we’ve also added a new section to the Beacon Settings page in our Cloud where you can find the data currently assigned to a given beacon. There is also a simple editor which helps you to easily add, change, and remove existing information, and best of all, **you won’t even need to connect to physical devices to apply the updates.**


You may be wondering how you can use this new feature. Just imagine you are responsible for exhibition content in a museum. Typically, in order to get some detailed information, your visitors would need to come close to the paintings to read the small print on a panel. That’s not necessarily a problem when the venue is empty, but what’s that experience like when the place is crowded? Not great! To make it better, you can place a beacon by the painting, then your team can create a mobile app and use the Estimote Attachments Cloud API. When your visitors are close to the art piece, their smartphones will know which paintings are nearby (thanks to[Estimote Proximity SDK](http://blog.estimote.com/post/166007374930/the-most-reliable-proximity-tech-now-with-a) ), and the app will fetch the data associated with this beacon to show them context-based information. Of course, that could be a more detailed description of the painting, but it could also include a thumbnail, or even a link to a video depicting an x-ray of the multiple layers built up.


Estimote Attachments Cloud API is available today to all our users! You only need to log in to[Estimote Cloud](https://cloud.estimote.com/) or if you prefer to use our API, just[check out our documentation](https://cloud.estimote.com/docs) . Happy hacking!
