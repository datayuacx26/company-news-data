---
schema_version: "1.0.0"
document_id: "b2991ad46acbf8e4ef58b8e2963c3541c9582ac8c344b5219e55df44968a7cf6"
company_key: "yc-icepanel"
company: "IcePanel"
source_id: "yc-icepanel-news-import-9cf2a09ec197"
canonical_url: "https://icepanel.io/blog/2026-07-29-design-strava-using-icepanel"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-28T18:12:49.767958+00:00"
fetched_at: "2026-07-29T00:00:01.007588+00:00"
content_hash: "sha256:2d210cd125afaf7e1a8da61ace1b667c3ee9b50ad62365560261418804a9555b"
---

# Design Strava using IcePanel

## 📝 Introduction


In this post, we’ll design **Strava** , a fitness tracking app that allows users to record and share their physical activities (e.g., running) with their friends. We’ll approach this problem as architects and design four hierarchical diagrams: Context, Container, Component, and Code (not familiar with these? Read[this](https://icepanel.io/blog/2024-07-18-what-is-the-c4-model) ).


Each diagram will be annotated to explain the key building blocks and responsibilities, and we’ll highlight how users interact with the system by visualising data flow using[IcePanel Flows](https://docs.icepanel.io/visual-storytelling/flows) .


We’ll start by defining the scope of the system through its functional requirements (what it should do) and non-functional requirements (the qualities it should have). From there, we’ll progressively go through each layer of the C4 model to build the overall architecture.


You can view the final architecture here:[https://s.icepanel.io/DWnaysJ3cbCQqg/rveR](https://s.icepanel.io/DWnaysJ3cbCQqg/rveR)


---


## 🔎 Scope


Strava is a popular fitness tracking app used by millions of runners and cyclists worldwide. It is also a social platform that has user feeds and regular events for users interested in participating.


For the scope of this post, we’ll narrow down the system to three functional requirements:


1. Users should be able to start, pause, stop, and save their activities (i.e., runs).
2. Users should be able to view their activity data (route, distance, and time).
3. Users should be able to view their completed activities and their friends’ activities in a feed.


For a popular app like Strava, the system should have these non-functional requirements


- Availability >> consistency. The system should expect up to 10 million daily active users.
- The app should work with low or no internet connection (e.g., runners in remote areas need offline GPS tracking)
- The app should provide real-time stats during the activity (distance, elapsed time, etc).
- The app should scale to handle up to 10 million concurrent activities during peak hours (e.g., weekend mornings).


Let’s start with the first diagram, Context.


---


## Level 1 - Context


The Context layer defines the actors and systems Strava depends on. At this level, we treat Strava as a single system. However, the internal breakdown of mobile app vs backend comes at the Container level. For the Context layer, we have one actor called Runner. Runner is an end user who records activities (runs) through the Strava app.


We also have external systems that Strava depends on:


1. **Map Provider ([Mapbox](https://www.mapbox.com/) ):** Provides map tiles and route rendering for displaying activity routes through the Strava mobile app.
2. **Identity Provider ([Auth0](https://auth0.com/) ):** Authenticates users via OAuth 2.0 and social login.


There are usually more external systems that integrate with Strava (e.g., wearable device APIs like[Apple HealthKit](https://developer.apple.com/health-fitness/) for apple watch) but we’ll leave them outside of scope for this post.


The more detailed design comes next, which is the Container diagram.


---


## Level 2 - Container


The Container layer zooms into the Strava system to show the services, apps, and data stores inside. A key architectural decision in this design is that most of the heavy lifting happens on the client (mobile app), not the backend. The client handles real-time GPS tracking, metric calculation, and offline support on-device. This means the backend can stay simple since it only receives completed activity data after a run is saved.


We don’t need a microservices architecture for this particular problem. Only 2 web servers (activity and feed builder) in the backend for handling activity data and user feed. We’ll talk more about the Components inside the mobile app as you’ll see in the deep dive below.


The Strava system contains the following containers:


**Client**


- **Strava App (iOS/Android):** A fitness tracking app for recording and sharing physical activities. The app tracks GPS locally, computes real-time metrics on-device, and syncs completed activities with the backend. The client-side app connects to the Identity Provider for user authentication and the Map Provider for route display.


**Backend**


- **API Gateway ([AWS API Gateway](https://aws.amazon.com/api-gateway/) ):** Routes incoming requests and enforces authentication and rate limiting.
- **Activity Server:** Processes activity uploads, stores data in PostgreSQL, and updates leaderboard scores in Redis.
- [SQS](https://aws.amazon.com/sqs/) **:** A message queue to decouple activity writes from asynchronous feed generation. When a new activity is saved, the Activity Server publishes an event. The Feed Builder picks it up asynchronously, calculates the user’s feed, and caches it for later.
- **Feed Builder:** Builds and serves friend feeds by polling SQS for new activities and caching results in Redis. The server has two entry points: serving synchronous feed requests from users, and processing async[fan-out from SQS](https://aws.amazon.com/blogs/compute/messaging-fanout-pattern-for-serverless-architectures-using-amazon-sns/) .
- **Activity DB ([PostgreSQL](https://aws.amazon.com/rds/postgresql/) ):** Stores users, activities, routes, and followers/followees.
- **Redis Cache ([Redis](https://redis.io/open-source/) ):** Caches friend feeds and maintains real-time leaderboards using[Sorted Sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/) .


On a high level, the system works as follows.


**Write path:** The user starts an activity. They record their GPS coordinates through their devices and cache them on the local storage temporarily. The Strava App uploads the complete GPS dataset to the API Gateway. The API Gateway routes it to the Activity Server, which stores the activity and route data in PostgreSQL, updates leaderboard Sorted Sets in Redis, and publishes a “new activity” event to SQS. The Feed Builder picks up the event, queries PostgreSQL for the user’s follower list, and writes the activity into each follower’s cached feed in Redis.


**Read path:** The user opens their feed. The API Gateway routes the request to the Feed Builder, which checks Redis for a cached feed. On a cache hit, it returns immediately. On a miss, it queries PostgreSQL, builds the feed, caches it in Redis, and returns the results.


We’ve designed two primary data flows using IcePanel. Check out these flows and play them step by step to see how our system works.


**Flow 1: Start and save an activity**
Complete flow:[https://s.icepanel.io/DWnaysJ3cbCQqg/vkVU](https://s.icepanel.io/DWnaysJ3cbCQqg/vkVU)


1. Runner starts an activity. Activity Manager triggers GPS Module.
2. GPS Module captures coordinates and stores them in Local Storage every ~10 seconds.
3. Metrics Module computes live stats (distance, pace, time) on-device from GPS data.
4. Runner saves the activity. Activity Sync reads the complete dataset from Local Storage.
5. Activity Sync uploads the GPS data via HTTP POST through API Gateway to Activity Server.
6. Activity Server writes the activity and route data to Activity DB and updates leaderboard Sorted Sets in Redis.
7. Activity Server publishes a “new activity” event to SQS.
8. Feed Builder picks up the event, fetches the follower list from Activity DB, and writes the activity into each follower’s cached feed in Redis.


**Flow 2: View leaderboard**
Complete flow:[https://s.icepanel.io/DWnaysJ3cbCQqg/7WZF](https://s.icepanel.io/DWnaysJ3cbCQqg/7WZF)


1. Runner opens the leaderboard page in the Strava App.
2. Activity Manager sends a GET request with query filters to Strava Backend.
3. API Gateway routes the request to Activity Server.
4. Activity API delegates to Leaderboard Manager.
5. Leaderboard Manager queries the appropriate Redis Sorted Set (e.g., leaderboard:run:CA).
6. Redis returns the top N ranked users.
7. Activity Server returns the leaderboard data through API Gateway.
8. Strava App renders the leaderboard.


### Offline-first and GPS tracking


A key architectural consideration is offline support. Runners and cyclists frequently lose connectivity during activities (trails, rural roads, tunnels). The mobile app is designed to be offline-first for activity recording. GPS coordinates are captured and stored locally using the phone’s GPS hardware, which operates independently of network connectivity. All real-time metrics are calculated on-device. The upload only happens after the activity is saved and the device has connectivity.


The beauty of this design is that a complex backend isn’t needed. Modern smartphones have sensors, processing power, and local storage. For a system like Strava, the client plays a critical role in the overall design, enabling offline functionality, reducing server load, and improving user experience. The backend is primarily responsible for serving and storing data!


---


## Level 3 - Component


In the C4 model, a component is a grouping of related modules encapsulated behind a well-defined interface. Let’s start with the components inside the Strava App. We’ll use iOS ([Swift](https://developer.apple.com/swift/) ) for our examples, but the same concepts apply to Android.


### Strava App (iOS/Android)


1. **Activity Manager:** Coordinates the activity lifecycle and connects to GPS, metrics, auth, and map services.
2. **GPS Module:** Captures GPS coordinates at regular intervals using device location services ([Core Location](https://developer.apple.com/documentation/corelocation) on iOS,[FusedLocationProviderClient](https://developers.google.com/location-context/fused-location-provider) on Android). Sends coordinates to Local Storage and to the Metrics Module.
3. **Metrics Module:** Computes live stats on-device: distance, pace, and elapsed time from GPS data. Fetches GPS data from Local Storage and returns live metrics to the Activity Manager for display.
4. **Local Storage ([SQLite](https://www.sqlite.org/) ):** Persists GPS coordinates locally every ~10 seconds to avoid data loss.
5. **Activity Sync:** Uploads completed activity data to the Strava backend when connectivity is available. The module reads the full GPS dataset from Local Storage and uploads it to the backend.


### Activity Server


The Activity Server handles the write path. Let’s look at its internal components.


1. **Activity API:** Receives activity uploads, coordinates storage, and triggers leaderboard and feed updates.
2. **Database Client:** Reads and writes user activities, routes, and GPS data to PostgreSQL.
3. **SQS Client:** Publishes new activity events to SQS for async feed generation.
4. **Leaderboard Manager:** Updates and queries leaderboard scores using Redis Sorted Sets.


### Feed Builder


The Feed Builder handles the read path and async fan-out. It has two entry points: synchronous feed requests from users, and async events from SQS.


1. **Feed API:** Handles synchronous feed requests from users and delegates to the Feed Repository.
2. **SQS Consumer:** Polls SQS for new activity events and triggers fan-out to follower feeds.
3. **Feed Repository:** Fetches the user’s follower list and assembles the activity feed.
4. **Redis Client:** Reads and writes cached friend feeds to Redis.
5. **Database Client:** Queries follower/followee relationships and activity data from PostgreSQL.


Let’s go one level deeper with the source code in the Code layer.


---


## Level 4 - Code


This is where we can view implementation details at the code level. We’ll go over two components: Activity Server and Feed Builder.


### Code Classes in Activity Server


This component handles all RESTful endpoints for activity management.


**Activity API**


- POST /v1/activities - Upload a completed activity (full GPS dataset)
- GET /v1/activities/:activityId - Retrieve full activity details including route
- GET /v1/activities?mode={USER|FRIENDS}&page&pageSize - Paginated activity list
- PATCH /v1/activities/:activityId - Update activity metadata


**Redis Client**


- incrementScore(key, userId, distance) - Updates user’s total distance in a sorted set.
- getTopN(key, n) - Returns top N users from a sorted set (for leaderboards).
- cacheActivity(activityId, data, ttl) - Caches activity data with expiry.


### Code Classes in Feed Builder


This component builds and serves friend feeds by polling SQS for new activities and caching results in Redis.


**Feed API**


- GET /v1/feed?userId&page&pageSize - Retrieve paginated friend feed for a user


**Redis Client**


- **getCachedFeed(userId)** - Returns cached feed if available.
- **setCachedFeed(userId, feed, ttl)** - Caches a built feed with time-to-live expiry.
- **appendToFeed(userId, activity)** - Pushes a new activity into a follower’s cached feed.


Database Client


- **getFollowers(userId)** - Returns the user’s follower list.
- **getActivities(userIds, page, pageSize)** - Queries activities for a list of users.


And these are some examples of interfaces that highlight the interactions within a Component in terms of code.


---


## Conclusion


In this post, we designed a fitness tracking system like Strava using the C4 model on IcePanel. We began with the core requirements and modelled the system from the top down, starting with the Context layer, followed by the Container, Component, and finally the Code layer.


If you’re interested in more design deep dives, check out:


- [https://icepanel.io/blog/2026-03-03-design-metrics-and-alerting-system-using-icepanel](https://icepanel.io/blog/2026-03-03-design-metrics-and-alerting-system-using-icepanel)
- [https://icepanel.io/blog/2026-01-28-design-chatgpt-with-icepanel](https://icepanel.io/blog/2026-01-28-design-chatgpt-with-icepanel)
- [https://icepanel.io/blog/2025-12-18-design-ebay-using-icepanel](https://icepanel.io/blog/2025-12-18-design-ebay-using-icepanel)
- [https://icepanel.io/blog/2025-10-20-design-ticketmaster-using-icepanel](https://icepanel.io/blog/2025-10-20-design-ticketmaster-using-icepanel)
- [https://icepanel.io/blog/2025-11-18-design-youtube-using-icepanel](https://icepanel.io/blog/2025-11-18-design-youtube-using-icepanel)


Let us know which system you’d like to see modeled next on IcePanel!


## 📚 Resources


- [https://www.strava.com/](https://www.strava.com/)
- [https://aws.amazon.com/api-gateway](https://aws.amazon.com/api-gateway/)
- [https://aws.amazon.com/rds/postgresql/](https://aws.amazon.com/rds/postgresql/)
- [https://aws.amazon.com/sqs/](https://aws.amazon.com/sqs/)
- [https://redis.io/open-source](https://redis.io/open-source/)
- [https://www.mapbox.com/](https://www.mapbox.com/)
- [https://developer.apple.com/health-fitness/](https://developer.apple.com/health-fitness/)
- [https://developer.apple.com/documentation/corelocation](https://developer.apple.com/documentation/corelocation)
- [https://developers.google.com/location-context/fused-location-provider](https://developers.google.com/location-context/fused-location-provider)
- [https://developer.apple.com/documentation/coredata](https://developer.apple.com/documentation/coredata)
- [https://redis.io/docs/latest/develop/data-types/sorted-sets/](https://redis.io/docs/latest/develop/data-types/sorted-sets/)
- [https://developer.apple.com/swift/](https://developer.apple.com/swift/)
