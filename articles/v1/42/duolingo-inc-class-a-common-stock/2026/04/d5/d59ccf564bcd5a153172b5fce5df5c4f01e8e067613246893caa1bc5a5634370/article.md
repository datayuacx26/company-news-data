---
schema_version: "1.0.0"
document_id: "d59ccf564bcd5a153172b5fce5df5c4f01e8e067613246893caa1bc5a5634370"
company_key: "duolingo-inc-class-a-common-stock"
company: "Duolingo Inc."
source_id: "duolingo-inc-class-a-common-stock-news-import-1f1e82a2742f"
canonical_url: "https://blog.duolingo.com/frontend-prediction/"
published_at: "2026-04-15T12:00:53+00:00"
first_seen_at: "2026-07-25T02:01:49.609700+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:52c7168eefa175e998b85825b25ef008ba95b6cec72744bfb1dc5de59559b2e1"
---

# Seeing the future: frontend prediction in Duolingo’s mobile app

At Duolingo, we constantly explore ways to make our app feel faster, more intuitive, and resilient to poor connectivity. One strategy we rely on is **frontend prediction** —a technique where the app updates the UI based on a prediction of what the backend response will be. When used thoughtfully, it provides significant benefits in reducing perceived latency and enabling offline functionality. However, it is not without its costs.


In this post, we’ll explore what frontend prediction looks like in practice, walk through real examples from the app, and share the tradeoffs we’ve encountered along the way.


## What is frontend prediction?


Frontend prediction—sometimes referred to as **optimistic updates** —is when the app immediately updates state and UI based on an anticipated backend result. This approach is used in many places throughout Duolingo, especially where we need offline support or want to minimize user-perceived delays.


While simple in theory, implementing frontend prediction requires careful thinking about state synchronization, conflict resolution, and rollback strategies. We’ll walk through these challenges using three main examples: **course progress, leaderboards,** and **follow buttons** .


Note: there is no standard term for this in the industry or at Duolingo, so I will opt to use “frontend prediction” for this post.


## Example 1: course progress


Let’s start with a relatively simple feature: tracking course progress along the learning path. In other words, how many lessons has the user done in their course, and what their next lesson should be.


In its most basic implementation, each time a user completes a session, the frontend sends a network request to the backend, which processes the session, increments a session counter, and returns the new value. The frontend then updates the UI accordingly.


### The problem with this approach


This works well—until the user goes offline. If the user isn’t connected to the internet, the network request fails, the backend isn’t updated, and the frontend can’t reflect the new progress. This is a poor user experience, especially since sessions are designed to work offline.


**The app is supposed to work when the user is offline.**


### Supporting offline progress


To address this, we implemented frontend prediction: when a session completes, the frontend **immediately** increments its copy of the course progress state and updates the UI—even while offline. When connectivity resumes, the app sends the stored sessions to the backend.


However, this introduces **state duplication** . Both frontend and backend are responsible for incrementing the counter.


While this sounds simple, there are now many more failure cases to consider:


- What happens if the PUT /sessions fails?
- What happens if it succeeds, but the returned response is new_course_progress=402?
- What if it’s 398 instead?


Furthermore, there is **logic duplication** as well. While the current logic (increment the counter by 1) looks simple, future product changes could complicate things. For example, if the spec later requires that users who perform poorly must restart a node, both frontend and backend need to implement that same business logic. This creates potential for bugs, inconsistencies, and headaches in versioning and experimentation.


## Example 2: leaderboards


Frontend prediction becomes more interesting in the context of leaderboards.


### The flow


In leaderboards, users are grouped into cohorts and earn XP by completing sessions. Their ranking is visible in the Leaderboards tab, and after each session, a session end card informs them of their new standing.


The backend is responsible for computing XP earned in a session based on several variables: number of correct challenges, bonus from in-lesson streak, XP boosts, and more. As before, we could have the frontend wait for the backend response.


### Predicting XP and rankings


However, this introduces too much latency between when a user completes a session and when they start seeing session end cards.


To improve responsiveness, we compute the expected XP on the frontend while the request is in flight. This lets us show the session end card immediately—often several seconds before the backend finishes.


But this comes with the same caveat as before: The **predicted XP** might be wrong.


### Why conflicts happen


Inconsistencies can arise due to:


- **Grading discrepancies:** The frontend might think the user answered 12 questions correctly, while the backend counts 11.
- **State mismatches:** The frontend may think an XP boost is active, while the backend disagrees.


In this example, there is a lot more business logic we have to keep in sync between the frontend and backend.


### Rollback strategies


So how do we resolve differences between predicted and actual values? Here are a few options for thought:


1. **Never roll back:** Risky. A user might see “You’re #1!” but later be demoted—an infuriating experience.
2. **Immediate rollback:** Use predicted XP for the session end card, but correct it in the Leaderboards tab. This could create a jarring UI discrepancy.
3. **Deferred rollback:** Use predicted values in both places, but replace them with backend values after an app restart or refresh. The discrepancy is less immediately noticeable, but still present. It also makes debugging tricky.


We’ve implemented features using all these strategies; there’s no clear winner.


## Example 3: follow buttons and instant UI


You may have noticed that when you tap “Follow” or “Follow back” in the app, the button state changes immediately. This is another example of frontend prediction in action.


The action updates visually before the server responds. This pattern, often called **optimistic UI** in the industry, is widely adopted in modern apps. Trello, iMessage, and WhatsApp all use similar techniques—for instance, showing a new message in the thread with a spinner while it’s being sent.


In these cases, failures are rare but noticeable. That’s why apps usually surface undelivered messages or errors clearly. This helps maintain trust while still delivering a snappy feel.


---


## Engineering considerations


Using frontend prediction means making deliberate tradeoffs. Here are some of the biggest considerations:


### 1. Duplicate state and logic


Maintaining logic on both frontend and backend increases code complexity. Even seemingly trivial rules (like “increment by one”) can evolve. If product requirements change, both implementations must stay in sync.


### 2. Rollback decisions


When predictions are wrong, we must decide whether and how to undo the incorrect UI. This decision depends on the stakes involved—mismatched XP might be fine, but incorrect gem balances or leaderboard promotions are not.


### 3. Granular models help


Duolingo’s user model contains fields like XP, gems, streaks, and more. If prediction logic is tied to a monolithic model, it becomes harder to manage partial rollbacks. Smaller, modular models enable different rollback strategies for different pieces of state.


## When to avoid prediction


In my opinion, some types of data should never rely on frontend prediction. These include:


- **Monetized resources** (e.g., when purchasing gems): Users expect precision.
- **Critical outcomes** (e.g., leaderboard promotions): These must reflect confirmed backend results.


A general rule of thumb: **If the user cares deeply or if the value affects monetization or fairness, wait for backend confirmation.**


## When it’s worth it


To recap, here’s a list of benefits of frontend prediction:


- **Reduces latency,** improving perceived responsiveness
- **Supports offline mode,** allowing uninterrupted usage
- **Improves resilience** to flaky networks or high latency


Additionally, it **decouples backend constraints** , freeing up architecture and performance optimizations by reducing the need for ultra-fast synchronous responses or strict consistency guarantees—both of which get harder at scale.


---


## Final thoughts


Frontend prediction is a powerful pattern, but it must be used with care.


First, consider whether you actually need it. If having a loading state and waiting for a backend response is acceptable, using this solution will let you avoid lots of complexity.


Next, in the case where you must use frontend prediction, while it might be tempting to search for a one-size-fits-all solution, the reality is that each feature has its own tradeoffs, rollback needs, and user experience considerations.


Prediction is inherently tied to product context—what’s acceptable for a follow button may be unacceptable for gem balances or leaderboard placement. You need to think carefully about whether prediction is truly worth the complexity in your specific case, how you’ll handle inconsistencies, what the rollback experience should look like, and how you will implement future product spec changes. There’s no one-size-fits-all pattern here—use this tool deliberately and with clear-eyed understanding of its costs.


Duolingo is unique in that our value to our learners is not one giant feature (e.g. a shopping cart or dating app that’s only matching and messaging). Rather, Duolingo is a large collection of mechanics (e.g., leaderboards). Therefore, teams have wide latitude to choose the right tradeoffs in technical decisions to fit their feature. If you’re interested in having this kind of agency in your work, we’re hiring!


[SEE OUR OPEN ROLES HERE](https://careers.duolingo.com/?department=Engineering&utm_source=blog.duolingo.com&utm_medium=blog&utm_campaign=frontend_blog_041526#careers)
