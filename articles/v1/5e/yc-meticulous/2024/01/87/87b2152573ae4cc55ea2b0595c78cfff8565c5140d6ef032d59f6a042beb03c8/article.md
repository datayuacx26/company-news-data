---
schema_version: "1.0.0"
document_id: "87b2152573ae4cc55ea2b0595c78cfff8565c5140d6ef032d59f6a042beb03c8"
company_key: "yc-meticulous"
company: "Meticulous"
source_id: "yc-meticulous-news-import-624e920f0172"
canonical_url: "https://www.meticulous.ai/blog/let-users-write-tests-for-you"
published_at: "2024-01-16T00:00:00+00:00"
first_seen_at: "2026-07-25T14:57:24.607504+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:51c8d05fb4c40cbda0f6fe44a5c72ee462e4aa1745dbf0738a9dc7d60e39e652"
---

# Let Your Users Write Your Tests for You | Part I

## Replay Testing


This blog post introduces the concept of 'replay testing' and why it can be a valuable tool in a software engineer’s toolbox.


During my time as a software engineer, previously at Opendoor and Dropbox, I discovered that however much you unit or integration test your code, you will tend to find that actual users of your application find a way to break it.


So we write tests. Lots and lots of them. Writing tests require you to simulate user behavior and write assertions around how an application should behave. This poses two problems. The first is that we can only write test cases for scenarios that we can think of. This makes it tricky to cover all edge cases, and is particularly difficult when an application is complex. The second problem, particularly pernicious with[end-to-end tests](https://www.meticulous.ai/blog/testing-pyramid-for-frontend) , is that every test carries a maintenance burden.


This is where replay testing comes in. Replay testing simulates what users’ actually do on your web application. When testing a given change, a user session is replayed on two versions of your web application: base commit and head commit. Before and after.


Suppose you want to detect new uncaught exceptions. During replay, the application is monitored and we can listen for uncaught exceptions. These can then be diffed in order to detect regressions. This moves from an assertion/expectation testing model towards surfacing differences for developers to approve/reject. Another example might be capturing screenshots and diffing these to detect visual regressions.


Replay testing can avoid these pitfalls, but it’s complex to build.


## Why has this not been Done Before and Why is it Tricky


It has. However, only large companies have the resources to sponsor a sufficiently large engineering lift. Twilio invested heavily into implementing replay testing in the early 2010s. Google has a system still-in-use today which takes visual snapshots and diffs these. However, most companies are left high and dry.


Implementing replay testing requires solving many intricate problems. Let’s explore two example problems. The first problem is that replaying a session on a production environment will cause side effects.


In order to overcome this, companies will often spin up a staging environment to replay against. However, building a staging environment that is representative of production is a tricky problem in and of itself. In addition, companies must implement a mechanism to reset state after each replayed session(imagine replaying a user signing up to your web application).


There are many more problems than this, but just solving the ones above require significant investment!


At[Meticulous](https://meticulous.ai/) we capture all network traffic at record time and then automatically mock out all network calls at replay-time. This isolates the frontend, avoids causing any side effects and allows any company to implement replay testing for their frontend code. This approach is not as powerful as full-stack replay against a representative staging environment - it only catches frontend regressions. However, it works out of the box and is a valuable additional layer of defense that you get, more or less, for free.


I will add more detail on how exactly this works and what other issues can crop up when implementing replay testing in part 2. Thanks for reading


Replay testing is what we’re building at Meticulous, as part of our mission to work towards making the world’s code safe, performant and reliable.


Are you interested in working on this problem or trying out Meticulous?


Email me at gabe@ this domain and mention ‘blog’ in the subject line. We’re backed by some of the best technical investors in the world, like the CEO & founder of Vercel, CTO of GitHub, CPO of Adobe, YC partner and Scribd CTO Jared Friedman and many other incredible folks.


If you’ve implemented replay testing at your company, please reach out - I’d love to learn more!


## Meticulous


Meticulous creates and maintains an exhaustive suite of e2e ui tests with **zero** developer effort.


This quote from the CTO of Traba sums the product up best: "Meticulous has fundamentally changed the way we approach frontend testing in our web applications, fully eliminating the need to write any frontend tests. The software gives us confidence that every change will be completely regression tested, allowing us to ship more quickly with significantly fewer bugs in our code. The platform is easy to use and reduces the barrier to entry for backend-focused devs to contribute to our frontend codebase."


This[post](https://www.meticulous.ai/blog/lessons-from-a-decade) from our CTO (formerly lead of Palantir's main engineering group) sets out the context of why exhaustive testing can double engineering velocity. Learn more about the product[here.](https://www.meticulous.ai/)


Authored by Gabriel Harper, founder of Meticulous
