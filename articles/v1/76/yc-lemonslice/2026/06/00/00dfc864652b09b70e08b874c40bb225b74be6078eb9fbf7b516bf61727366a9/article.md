---
schema_version: "1.0.0"
document_id: "00dfc864652b09b70e08b874c40bb225b74be6078eb9fbf7b516bf61727366a9"
company_key: "yc-lemonslice"
company: "LemonSlice"
source_id: "yc-lemonslice-news-import-9806857f7d31"
canonical_url: "https://lemonslice.com/blog/connection-time"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-24T02:14:03.501451+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:c5bc783d82d7c34c089c78f114c55481f05fc3f2b9d88cbcd34c1536aa23586e"
---

# How we reduced avatar connection time by 76%

Our interactive avatars are real-time once a conversation begins. Every pixel is generated from scratch at 20fps: the lip sync, the face, the hand gestures, and even the background are all generated in real-time in response to what a human says. However, before the conversation begins, there is what we call the avatar connection time. You can think about this like the “ringing” time on a phone call: the time between you pressing “Call” and the person picking up on the other side.


In the past few weeks, we’ve *dramatically* reduced our avatar connection time. While the median connection time nearly halved (it’s now <3 seconds), the biggest improvement came from eliminating long-tail latency. Our P99 dropped by more than 28 seconds.


This is important because every additional second before an avatar becomes active increases the likelihood that users will abandon the session before the conversation even begins.


Here are the new connection time numbers and how we achieved these improvements.


## Optimizing the Pipeline’s Initialization


Reducing connection latency wasn't about finding a single bottleneck. Instead, we identified several independent sources of delay throughout the initialization. Some only affected a small percentage of requests and created long-tail latency, while others added unnecessary time to every connection.


#### Eliminating Slow Initial Video Generations


In rare situations, the initial video generation for a session could take 5–7 seconds. We traced these slow generations to cases where our VAE had lost its warmed-up state, forcing expensive initialization work to occur during a user's session.


Rather than allowing users to encounter these cold generations, we identified the situations that could invalidate the VAE and proactively re-warmed it before the GPU was returned to the scheduling pool. This ensured users consistently received low-latency initial video generation. We saw a dramatic stabilization in our initial generation times:


#### Redefining "GPU Ready"


During GPU initialization, a GPU was sometimes marked as "ready" before it was actually capable of serving inference. Although the container had started, there was still initialization work remaining before it could generate video with low latency.


We adopted a much stricter definition of "ready" and only added the container to the scheduling pool after all initialization and warmup had completed. This prevented users from being routed to GPUs that were technically running but not yet ready to serve requests.


### Parallelizing Independent Work


A recurring theme throughout our optimization effort was reducing the critical path by identifying work that could happen in parallel. This became a guiding principle throughout our connection pipeline: if two tasks don't depend on each other, they shouldn't wait on each other.


Every avatar connection performs two major tasks: preparing the avatar image for inference and establishing the WebRTC connection. Both involve network requests and initialization work, but neither depends on the other. By running them in parallel, we eliminated unnecessary idle time without changing any underlying components.


We applied the same idea to our recently launched avatar actions. Some avatars require a large number of actions to be initialized on the GPU before they can be used.


To address this, we first parallelized action initialization instead of processing each action sequentially. This significantly reduced the time required for avatars with many actions. We then moved action initialization into a background thread so the avatar no longer had to wait for every action to be ready before connecting. Users can now begin interacting with their avatar immediately while the remaining actions continue initializing in the background.


## Where do we go from here?


Today, the two largest remaining contributors to connection latency are outside of our application logic.


The first is waiting for a warm GPU during periods of high demand. On average, routing a request to a warm GPU currently takes **0.93 seconds** , making it one of the largest remaining components of the overall connection process. Here is our current distribution of GPU allocation time:


The second is establishing the WebRTC connection. This stage depends heavily on network conditions and user integrations, introducing additional variation in connection time. Looking at the time from GPU allocation to the first generated video frame, we can clearly see the long tail created by slower WebRTC connection setups:


We've reached the point where most of the remaining connection time comes from 3rd party dependencies rather than avatar initialization itself. That's an exciting milestone! However, we will continue to improve our pipeline and work with WebRTC providers to further reduce this connection time as much as possible.


Through user testing, we also learned that perceived performance is just as important as actual performance. Users were surprisingly tolerant of waiting up to 10 seconds for a connection, provided the interface clearly communicated what was happening. A ringing sound or animated call indicator sets the expectation that another participant is joining, making the wait feel natural instead of broken.


## Appendix


The chart below summarizes the improvement in connection time across each percentile before and after these changes:


Percentile Old (s) New (s) Improvement (s) Improvement (%)


Median 5.4 2.9 2.5 46.3%


P75 7.4 3.5 3.9 52.7%


P90 13.2 5.0 8.2 62.1%


P95 21.5 6.1 15.4 71.6%


P99 37.8 9.0 28.8 76.2%
