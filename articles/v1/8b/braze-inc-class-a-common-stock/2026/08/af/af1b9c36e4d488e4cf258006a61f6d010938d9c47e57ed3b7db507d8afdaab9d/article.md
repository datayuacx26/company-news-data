---
schema_version: "1.0.0"
document_id: "af1b9c36e4d488e4cf258006a61f6d010938d9c47e57ed3b7db507d8afdaab9d"
company_key: "braze-inc-class-a-common-stock"
company: "Braze Inc."
source_id: "braze-inc-class-a-common-stock-news-import-06f37ae7f1b6"
canonical_url: "https://www.braze.com/resources/articles/summer-2026-whatsapp-identity-features"
published_at: "2026-08-11T21:48:28.712+00:00"
first_seen_at: "2026-08-12T07:50:50.567999+00:00"
fetched_at: "2026-08-12T07:50:51.186145+00:00"
content_hash: "sha256:d1a3b97564f5179545c809c46e32d7220cabfd3ca5242c67a9bf6973f3611b3c"
---

# Usernames, BSUIDs, and business usernames: Demystifying WhatsApp's newest identity features

***TL;DR***


*Meta is rolling out three key changes this summer in connection with its collection of user and business identity features: Usernames, business-scoped user IDs (BSUIDs), and business usernames. These features give users more control over whether they share their phone numbers with brands, while also making it easier for users to recognize brands when they receive messages.*


***Key takeaways***


- *WhatsApp usernames, business-scoped user IDs (BSUIDs), and business usernames are being launched this summer and are distinct from previous features like profile names for users and display names for businesses*
- *Braze will help brands message individuals who select consumer usernames and support BSUIDs, but brands are responsible for claiming their own business usernames*
- *WhatsApp is giving both consumers and brands more control over how they show up to each other*


Consider the user name. It's been a key way for consumers to represent themselves online since the dial-up days. Now WhatsApp is putting a whole new spin on it.


This summer, Meta is rolling out three related (but distinct) new concepts in connection with WhatsApp: Usernames, business-scoped user IDs (BSUIDs), and business usernames. The good news? These changes are a win for both consumers and brands—consumers get more control over their privacy with new, optional usernames, while brands get clear, recognizable identities on the platform with business usernames—and Braze is handling the heavy lifting on the backend, making it easier for marketers to take advantage.


Read on for a breakdown of each concept, what's changing, and what action (if any) you need to take to keep your marketing campaigns and journeys running without a hitch.


## **A quick overview of the three new concepts**


Before we go deep on each of these concepts, here’s a quick overview:


Feature


Who it's for


What it does


Level it operates at


Usernames (e.g. consumer usernames)


Consumers


An optional feature that let users promote and share a chosen “handle,” and hides their phone number from the people and businesses they message


Consumer's own account


Business-scoped user IDs (BSUIDs)


Businesses (backend)


The alternate identifier Meta gives you when a user's phone number is hidden


Business portfolio*


Business usernames


Businesses


An optional, brand-controlled handle that displays in place of your phone number


Phone number


**Note that a Meta business portfolio can contain multiple WhatsApp Business Accounts (WABAs), and each WABA can contain multiple phone numbers.*


Now let's dive deeper into each of these updates.


## **Usernames: More privacy for your customers**


[Usernames](https://blog.whatsapp.com/its-time-to-reserve-your-whatsapp-username) are an optional feature that lets WhatsApp users set a unique @username and display it instead of their phone number. When a consumer adopts a username (Braze refers to them as consumer usernames for clarity), their phone number is no longer automatically shared with the people and businesses they message or call. But fear not. Brands can still connect with these users via a new backend identifier called a business-scoped user ID (more on that later!). Both consumer usernames and BSUIDs are rolling out in August 2026.


This feature represents a meaningful privacy upgrade for your customers, which is ultimately good news for your brand, too. After all, the more comfortable people feel engaging with brands on WhatsApp, the more engagement you're likely to see over time.


You may be wondering how consumer usernames interact with profile names, an existing WhatsApp feature that's been part of the app for years. Think of the profile name as the consumer's public display name (e.g. "John Smith"), which doesn't need to be unique and can be changed anytime. A consumer username (or handle) is a unique, @-symbol identifier used for searching and tagging. More importantly, adopting a consumer username determines whether the consumer's phone number gets shared with businesses on the platform, providing additional privacy protections.


### **How Braze helps**


If you're a Braze customer, rest assured that we've designed our platform to handle this update for you automatically via the BSUID.


## **Business-scoped user IDs (BSUIDs): Your new (backend) identifier**


A business-scoped user ID (BSUID) is the unique, persistent identifier Meta uses to represent to every consumer you message, regardless of whether they've adopted a username. BSUIDs are particularly important when consumers do adopt usernames, because it allows marketers to continue reaching them once their phone number is no longer automatically shared. BSUIDs show up in the same webhook payloads that carry phone numbers, so you can still send template messages, response messages, and run campaigns and Canvases just as you would with a phone number.


BSUIDs are assigned at the business portfolio level, so a given consumer will have the same BSUID no matter which of your WhatsApp numbers they message within your portfolios. However, if you operate multiple separate business portfolios, each consumer will get a different BSUID in each one. To address that complication, Meta offers parent BSUIDs, which allows eligible organizations to link together separate business portfolios and receive a parent BSUID for each consumer, ensuring a single identifier that can be shared across every phone number in every linked portfolio.


All that said, BSUIDs mostly come into play for new consumers you haven't engaged with before. If a consumer has an existing conversation history with your brand, or shows up in your WhatsApp Contact Book, WhatsApp will continue to share their phone number with you instead.


### **How Braze helps**


If you're a Braze customer, this update is built to be invisible to you: Your existing campaigns and Canvases will keep running as is, and you'll keep reaching every consumer automatically, whether or not they've adopted a username. Here's what will happen under the hood:


- **Message routing:** If a user only has a BSUID (no phone number available), Braze will automatically route messages using that BSUID instead. No changes will be needed to your message templates, response messages, or Canvases.
- **Full user profiles:** BSUID-only users get complete Braze profiles. They can enter Canvases, trigger events, and be targeted through standard audience filters (like subscription group membership) just like any other user.
- **Data pipelines:** WhatsApp Currents events include a BSUID field alongside (or in place of) the phone number field, so your downstream reporting continues working.
- **Parent BSUID prioritization:** If your business portfolios are linked, Braze automatically prioritizes a consumer's parent BSUID (when one is present) as their identifier to avoid duplicate profiles.


### **What you should double-check**


For most customers, no action is required. But if you want to be extra thorough and avoid any risk of duplicate user profiles, here are three quick checks to go over now:


1. **Confirm Contact Book is enabled.** This Meta feature (on by default) preserves phone numbers for consumers you've already messaged, even after they adopt a username. Double check that this is enabled under Meta Business Suite > Business settings > Business info.
2. **Link your business portfolios, if you operate more than one.** If your organization runs multiple, separate business portfolios, ask your Meta point-of-contact about linking them so you can start receiving parent BSUIDs and avoid duplicate profiles.
3. **Check your downstream pipelines.** If you use Currents, confirm your systems can gracefully handle a null or empty phone number field for BSUID-only users.


For more information and technical details on how Braze handles BSUIDs, check out our[User Docs](https://www.braze.com/docs/user_guide/channels/whatsapp/message_features_and_optimization/bsuid) .


## **Business usernames: Claim your handle**


Separately, Meta is also rolling out business usernames this summer. Business usernames are optional handles that appear in chat windows in place of your phone number if your display name is not verified (and if the consumer hasn’t saved you as a contact). A business username gives you a cleaner, more recognizable identity than a phone number, making it more likely that your customers will engage with your messages.


Because it's a unique handle, customers who already know it can message you directly, without needing to save your phone number. Unlike the consumer version, though, adopting a business username does not hide your phone number. It always remains visible on your business profile, allowing your customers to contact you.


Business usernames are yours to reserve, claim, and manage now in the[WhatsApp Manager](https://business.facebook.com/wa/manage/) . Meta may have already pre-reserved one for your brand (typically matching your existing Facebook Page or Instagram handle); if so, you can claim that reserved username or pick a different one. If the reserved name matches a Facebook Page or Instagram account you haven't yet linked to your WhatsApp phone number, you'll need to link them first.


A few important nuances to know before you claim a business username:


- **Business usernames are unique across all WhatsApp phone numbers** (and across consumers and businesses). That means if you run two WhatsApp phone numbers—say, one for support and one for marketing—you'll need two distinct usernames.
- **Business usernames are case-insensitive, but periods and underscores count.** For instance, myid and myID are treated as the same username, but myid, my.id, and my_id are all considered distinct.
- **Format requirements:** Usernames must be 3–35 characters, contain only English letters, digits, periods, or underscores, include at least one letter, avoid leading/trailing periods or consecutive periods, and can't start with "www" or end in a common domain suffix like .com, .org, or .net.


We mentioned that business usernames are beneficial if your display name isn't verified yet, so you might be wondering what the difference is between the two. In short, a WhatsApp display name is your official, Meta-approved business name. A business username is a new, unique handle (e.g. @YourBusinessName).


So which one appears in the chat window? WhatsApp determines what to show using a strict priority order, from highest to lowest:


1. Saved contact name (if the consumer has you saved in their phone)
2. Your verified display name
3. Business username
4. Phone number


A key point on display names: Every business gets a display name the moment it registers a phone number, verified or not. But verification controls visibility—an unverified business's display name only shows up in small text in the contacts view, with the phone number showing everywhere else, whereas a verified business's display name appears everywhere. This is why business usernames are a useful stopgap before verification or when a consumer hasn't saved you as a contact and is searching for you by name. Once verified, your display name is what most customers will see either way.


### **How to claim your business username**


This process is a quick, self-serve step on your end, rather than something Braze automates. Business usernames are a Meta-side identity setting, so you'll claim and manage yours directly in WhatsApp Manager and Meta Business Suite rather than in Braze.


For more information and instructions on how to claim your business username, check out our[User Docs](https://braze.com/docs/user_guide/channels/whatsapp/meta_resources#2026-business-usernames) .


## **The bottom line**


Taken together, these three updates point in a consistent direction: WhatsApp is giving both consumers and brands more control over how they show up to each other. For consumers, that means more privacy and more comfort engaging with brands on the channel. For your company, it means more ways to be recognized and found. It might feel like a lot to absorb all at once, but the underlying goal for your business is simple—to keep reaching your customers effectively on this personal, high-engagement channel.


*Want more on how Braze supports WhatsApp as part of your cross-channel strategy? Check out our[WhatsApp product page](https://www.braze.com/product/whatsapp) or check out our[WhatsApp User Docs](https://www.braze.com/docs/user_guide/channels/whatsapp/) for technical info.*


**Forward-looking statements**


*This blog post contains “forward-looking statements” within the meaning of the “safe harbor” provisions of the Private Securities Litigation Reform Act of 1995, including but not limited to, statements regarding the performance of and expected benefits from Braze and its products and services. These forward-looking statements are based on the current assumptions, expectations and beliefs of Braze, and are subject to substantial risks, uncertainties and changes in circumstances that may cause actual results, performance or achievements to be materially different from any future results, performance or achievements expressed or implied by the forward-looking statements. Further information on potential factors that could affect Braze results are included in the Braze Quarterly Report on Form 10-Q for the fiscal quarter ended April 30, 2026, filed with the U.S. Securities and Exchange Commission on May 28, 2026, and the other public filings of Braze with the U.S. Securities and Exchange Commission. The forward-looking statements included in this blog post represent the views of Braze only as of the date of this blog post, and Braze assumes no obligation, and does not intend to update these forward-looking statements, except as required by law.*
