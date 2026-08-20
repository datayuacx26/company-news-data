---
schema_version: "1.0.0"
document_id: "5628ff9e2c382165740f92464bfca8919ed73af7d3100e7baee970432c083aa9"
company_key: "yc-dyspatch"
company: "Dyspatch"
source_id: "yc-dyspatch-rss-ea7cf496860a"
canonical_url: "https://www.dyspatch.io/blog/10-email-design-pitfalls-and-how-to-avoid-them/"
published_at: "2026-03-02T19:58:51+00:00"
first_seen_at: "2026-07-20T23:20:29.288677+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:9e03ce95688711bacdffd1c4dd7f2b50492e71da73c371c58f97ae3c203ac341"
---

# 10 Email Design Pitfalls (And How to Avoid Them)

Anything is possible, I truly believe that. There are teams building whole games in email! That said, at Dyspatch, we’re all about two things:


- **Helping product and marketing teams collaborate**
- **Sending a perfect email every time**


That means designing with email’s constraints in mind


**up front** , so you’re not backtracking later trying to fix dark mode, unclip Gmail, or rework layouts.


Below are common design choices that create pain during build, QA, and long-term maintenance. Most of them aren’t a full veto! They can be used safely, sparingly, with intention.


Knowing these challenges early will help you build a more robust email design system: a foundation of easy-to-build, accessible, reusable, brand-compliant modules.


###


## 10 Email Design Pitfalls


###


### **Overlapping elements**


**The pitfall:** Layouts that require content to sit


*on top of* other content (overlapping sections, floating badges across blocks, negative margins everywhere).


On the web: easy, z-index, flexbox, etc (I don't know, fancy web dev stuff, I'm an email dev).


In email: unreliable across clients, falls apart in Outlook on Windows.


**Better:** Treat email as


**2D** . Stack things vertically or place them side-by-side. If you need a “layered” feel, fake it with:


- A background image + live text above/below


- A card block with a simple border


###


### **** **Choosing specific colors for dark mode (before you test)**


**The pitfall:** Making a specific dark mode design before testing in actual email clients/device combinations is like wishing upon a star when really, you’re just squinting into the dark (get it, because all your well-intentioned designs will just turn into an inky void in Gmail on Android, no? Not funny?). Gmail on iPhone and Android, and many of the Outlook clients, don’t support dark mode preferences in a way you can reliably control, so there isn’t a dependable way to “design a perfect dark mode theme” for everyone.


**Better:** During the design phase, test color combinations (Litmus, Email on Acid, or just sending to real Gmail iPhone/Android inboxes) to make sure contrast holds up. Be okay with


**clarity over pixel perfection** .


###


### **** **Wildly different mobile vs desktop designs**


**The pitfall:** Hiding whole sections for desktop or mobile, or writing media queries for every element in your email to shift things around, gets you closer to


[clipping in Gmail](https://www.dyspatch.io/blog/why-is-gmail-clipping-my-emails/) and makes builds harder to maintain.


**Better:** Design


**mobile first** . This mindset shift cuts down on huge alterations that happen when trying to go from desktop → mobile. If your design works on mobile, it’s much more likely to work well on desktop — you might just need to bump up some text sizes.


###


### **** **Nesting containers**


**The pitfall:** More nested containers = more email size = more Gmail clipping risk. For email to be responsive and work in different clients, we need to be code-size conscious. Designs with a box, in a box, in a box end up being a lot of code for purely design purposes.


In web design this might be a tidy


div


in a


div


in a


div


. But in email it becomes:


table > tr > td > div > table > tr > td > div > table > tr > td > div


…


…and that’s before all the inline styles, classes, and Outlook-specific code required for email.


**Better:** Use nesting intentionally to reduce file size and prevent Gmail clipping:


- Fewer containers, stronger spacing


- Reuse a small set of layout basics (section, card, grid, divider)


- Reserve “box-in-box” for places where it adds real meaning (hierarchy, emphasis, scannability)


###


### **** **Text on images**


**The pitfall:**


- **Responsive scaling:** text and images resize on different devices, which can make how the text lays on your image unpredictable.


- **Dark mode:** dark mode can invert your text but your background image stays the same. That can make text disappear into the image in dark mode, even if it popped in light mode.


**When to use:**


- You’ve picked a text color that stays legible even when it’s inverted in dark mode


- You’ve designed the text size and placement to hold up across screen sizes


###


### **** **Dark logos/icons with transparent backgrounds**


**The pitfall:** This is a super common one! A dark logo/icon set that looks great in light mode disappears in dark mode.


**Better:**


- Give the image a baked-in background instead of fully transparent,


**or**
- Add a stroke around the logo/icon that matches your background in light mode


That way it looks the same in light mode, but still pops out in dark mode.


###


### **** **Everything is a link, all the analytics!**


**The pitfall:** More links = more time to build your email, more chances for a wrong link to be added or missed. And it’s not just effort, it’s size which leads to Gmail clipping.


**In our testing, we’ve seen email clients add an extra ~2kb of characters for every link to add click tracking.** That can mean your email balloons


**20% or more** on send


*just from link tracking* .


**Better** Link intentionally: Clear CTAs mean you can avoid Gmail clipping by simply having fewer links.


###


### **** **Make it just a big image!**


There are many blog posts on this so I’ll just link to those below. What I will say:


**The pitfall:** Image-only emails make iteration and localization harder, and may fall apart when images are turned off or slow to load.


**Better:** A live text email is faster to build, faster to edit, and faster to localize. Using an image for more complex design elements is okay. Even having text in your image isn’t the end of the world if you have good alt text (bonus points if you have a good alt text generator).


If you are going to use large images in your email:


Know your audience


- More technical audiences often have images turned off in their email client


- Rural audiences might not have the data to download your image


And know your team


- Do you have designers on hand with time to rebuild imagery through feedback cycles? This can be a strain.


- Are you hoping to have your email in different languages? This becomes a huge lift to localize.


- Do you have a strong QA process that will catch alt text that is off or missing?


###


### **** **Web fonts and “perfect typography”**


**The pitfall:** Designing an email around a specific font. Many email clients will fall back to their own default fonts. When your font changes, line breaks change, spacing changes, and suddenly your “perfect” layout looks off.


**Better:**


- Design with a realistic fallback font stack in mind


- Keep headings and key lines flexible so they still look good when the font swaps


##


## **A quick design-phase checklist**


If you want to catch the big issues early, run through this before your design system gets “locked”:


- **Layout:** Can this be built without overlaps, negative margins, or fragile positioning?


- **Dark mode:** Do your core text/background combinations hold up in real inbox testing?


- **Mobile-first:** Does the design work on mobile without heavy hide/show logic?


- **Complexity:** Are containers/nesting used intentionally, or just for “box-in-box” aesthetics?


- **Links:** Are you tracking what matters (and not ballooning size with “everything is clickable”)?


- **Typography:** Does it still look good in fallback fonts?


### **The Email Builder that Global Brands Love**


Learn more


```text


```
