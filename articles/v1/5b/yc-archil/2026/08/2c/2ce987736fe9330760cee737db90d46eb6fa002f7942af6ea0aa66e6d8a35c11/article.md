---
schema_version: "1.0.0"
document_id: "2ce987736fe9330760cee737db90d46eb6fa002f7942af6ea0aa66e6d8a35c11"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/launching-a-million-sandboxes-per-second-in-constant-time"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:03d8f254d46f705a6a1023824c88083d9a9c47bf702b31fb9b7116f6c1dc9328"
---

# Launching a million sandboxes per second in constant time

They say that in the future, everyone will have their own sandbox company for 15 minutes. Let's make that simpler, here's exactly how I built our compute platform to handle millions of sandbox launches per second in constant time. I hope that you can plug it into Fable/Mythos and get a product out, so that you too can have your own sandbox company.


## How to build for performance


There are two key properties that we're trying to achieve, and they're at odds.


First, throughput. We want to be able to launch millions of sandboxes per second. There are two key insights to achieving this property. First, you need a tremendous amount of capacity in order to absorb these sandbox launches. As a result, you cannot have a system which assumes it knows the full state of the world at any given time. Second, you clearly cannot have any point of the system serialize. Two different sandbox launches need to execute completely independently, lock-free.


Second, latency. Usually, sandbox platforms trend towards using a bunch of different caches local to the machines running the sandboxes in order to achieve startup times. For our purposes, this is a bad idea because it starts to tightly couple the placement of the sandbox to the performance of that sandbox (i.e. if you have a customer and they run one sandbox, you need to run their next sandbox on the same box in order to get good performance). The key problem with this is that it creates a situation in which your scheduler needs more information in order to function, reducing overall performance.


Let's talk about tackling these issues one-by-one.


## How to launch a million sandboxes per second


Let's first look into how we would even design a scheduler to handle a million sandboxes per second. We'll scope this part to literally just getting the sandbox to *run* on any host, and then tackle latency in the next section (understanding that sandbox locality is explicitly not a property that we want because it will cause bottlenecks).


There are two important state management primitives that we'll need for zero bottleneck schedules.


First, a database. As you may know, I am partial to Cassandra in all its forms (DynamoDB, BigTable) because I like the property that get O(~1) lookups with IOPs that trivially scale linearly with the size of the cluster that's running Cassandra. The downside here is that we need to be super careful about pre-planning our queries since we won't be able to change them later.


Second, storage. If you're not me, you could use[@archildata](https://x.com/archildata) for this, but since we have to avoid circular dependencies in our service, I'm going to just use S3 directly. This gives us scale for free, but the downside with this one is that any request to S3 can't be in the hot path because a 100ms read is going to be too slow for the sandboxes that we want to launch.


The purpose of the scheduler is to collect information about how many sandboxes are running on each host at any given time so that, at scheduling time, we can pick which location to use for placement quite quickly. This means that we want to be aggregating the capacity of each host well before we do scheduling, so let's introduce a service for that.


Assuming that we are running a cluster of 1 million runtime servers to hold all of these sandboxes, and they report their capacity every 15 seconds, we're looking at close to 70K requests per second. High, but certainly achievable.


We want to store each host's capacity inside of our Cassandra so it's easy to look up later when we want to place something on the host. We want to use a primary key for our Cassandra database that has good uniformity over the search space -- so let's assume that we need to map each host's IP address to some kind of random, unique identifier like a UUID.


We should have our capacity aggregation service own uploading this mapping into something like S3. Assuming that we have 64-bit server identifiers and 32-bit IP addresses, this mapping should be on the order of 12 MB. Again, large, but certainly something that we can do -- and we can split it into different pages if we need to.


We'll have our Capacity Aggregation service also be responsible for new host registration, so we just add a host to the fleet, it starts reporting capacity, and our service add its to Cassandra. As a result, we need one of our "Capacity service" hosts (some kind of leader election will have to happen) to be responsible for periodically scanning Cassandra and putting the mapping of UUID to IP addresses into S3, but because host turnover is slow, this can happen on the order of minutes. Similarly, we don't have to immediately detect dead hosts instantly so long as we have a low probability of selecting them, so let's assume that there's some slow process in this server that scans our Cassandra table to look for servers which haven't sent capacity in a while and removes them.


The great part about centralizing this into a single service is that it can also be responsible for a bunch of other functions we might want, like quiescing a host that's alive so that we can deploy to it without disrupting customer workloads.


Let's look at the actual host selection path:


People give microservices a bad rap, but it's a super simple way to build something that will scale forever, and this is probably less than 10K lines of code (not that we care about that anymore).


Our API service will be, in the background, periodically (O(minutes)) refreshing the list of hosts from our mapping file stored in S3. This will give us a slightly out-of-date view of which hosts are alive (you will note that "slightly out of date" is a common theme).


When a customer requests a sandbox, we can now select from this list with uniform randomness. We will pick 2 at random (or whatever "best-of-X" you prefer), and then query Cassandra for both of those hosts (let's assume ~10ms) to get their slightly-out-of-date capacity information. We'll select the host with the most capacity and attempt to put the sandbox on that host. If this fails (either for capacity reasons or for health reasons), we will report the failure to the capacity service to take the appropriate action (either update or remove the host). If it fails, we'll try the other host we selected, or pick again.


This gives us a 10ms scheduling p50, and a slightly higher p99 depending on how frequently we pick bad hosts. Depending on your goals as a platform, you can tune the probability of picking a bad host by: selecting more or fewer hosts to query, updating the capacity information more or less frequently, and potentially sandbagging capacity by always reporting values that are lower than the actual host capacity.


There's nothing in this path that has any kind of locking out, so we can now launch one million sandboxes per second in around 10ms. What's next?


## How to launch sandboxes in constant time


The next usual challenge here is how we do this in a way that doesn't have terrible cold-start latencies depending on the placement. We just built a placement service that doesn't consider any constraints on the host that we pick, so we need to make sure that we can get the sandbox up in the same amount of time no matter what host it lands on.


Actually booting the sandbox usually isn't a problem because you can get constant time trivially by just always booting them. On the other end of the spectrum, you can do what[@archildata](https://x.com/archildata) does and have every sandbox that the platform could run already prebooted, and lock customers into a few different shapes of choices (which we can signal to the schedule by reporting more complex capacity information in our reports). The reader can decide which way they want go here.


In fact, the usual problem with a sandbox platform is actually the image that the user wants to use on their host. Users aren't very good at building their workloads around the constraints of the system, so as much as we all wish that all of our container images were like 10 MB, it turns out that most container images in production are more on the order of 500 MB to 2 GB.


Now, this poses a problem. The usual way that container platforms are built is that the user will "prepare" or upload their container image into some kind of a registry. This is usually S3-backed storage. When they launch the container on the host, then the host will actually pull down the image to a local cache.


This makes warm startups (scheduling the same container on the same host) quite fast (the image is already there), but it makes cold starts terrible. If you download from S3 naively (80 MB/s) it can take nearly 25 seconds to get a 2 GB image downloaded to the runtime host. This is not great for consistency when a warm startup can do this in 0 seconds.


As a result, many systems end up trying to do image-aware placement to maximize the chances that the user's container lands on a host that has already done this download. This, of course, introduces bottlenecks into the placement process which ends up limiting the speed at which the system can launch containers.


Worse, most container launches don't actually need to read 100% of the image bytes to actually startup, so a lazy-loading approach could significantly improve these cold-start times.


Lucky for us, in 2026, we can use high-speed file storage (like[@archildata](https://x.com/archildata) ) to just host the images for us, because they provide online access to the data from all of these hosts without needing a download step. In this world, we put all of the container images that customers want (via a "preparation step") onto a file system that they already own.


Unlike the capacity management (which is an issue for operating the sandbox service) we do get to use Archil for image storage because we think of those images as belonging to each user, which means that they can use the Archil disks that they already have to hold the images.


Because this is disaggregated storage, all 1 million hosts in the service are able to access the data at the same speed and read only the bytes they need. Even though the sandboxes are already booted, we can easily tell Linux to swap into the container namespace from the disaggregated storage system as part of the "launch" process.


This means that we now can launch any container, on any host, in the same amount of time, regardless of the image size that the container wants to use.


Let's look at the entire architecture diagram.


As you probably realize, this isn't actually a complex thing to build. I think that the core reason why "scheduling" gets a bad rap about being difficult is for a lot of random reasons:


- I think that people are used to thinking about using Kubernetes for problems, which isn't singularly designed for the task of placement and scaling containers
- I suspect that many system designs rely on relational databases that are more challenging to scale, out of the box, which run into issues
- I believe that many schedulers being built today (especially given the race to build sandboxes) are likely trying to do fully-consistent capacity management, which isn't possible at high levels of scale
- People don't immediately reach for online shared storage like[@archildata](https://x.com/archildata) , instead opting to try to upload+download things from places like S3 (slow)


If you want any of the code to prepare Docker images into an efficient format to store on Archil (or other file storage) or the code to "launch" that Docker image (w/o installing Docker), then please DM me and I'll shoot it your way.


Anyway, I hope that you go off and build a sandbox company with this blueprint. Obviously, there's lots more that has to happen besides just getting a container running on a host using a Docker image, but I think that everything else should be relatively straightforward for you and Fable.


Please let me know if you feed this to your Claude and something pops out!
