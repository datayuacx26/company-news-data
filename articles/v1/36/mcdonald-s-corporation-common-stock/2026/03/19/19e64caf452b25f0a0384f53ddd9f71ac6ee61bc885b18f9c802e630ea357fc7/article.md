---
schema_version: "1.0.0"
document_id: "19e64caf452b25f0a0384f53ddd9f71ac6ee61bc885b18f9c802e630ea357fc7"
company_key: "mcdonald-s-corporation-common-stock"
company: "McDonald's Corporation"
source_id: "mcdonald-s-corporation-common-stock-rss-e3f7e88d5cc9"
canonical_url: "https://medium.com/mcdonalds-technical-blog/why-waiting-feels-slow-and-how-were-using-real-time-signals-to-change-that-754cb13fdab1"
published_at: "2026-03-31T14:17:27+00:00"
first_seen_at: "2026-07-22T17:27:13.648832+00:00"
fetched_at: "2026-07-28T21:58:04.130568+00:00"
content_hash: "sha256:5ef62149c7e2d2ba12bf6dc86c4b23031d5ca890b61934921e5387f64c9c1202"
---

# Why Waiting Feels Slow — and How We’re Using Real‑Time Signals to Change That

# **Why Waiting Feels Slow — and How We’re Using Real‑Time Signals to Change That**


[Global Technology](https://medium.com/@global_technology?source=post_page---byline--754cb13fdab1---------------------------------------)


5 min read


·


Mar 31, 2026


--


A look into how our team turned uncertain wait moments into confident, glanceable experiences for guests in iOS using Live Activities.


*by: Maitree Shukla, Product Manager, Menu Browse, Alex Timmler, Director, Product Management, Nick Wilczynski, Sr Manager, Engineering Tech Lead, and Connor Sheehan, Software Engineer*


**Quick Bytes:**


- Digital transparency directly shapes perceived speed — and uncertainty can make even fast experiences feel slow
- Live Activities provide customers with real‑time order updates directly on the iOS Lock Screen and Dynamic Island
- Built as a reusable, backend‑driven platform, this work unlocks future innovation across McDonald’s digital ecosystem


In a digital experience, speed isn’t just about how fast an order is prepared — it’s about how fast it *feels* . When customers lack visibility into what’s happening with their order, uncertainty creeps in, and even short waits can feel longer than they are. Our Product and User Experience Design teams set out to reduce that uncertainty by delivering real‑time, glanceable updates through iOS Live Activities.


The concept of Live Activities stemmed from one guiding question the team asked ourselves: **How might we make the ordering experience feel as frictionless as possible?**


From there, that question naturally led to others.


How do we make waiting feel faster?


How can we be more transparent with customers?


And how can we reduce moments of uncertainty along the way?


To address those questions, we turned to Live Activities — a capability built into iOS — that enables real-time order updates on the Lock Screen and Dynamic Island, using premium screen real estate to provide reassurance without opening the app. It’s a simple but powerful way to reinforce McDonald’s reputation for leadership in convenience and speed, across both physical and digital experiences.


To deliver this experience, we needed strong foundations: updates must be timely, flows can’t disrupt restaurant operations, edge cases must be handled gracefully, and information must be relevant to the guest journey.


And while order fulfillment was our starting point, we knew this could go further. Other product teams were already curious about Live Activities, and we saw organizational value in making it reusable. As an example, delivery, loyalty, and accounts all have trackable experiences that could benefit from a state-based home on the Lock Screen or Dynamic Island. Designing for reuse means that our feature also provides a foundation for future innovation.


**Starting small to prove customer value** Before beginning the full development process for Live Activities, we first needed to gauge guest interest. We created a quick MVP using a straightforward Drive-Thru pickup flow. This lightweight setup allowed us to avoid complex configuration — the Live Activity could be started and automatically closed after a set period, giving us a simple way to answer a foundational question: *Would customers find value in this feature?*


The feedback was clear: guests valued the feature, with 93% agreeing that the feature was simple and intuitive!


**Designing an experience** When we began designing the ordering Live Activities widget, we kept a few design principles in mind:


- Design for continuity, so customers experience the same order information and visual language whether they’re viewing the Live Activity on the Lock Screen or in the Dynamic Island
- Maintain a clear information hierarchy showing customers exactly what they need
- Create a design that supports multiple routes and organizational opportunities


Following these guidelines, we created a sleek widget design that highlights store address, order progress, and order number.


**Scaling the solution beyond the MVP** With validated demand, we moved to a full solution. One of the first decisions was deciding how to deliver real-time updates — directly through Apple’s network (APN) or using a third-party provider. After weighing control, extensibility, and token management, we chose the third-party provider as this aligned with our existing push notification infrastructure and offered future flexibility.


We integrated our third-party vendor’s SDK into our Swift code, enabling token generation and accurate device targeting. The code was enhanced for dynamic content updates, allowing other product teams to adopt the same format seamlessly. All content is backend-driven and delivered in the user’s localized language.


> *“With the backend-driven architecture already handling content and localization, my focus on the front end became purely about the experience. That clarity made building beyond the MVP feel less like a rebuild and more like a natural extension of what the team had already put in place.”*
>
>
> — Connor Sheehan, Software Developer


**The backend work that made this possible** Our existing pub/sub system notified consumers when order states changed, but Live Activities required a new listener tailored to specific event triggers. This listener collects relevant information and passes it to our provider for delivery to devices.


At McDonald’s scale, complexity increases — millions of daily updates, varied device eligibility, and the need for fast filtering. We built a microservice to efficiently identify qualifying records and push updates with minimal CPU load.


**What we’d share with other teams** Designing and developing Live Activities at McDonald’s scale brought many valuable lessons:


- Building a platform matters more than building a single feature; flexibility encouraged interest across delivery, loyalty, and accounts
- Reliability, performance tuning, and localization are essential at a global scale
- Edge cases — like push‑to‑start tokens and network variability — matter more than anticipated and required thoughtful engineering
- A backend‑driven model greatly simplified testing, analytics, and reuse


**What this unlocks for future innovation**
Bringing Live Activities to life has been a true team effort, working closely with Laura Gomez, Sr. Manager Design, and Brad Carrera, Sr. Research Manager — and the result is more than just a convenient UI. It strengthens perceived speed, reduces uncertainty, and makes McDonald’s digital experience feel more transparent and responsive. The reusable architecture also sets the foundation for future innovations across our digital ecosystem.


Our hope is that guests find Live Activities as helpful and reassuring as we did while building them — and that this work becomes an important piece of how McDonald’s continues to lead in digital convenience.
