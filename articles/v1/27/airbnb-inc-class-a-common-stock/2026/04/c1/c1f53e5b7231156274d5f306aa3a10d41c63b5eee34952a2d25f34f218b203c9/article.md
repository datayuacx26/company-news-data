---
schema_version: "1.0.0"
document_id: "c1f53e5b7231156274d5f306aa3a10d41c63b5eee34952a2d25f34f218b203c9"
company_key: "airbnb-inc-class-a-common-stock"
company: "Airbnb Inc."
source_id: "airbnb-inc-class-a-common-stock-rss-af0970d10a00"
canonical_url: "https://airbnb.tech/data/privacy-first-connections-empowering-social-experiences-at-airbnb/"
published_at: "2026-04-16T20:46:58+00:00"
first_seen_at: "2026-07-20T03:32:35.357269+00:00"
fetched_at: "2026-07-28T20:52:31.063760+00:00"
content_hash: "sha256:2c2978a9e83c83749f09c5c99f04f3630167900b90d748f9fcc4407141fba107"
---

# Privacy-first connections: Empowering social experiences at Airbnb

Discover how Airbnb prioritizes user privacy while building a more connected community, empowering guests to engage socially, connect confidently, and maintain control of their personal data.


By:[Joy Jing](https://www.linkedin.com/in/joyjing1/)


## ✨ Building a more connected community


At Airbnb, our hosts and guests form the heart of our community. As[shared by CEO Brian Chesky](https://news.airbnb.com/introducing-social-features-for-airbnb-experiences/) , we’re evolving into a more social ecosystem.[Airbnb Experiences](https://www.airbnb.com/resources/hosting-homes/a/introducing-completely-reimagined-airbnb-experiences-742) now highlight the people involved as much as the activity. Guests can see Who’s going on an Experience, message co-guests directly, and view people they’ve met through the Connections section in their Airbnb profile. Guests are able to choose to share their profile for each new Experience. Guests who choose not to share their profile will not have their photo or profile info shared with others outside of their travel group before or during the experience. Our goal is to foster meaningful connections while giving guests control over their privacy.


*Guests can choose whether to share their profile with other co-guests when booking an Experience.*


In this post, we’ll share how we built new social features with privacy by design at their core. You’ll learn about our approach to user privacy, the technical decisions we made, and how we’re empowering guests to control their visibility every step of the way.


## 👩‍💼 Users vs. Profiles: Why we separate them


At Airbnb, building trust while protecting privacy is fundamental. To achieve this, we’ve made a clear distinction between the concepts of **User** and **Profile** .


**User** represents the complete, internal record we hold about an Airbnb user, including names, email addresses, phone numbers, and account details. This is the information we collect and use when providing our services. Whereas a **Profile** includes a subset of information about a User and is their public-facing representation. The information displayed on a Profile varies based on whether the user is a host, guest, or both. Users can choose to add as much or little to their profile as they see fit.


For example, as a guest, you might need your host’s phone number for check-in, but other guests reading your review after the stay shouldn’t see any of your contact info.


One user can have multiple profiles. For example, we have **Host Profiles** for prospective guests to learn more about the host they’ll be staying with.


The Host Profile for CEO Brian Chesky.


We also have **Guest Profiles** for hosts to learn more about the guests they will be hosting.


The Guest Profile for CEO Brian Chesky.


We now also create **Experience-specific Guest Profiles** , which manages how a guest’s profile information is shown to other guests on that experience outside their travel group. If a guest chooses not to share their profile, their profile will not be viewable by others looking to book the Experience. Only their first name will be visible to other guests on the Experience outside of their travel group.


How an Experience-specific guest profile will look if the guest chooses to be private on an Airbnb Experience. Only the first name of the Profile is visible to other co-guests.


## 🆔 How we enable this: Separate identifiers


To deliver this context-aware experience, we’ve introduced two distinct types of identifiers:


- A **User ID** that represents the internal user entity.
- A **Profile ID** that identifies how that user appears in a specific context.


Each user will only have one User ID, but could have multiple Profile IDs that are used in different contexts. By decoupling these, we enable:


- **Context-awareness:** Each profile is visible only where it’s relevant.
- **Flexible representation:** Guests can choose how they appear to other guests on each Experience.
- **Privacy controls:** By decoupling the User ID and Profile ID, we put mitigations in place to make it more challenging to link profiles across contexts.


Ultimately, this empowers guests to control when their profile data is shared with other guests and hosts, and keeps identity management simple and intuitive.


*Diagram showing how different profiles represent a user in various contexts, such as different Airbnb Experiences, or as a host versus a guest. The diagram shows how different profiles are linked to the same user, and how guests on an Experience can only view the co-guest profiles that are specific to that shared Experience.*


For example, let’s say Marie chooses to attend an Airbnb Experience called “Pasta Making with Nonna” and decides to remain private. We will create Profile A for Marie, associated with “Pasta Making with Nonna,” which will only surface her first name and will not include her profile photo. If Marie also attends an Airbnb Experience called “Goat Yoga”, we will create a separate Profile B for her associated with “Goat Yoga.” If Marie opts-in to social features on the “Goat Yoga” Airbnb Experience, other co-guests will be able to see her profile photo, guest stats, and other “About me” information, which she can curate from her Profile edit page.


If Alex also attends the Airbnb Experience “Goat Yoga,” he will see Marie’s full Profile B. If Alex happens to browse “Pasta Making with Nonna”, however, he won’t see Marie’s profile photo in the “Who’s going” list. As a result, Alex will not know that Marie from the “Goat Yoga” Airbnb Experience will be attending the “Pasta Making with Nonna” Airbnb Experience as well.


*The “Who’s Going” list displays other guests attending an Experience before you book.*


As another example, say Alex is a host of a mountain cabin in the Swiss Alps. When he attends the “Goat Yoga” Experience as a guest, his host profile remains separate from his guest profile for that Experience. This means that fellow guests like Marie won’t be able to tell that Alex also hosts a cabin in the Alps, because Alex’s host profile and guest profile are not linked. If Marie later searches for places to stay in the Swiss Alps, she might come across Alex’s cabin listing. However, if Alex has chosen to remain private on the “Goat Yoga” Airbnb Experience, Marie will only see his first name, and won’t be able to know that his guest profile on “Goat Yoga” and his host profile for the mountain cabin represent the same person.


*Options for guests to control whether their profile should be visible or not for an Experience.*


## 🔐 Permissions


Airbnb users interact with a range of people: fellow travelers, hosts, Airbnb support personnel, and more. Each interaction requires the right privacy boundaries. We use **least-privileged access** to ensure everyone sees only the data they need.


To manage these permissions, we use[Himeji](https://medium.com/airbnb-engineering/himeji-a-scalable-centralized-system-for-authorization-at-airbnb-341664924574) , our in-house authorization system. Himeji enforces access controls at the data layer, ensuring privacy and security beyond just the user interface. One of Himeji’s key optimizations is its ability to perform configurable relation denormalization at write time, when profile information or permissions change. This makes permission checks at read time extremely fast and scalable, enabling users to have a seamless experience even as privacy needs grow more complex.


## 🔄 Implementation


In order to launch, work was needed to ensure that Airbnb’s platform utilized the right identifier in the right context.


**1. Automated auditing**


We developed Python scripts to audit the codebase, searching for known patterns associated with user data access. This gave us a comprehensive list of candidate locations.


**2. Determining team ownership**


Our scripts mapped each finding to the owning team via the directory structure. This let us assign migration work directly and efficiently.


**3. Manual review for context**


Code owners manually reviewed findings, determining whether each instance was internal-only or externally used. This hands-on review layer was critical for accuracy and confidence.


**4. AI-powered refactoring**


We leveraged AI-powered refactoring tools to suggest code changes based on our audit findings. However, engineers always remained in the loop by reviewing, refining, and applying code updates, which ensured correctness and protected nuanced business logic.


**5. Company-wide collaboration**


Perhaps the most important ingredient was company-wide alignment. Teams across Airbnb (engineering, product, privacy, legal, and beyond) came together with a shared mission. This collective commitment ensured prioritization, smooth coordination, and ultimately, a successful migration.


## 🧰 Type safety and migration quality


Strong typing and automated tests were our safety net. We made sure profile IDs and user IDs couldn’t be mixed up accidentally. Code reviews, linters, and type checks enforced boundaries. Progress was tracked in a shared hub, keeping everyone aligned and accountable.


## 🎯 Our privacy principles


As Airbnb becomes more social, guest privacy stays at the heart of everything we build. Our new context-aware profile IDs lay the groundwork for future features without compromising trust and reflects our commitment to privacy in our[Privacy Principles](https://www.airbnb.com/privacy) .


If this type of work interests you, check out some of our[related positions.](https://careers.airbnb.com/)


## Acknowledgments


It takes a village to build a robust privacy-oriented infrastructure. Special thanks to:


- [Usman Abbasi](https://www.linkedin.com/in/uabbasi/)
- [Ansuman Acharya](https://www.linkedin.com/in/ansumanacharya/)
- [Matt Byington](https://www.linkedin.com/in/matt-byington/)
- [Simon Cao](https://www.linkedin.com/in/simoncao1/)
- [Ananth Dandibhotla](https://www.linkedin.com/in/ananth-dandibhotla/)
- [Matt Gadda](https://www.linkedin.com/in/mgadda/)
- [David Haggerty](https://www.linkedin.com/in/david-haggerty-0942b54/)
- [Weiwei Huo](https://www.linkedin.com/in/weiwei-huo-b3450949/)
- [Jasmine Lee](https://www.linkedin.com/in/ljasmine/)
- [Houkun Li](https://www.linkedin.com/in/lihoukun/)
- [Hugh McCauley](https://www.linkedin.com/in/hughmccauley/)
- [Margot Nack](https://www.linkedin.com/in/margotnack/)
- [Ezlie Nguyen](https://www.linkedin.com/in/ezlie/)
- [Ashley Quitoriano](https://www.linkedin.com/in/ashleymika/)
- [Omar Cruz Salgado](https://www.linkedin.com/in/omaracruz/)
- [Evelyn Shen](https://www.linkedin.com/in/evelynxiaoyingshen/)
- [Jordan Smith](https://www.linkedin.com/in/jordansmith42/)
- [Andrew Sorensen](https://www.linkedin.com/in/andrewx192/)
- [Yupeng Yan](https://www.linkedin.com/in/yupeng-yan/)
- [Ximin Zhang](https://www.linkedin.com/in/simplechrisz/)


*All product names, logos, and brands are property of their respective owners. All company, product and service names used in this website are for identification purposes only. Use of these names, logos, and brands does not imply endorsement.*
