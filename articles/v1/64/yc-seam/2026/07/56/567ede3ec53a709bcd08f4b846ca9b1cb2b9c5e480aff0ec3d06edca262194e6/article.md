---
schema_version: "1.0.0"
document_id: "567ede3ec53a709bcd08f4b846ca9b1cb2b9c5e480aff0ec3d06edca262194e6"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/whatever-happened-to-digital-keys"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:b0b9e2f32ba65469e2c694aeb5d9074d7d60ba6a705e8e70885e0ba9e865fbaf"
---

# Whatever Happened To Digital Keys?

> In 2020, industry analysts at *Hotelier Magazine* said digital keys were "destined to become an industry standard." So why didn’t they?


The benefits were obvious.


Digital keys would lower the burden on front desk staff. Support stronger security measures. And allow guests to check in and access their rooms without friction. It was the textbook definition of a win-win.


But more than 10 years after digital keys first appeared on the market, the defaults for granting access are still plastic keycards and PIN codes.


The software exists. The hardware exists. And guests’ expectations of convenience and sophistication exist — with 71% of travelers saying they prefer hotels with self-service options like mobile keys and contactless check-in.


So, why are hotels and motels stuck in the past?


In this article, we’ll explore why, despite the initial hype, the hospitality industry has been slow to adopt digital keys and what types of solutions exist, and discuss what needs to change so that they can finally achieve the wide adoption they should already have.


## Strong Demand — From Inside and Out


According to a 2025 report by the American Hotel & Lodging Association, a whopping 65% of hotels report they are understaffed. And despite their intense efforts to recruit, housekeeping and front desk roles are, by far, the hardest roles to fill.


No wonder digital keys sound like common sense: they easily fill this gap — taking the pressure off front desk staff so hotels can do far more with smaller teams, while still providing consistent guest experiences.


But this makes the low adoption of digital keys all the more confusing, as hotels and motels are already using technology to pick up the slack: from AI chatbots and virtual assistants that can serve guests 24/7, to scheduling software that automatically adjusts maintenance and housekeeping shifts to maximize efficiency behind the scenes.


What’s surprising is that digital keys also improve guest satisfaction. It doesn’t matter how much or how little they’ve traveled, guests just want to get to their rooms as fast and as smoothly as possible — with 80% preferring fully automated front desks. However, guests frequently report having to physically check in at a front desk anyway, even in hotel chains that have digital key systems.


With so much demand, it doesn’t seem to make sense why digital keys haven’t been adopted.


## Many Solutions. Many Limitations.


To get to the heart of the problem we have to first define what “digital keys” actually are.


Today, there’s a handful of solutions that qualify as digital keys, but they blur the lines with their language, switching between terms like “mobile”, “digital”, and “Bluetooth” — making it difficult to understand the differences between them.


Here’s a quick overview of what those solutions are and how they differ from each other.


## Mobile BLE Keys (App-Based)


As with all things Seam, our goal is to provide a simple API that makes it easy and intuitive to integrate multiple access control systems and issue access credentials to your users.


**Think:** OpenKey, Flexipass, or proprietary solutions like Marriott Bonvoy. And of course, Seam.


Mobile BLE (Bluetooth Low Energy) keys are the most mature digital key solution—the first mobile keyless entry systems were implemented as early as 2014. These systems use, you guessed it, Bluetooth to deliver secure, strongly-encrypted device-based credentials to grant access. They can be integrated directly into a hotel or motel’s branded app and can include advanced features such as remote revocation and time-limited access, which is great for short-term or temporary guests and visitors.


But Mobile BLE keys have a catch: it puts a burden on your guests as they have to download your app, register, verify their identity, and onboard themselves just to get access to their room. Compared to the relative inconvenience of standing at a front desk for a few minutes, it’s easy to see why most would choose to wait.


**Pros of Mobile BLE Keys:**


- Uses widely available Bluetooth technology found in all modern smartphones
- Secure, device-based credentials with strong encryption
- Enables advanced features like remote revocation and time-limited access
- Can integrate into branded apps to reinforce brand engagement
- Seam mobile SDK to download this credential automatically and begin unlocking the door. Alternately, if you’ve issued a plastic card, you can use our Encoders API to encode the key and begin using it.


**Cons of Mobile BLE Keys:**


- Requires guests to download a mobile app, which creates friction—especially for short stays or one-time visitors.
- Adoption is low among guests who aren't part of loyalty programs or already familiar with the app.
- Onboarding and registration add complexity to what should be a simple check-in process.


## Remote Unlock Systems


**Think:** Some implementations via Flexipass, cloud-native systems. And of course, Seam.


Remote unlock systems let hotel staff to unlock doors for guests from a central location or device of your choosing, via Bluetooth, NFC, or Ultra-Wideband Bluetooth (UWB).


These systems are a great solution for hotels and motels with multiple units or houses scattered across a property, including properties where guests usually have extended stays. It also is a great solution for operators struggling to fill in early morning and late night shifts at the front desk — and may eliminate the need for a front desk, at all.


The only problem is that remote unlock systems don’t work with offline lock systems or with mixed hardware. So if you use multiple hardware vendors or have a mix of cloud and offline systems, then you have a long road ahead of you to get it up and running.


**Pros:**


- Allows staff to remotely unlock doors for guests or vendors without being physically present
- Useful for late check-ins, unattended front desks, and remote property management
- Reduces the pressure on front desk staff during peak periods


**Cons:**


- Only supported by cloud-connected lock systems
- Not universally supported, even by vendors with "online" systems such as Vostio or Salto may not support full unlock API access
- Limited manufacturer support makes it difficult to scale across property portfolios with mixed hardware


## Apple Wallet Keys


**Think:** Zaplox and direct manufacturer integrations such as ASSA ABLOY. And of course, Seam.


No need to download and install a separate app with Apple Wallet keys. Accessed directly from their Apple Wallet, guests can receive their digital keys ahead of time, allowing them to gain access to their rooms immediately on arrival — for a truly elegant and intuitive native guest experience.


But there’s a catch: guests will need to be carrying an iPhone 6s (or higher) that’s running on iOS 15 (or higher). And if they don’t have a tablet or iPhone, they’ll need at least an Apple Watch Series 3 running watchOS 8. You’ll also need to have the right hardware installed to make sure it will work with Apple’s infrastructure, and pass Apple’s strict due diligence process. And if you can get past all those hurdles, there’s still additional fees — with Apple charging you around 20 to 40 cents per digital key delivered.


**Pros:**


- Provides an elegant, native guest experience on Apple devices
- Keys can be delivered digitally before arrival and accessed via Wallet without opening an app
- High perceived value among design-conscious and tech-forward guests
- Seamless integration with the Apple ecosystem that many travelers already use


**Cons:**


- Only works with iOS device owners, excluding the substantial portion of guests with Android phones
- Requires specific recent devices (guests need iPhone 6s or later running iOS 15 or later, or Apple Watch Series 3 or later running watchOS 8 or later)
- Requires full compliance with Apple's hospitality key specifications
- Technical integration is complex, requiring specific brands of hardware and firmware, as well as full ecosystem support — including servers that support TLS 1.2 and later versions
- Per-key delivery fees, commonly reported at $0.20–$0.40 per key


## App-Free Instant Keys


**Available via:** Seam


Instant Keys represent a newer approach that offers the convenience of digital keys without buying new hardware, forcing guests to download an app, or submitting to Apple’s laundry list of requirements.


Similar to mobile BLE keys, Instant Keys are delivered to guests by email or SMS and can be accessed instantly through an app clip. Allowing guests to gain access quickly and simply — no matter what device they have or which operating system they use. It’s also highly flexible, already integrating with multiple existing smart lock brands. Making it less likely for you to replace your hardware or upgrade your infrastructure to get it to work.


**Pro:**


- Uses BLE for secure access without requiring guests to download an app
- Delivered securely via SMS or email and opens instantly via an app clip (a lightweight, temporary app experience)
- Works with many existing lock systems through unified API integration — handled through the platform layer
- Supports both guest and staff workflows
- Next-to-no friction for one-time guests and short stays


**Cons:**


- Lock compatibility depends on vendor and firmware, but multiple integrations are already available


# Unlocking The Adoption Of Digital Keys


By taking a closer look at the pros and cons of the solutions above, three distinct patterns emerge as the core blockers to adopting digital keys: infrastructure, engineering, and guests’ experience.


## Infrastructure


Despite the leaps in technology over the last decade, hotels and motels are still in the middle of upgrading their systems and infrastructure from offline to online. Leaving many operators stuck as they try to retrofit modern solutions around decades-old plastic cards and PIN codes.


Even when properties have some connected systems in place, hardware can still vary across different rooms, floors, and buildings. And rarely do operators have the luxury of upgrading all their infrastructure — from locks to encoders, readers, and management software — all in one go. This fragmentation makes it nearly impossible to ensure everything runs smoothly, all the time, let alone, roll out digital keys at scale.


Seam directly addresses many of these challenges — enabling on-premise systems to behave like cloud-based solutions through Seam Bridge, and bridging the fragmentation caused by diverse, incompatible systems.


## Engineering


What complicates digital keys even further is that what works for one vendor doesn't work for another. Each lock manufacturer has their own API, authentication requirements, capabilities, and quirks. So if you want to get digital keys up and running for multiple smart lock brands in your portfolio, you have to build and maintain separate integrations for each brand. And as lock manufacturers update their systems, push out new firmware, or change their APIs, your integrations break — and then you’re back to square one.


Seam removes the complexity of working with multiple smart lock vendors by offering a unified API that supports all major brands, along with a suite of tools to manage and deploy access at scale. It includes a cloud-based dashboard for monitoring devices, built-in developer tools for testing and integration, prebuilt UI components for faster front-end development, and Seam Bridge to bring on-premise systems online. With Seam, you avoid maintaining fragmented integrations and can reliably support digital keys across any property or lock type.


## User Experience


It takes almost zero effort to download an app and go through an onboarding flow. But when you’re tired from traveling, the thought of downloading another app is overwhelming. And as long as the burden of work is on the guest, even the best BLE systems will go unused — no matter how much “convenience” they promise.


No wonder hospitality organizations have struggled to adopt digital keys.


Seam solves this by offering state-of-the-art access experiences tailored to each guest journey — all unified under a single platform. For guests who don’t want to download an app, Instant Keys deliver app-free, BLE-powered mobile keys that work instantly from a browser. For deeper integrations, Seam also supports standard mobile keys, as well as Apple Wallet keys for the most seamless, native experience. Whether it’s a quick overnight stay or a premium check-in flow, Seam ensures access feels effortless — with the right UX for the situation, and one product to manage it all.


# Paving the way forward


Addressing just one or two of these barriers to adoption is clearly not enough.


A solution that claims to deliver perfect guest experiences, but only works with specific hardware, will struggle to scale. A system compatible with all locks, but still requires app downloads, will annoy guests instead of delighting them. And any approach that forces an engineering team to build custom integrations for different lock vendors will quickly become too expensive, too time-consuming, and too much of a headache to maintain.


It is only when all three blockers are taken into account, all at the same time that digital keys will be able to deliver on their promises: that properties can implement them without being held to one hardware vendor, engineering teams can integrate multiple vendors and solutions without creating a mess on the back end, and guests can finally breeze past the front desk and get to their rooms — without a hitch.


This is the gap Seam is designed to help bridge. By combining a unified API, tools for working across diverse lock systems, and a range of modern key experiences — from app-free Instant Keys to Apple Wallet and mobile keys — Seam brings together the building blocks properties need to make digital access practical, scalable, and guest-friendly.


# About Seam


Seam provides a unified API infrastructure layer that solves all three barriers blocking digital key adoption in hospitality, making it easy for hospitality properties, of any size, to start using digital keys.


Through a single integration, hospitality owners and operators can deploy Instant Keys (no app download required), as well as support both online and offline lock systems from any manufacturer without hardware upgrades, and orchestrate any type of access credential — mobile keys, PIN codes, or plastic cards — across hundreds of lock types.


Let us show you how Seam can help you when you book a demo.
