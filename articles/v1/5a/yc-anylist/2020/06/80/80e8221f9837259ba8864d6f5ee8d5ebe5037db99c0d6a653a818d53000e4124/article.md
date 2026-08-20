---
schema_version: "1.0.0"
document_id: "80e8221f9837259ba8864d6f5ee8d5ebe5037db99c0d6a653a818d53000e4124"
company_key: "yc-anylist"
company: "AnyList"
source_id: "yc-anylist-rss-0111078d9e42"
canonical_url: "https://blog.anylist.com/2020/06/sign-in-with-apple/"
published_at: "2020-06-29T06:00:00+00:00"
first_seen_at: "2026-07-24T16:23:53.293792+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:5b09bf81b7443b0da149a6f93979043006f88fab5b08e9e824d84e7b001a2a2f"
---

# Why AnyList Won’t Be Supporting Sign In with Apple

Starting June 30th, Apple will be[enforcing a new rule](https://developer.apple.com/news/?id=03262020b) in the App Store requiring many apps to support Sign in with Apple. AnyList is one of the apps affected by this new rule, which means that we must either implement Sign in with Apple or make other changes to our app. After considering the merits of Sign in with Apple, we have decided not to support it. We understand that this may surprise some of our customers, so we’d like to explain in detail why we made this decision.


Third-party login systems like Sign in with Apple cause many user experience and customer support headaches. People don’t remember which login system they used to create their account. ( *“Hmm, I created this account a couple of years ago. Did I use my email address? Facebook account? Sign in with Apple?”* ) Simple questions like, *“How do I reset my password?”* no longer have simple answers and depend on which system you used to create your account, if you can remember. And if you get locked out of your account and used a third-party login system, we may not be able to help you ourselves and will instead have to direct you to another company, with all of the hassles that entails.


In addition to these customer experience problems that are common to all third-party login systems, Sign in with Apple introduces several more that are unique to it.


One problem is that most Apple IDs are tied to an iCloud email address. So most accounts created via Sign in with Apple will use an iCloud email address. But many of those iCloud email addresses are unused and unchecked, because a customer’s “real” email account is their Gmail, Yahoo, or Hotmail account. If we try to contact a customer using their iCloud email address, they may never see our message. We used to run into this problem constantly with customer support, back when AnyList used the built-in iOS email compose interface for sending support requests. This interface often defaults to using an iCloud email account. So people would ask for help, we’d reply, and they’d contact us again later, angry that we never replied. Our reply was going to their iCloud email account, but they didn’t see it because they only ever looked at their Gmail account, in the Gmail app.


Another issue is Sign in with Apple’s[“Hide My Email”](https://support.apple.com/en-us/HT210425) feature. With this feature, if you create an account with us, Apple will generate a special email address just for that account. So rather than your email address being *john.doe@icloud.com* , we will see your email address as something like *dpdcnf87nu@privaterelay.appleid.com* . While this is an intriguing idea that provides a measure of privacy, in practice it creates numerous support and user experience headaches. Here are a few:


-


If a customer contacts us asking for support, and we need to look up something in their account, typically we can just ask them for the email address on their account. But with “Hide My Email” that wouldn’t be easily possible, because the customer would have to figure out the *privaterelay.appleid.com* email address used for their account.


-


Furthermore, if there are platforms where AnyList doesn’t support Sign in with Apple, like Android, and someone wants to log into their account, they’d have to know their *privaterelay.appleid.com* email address. (And that certainly won’t be easy to find if you no longer have an iOS device.) And then they’d have to create a password with us, since they wouldn’t be able to sign in using Sign in with Apple.


-


Finally, for a service like AnyList, which is heavily focused on sharing lists with other people, the “Hide My Email” option greatly complicates collaboration. Typically, customers share a list by typing in the email address of the person they want to share with. If that person already has an account, the list is instantly shared. But with the “Hide My Email” option, your spouse or friends obviously won’t know your *privaterelay.appleid.com* email address, so when they enter your email address, our systems will believe that you don’t have an account. At that point, you’ll get an email from us asking you to create an account. If you accidentally create a new account, it won’t include the work you’ve done in your existing account created via Sign in with Apple. And if you manage not to make that mistake, then there would be a link between your email address and the account you created with Sign in with Apple, negating the value of hiding your email address.


We agree with Apple that[privacy is a fundamental human right](https://www.apple.com/privacy/) , and understand that the “Hide My Email” option in Sign in with Apple is well-intentioned, but it feels like Apple didn’t really think through all of the implications for basic user experience, customer support, and collaboration. At AnyList, we respect your privacy. We’re a small company that makes money when people like our app and pay for it. We do not make money with creepy tracking or by selling your information. When you provide us with your email address, it is **never** sold, shared, or used to invade your privacy.


Beyond customer experience, there are also many problems that Sign in with Apple creates for us as developers, which has knock-on effects for our customers.


First, implementing support for third-party login systems like Sign in with Apple significantly increases the complexity of handling user accounts in our systems. Instead of having one system for common operations like creating an account and signing in, supporting third-party login systems can quickly turn account management code into a rat’s nest, with special logic necessary to handle each different login system. This is especially true when supporting multiple third-party login systems (e.g., Facebook Login, Google Sign-In, Sign in with Twitter, etc.). This makes maintenance more difficult and error-prone, and if there’s any place where you cannot afford to have any errors, it’s in security-critical code like account management.


It’s also time consuming to implement support for a new third-party login system, particularly for a small company like ours that supports our app on multiple platforms. It’s not enough for us to add Sign in with Apple to our iOS app. We also have to add it to our web app, Mac app, and Android app. (Creating even more complexity, Apple does not provide any real solution for supporting Sign in with Apple on Android, see below.) So if we choose to support Sign in with Apple, that means that we have to spend a significant amount of time to get it working everywhere, rather than spending that time improving the core list, recipe, and meal planning functionality of our app.


As developers, we always have to do our due-diligence in evaluating the security implications of our work. In the last month a[massive security flaw](https://bhavukjain.com/blog/2020/05/30/zeroday-signin-with-apple/) was reported in Sign in with Apple, so serious that Apple paid $100,000 to the person who found it. If you read the linked report, you’ll see that this serious flaw was also very simple, which doesn’t provide a lot of confidence. For something as critical as a service for *logging into accounts* , it doesn’t seem wise to use an immature service (less than a year old) that has recently been the subject of a serious security flaw.


Another sign of Sign in with Apple’s immaturity is the sad state of the documentation for it. Good documentation is critical to facilitating developer adoption of any service. Since Apple is expecting developers to adopt this service by June 30th, it seems reasonable to expect decent documentation. Sadly,[like most of Apple’s recent developer documentation](https://mjtsai.com/blog/2019/05/20/the-state-of-apples-developer-documentation/) , it’s sorely lacking. For example, Apple vaguely states that you can implement Sign in with Apple on Android, but there is no direct documentation on how to do it. We understand that Apple probably doesn’t care much for Android, but if they are going to provide a login system, and are going to force developers of multi-platform apps to adopt it, then providing no real support for a major platform that these multi-platform apps run on is not acceptable.


Finally, from a policy perspective, Apple[explicitly states in their usage guidelines](https://developer.apple.com/sign-in-with-apple/usage-guidelines-for-websites-and-other-platforms/) , “Apple reserves the right to disable Sign in with Apple on a website or app for any reason at any time.” If customers cannot log into their accounts, then they can’t use our service. Giving a third-party such powerful control over a core part of our service when it’s not *absolutely* required is unnecessarily risky, in our view.


## What about Facebook Login?


At this juncture, you may be thinking:


- *“You have a point, but I use AnyList and it offers to let me sign in with Facebook. Doesn’t it suffer from many similar problems?”*
- *“Anyway, don’t the new App Store Review Guidelines require you to support Sign in with Apple, since you offer Facebook as a sign in option?*


These are both excellent points, and it’s absolutely true that some of the arguments above apply to creating an account via Facebook. That’s why we’re also announcing that we’ll be removing Facebook Login from AnyList. Our support for signing up using Facebook was begrudgingly added years ago as an experiment in offering another signup option, but we were never enthusiastic about it. That’s become even more true as time goes on, since Facebook constantly seems to be upping the ante with creepy privacy practices. We use the Facebook SDK to provide login functionality, and every new release of the SDK seems to add new tracking options that are turned on by default, which we have to take action to disable. Furthermore, the Facebook SDK has quality problems, and[recently caused a huge number of iOS apps to crash](https://www.theverge.com/2020/5/7/21250689/facebook-sdk-bug-ios-app-crash-apple-spotify-venmo-tiktok-tinder) due to a misconfigured server. You can expect to see an AnyList app update soon that removes Facebook Login.


We hope this post has helped to explain why we won’t be supporting Sign in with Apple. We’d love to hear your feedback on this post. If you have any comments or questions, please[contact us](https://www.anylist.com/contact) and we’ll get back to you.


---


Want to be informed when a new post is available? Sign up to be notified via email. Infrequent updates, no spam:
