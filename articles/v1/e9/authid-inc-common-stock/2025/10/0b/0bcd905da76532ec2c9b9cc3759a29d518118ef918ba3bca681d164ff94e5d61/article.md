---
schema_version: "1.0.0"
document_id: "0bcd905da76532ec2c9b9cc3759a29d518118ef918ba3bca681d164ff94e5d61"
company_key: "authid-inc-common-stock"
company: "authID Inc."
source_id: "authid-inc-common-stock-rss-c1a5aa3dd4ef"
canonical_url: "https://authid.ai/biometrics-are-a-great-idea-when-theyre-done-right/"
published_at: "2025-10-16T01:40:41+00:00"
first_seen_at: "2026-07-24T08:14:02.330532+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:1bcc4a06f7bea5edbd8c92c3d6b2ec3d0ed9a06c7a26510c23346b828659aa83"
---

# Biometrics Are a Great Idea… When They’re Done Right

My lovely wife and I just returned from an excellent trip to Montreal and Quebec City. It was our first foray out of the country since pre-Covid, and we were pretty excited. I spent some airline miles to take her there and back first class, to make it even more memorable. Lots of hiking, and way too much French food.


But weird thing … at the gate in Chicago, everyone had to stop at the door for a quick picture. Stand on the little square, look at the camera, and get snapped. The gate agent said it was to “expedite boarding.” And it did anything but.


Besides the very rude man running the process, it was challenging. It wasn’t just an extra step, which is hardly expeditious. It just plain didn’t work that well. Look left, look right. Not a successful take? Let’s try again. Okay, it took the picture, so now just hang on. Hang on a little longer. Okay, you can go now.


And in a couple of cases, it was, “Wait, come back, we have to do it again.”


So rather than expediting, this exercise in facial image capture was inhibiting.


On the surface, this kind of experience is bad enough. It annoys the user, who just wants to get on the plane. It holds up everybody behind them. Those people see this process bogging things down. And it pretty much ensures that nobody in line will have any confidence in the results at all.


On top of, nobody knew they’d be getting their picture taken in the first place, and therefore no consent was captured.


There’s one more thing a lot of people probably didn’t even think about. What are they comparing our faces to? Did I submit one earlier? No? Got it.


So what should a positive biometric capture and practical application look like? That’s easy to list, but for most providers, not easy to provide.


What’s the purpose in the first place?


First, to stay compliant with a variety of expanding compliance laws regarding biometric storage, you must capture consent. Laws in the US are largely inherited from the EU’s GDPR, which requires explaining to users what is being captured, for what purpose, for how long it will be used, how it will be securely stored, and how it may be deleted as needed. Garnering user consent also provides the public confidence in their privacy being protected.


Image capture must be fast and friendly. Make the experience a seamless, painless, intuitive one. Require no action from the user other than providing their face (and, when applicable, id document). As soon as you recognize the target, take the picture for them.


Process the images quickly. Don’t make the user wait, don’t make them perform multiple takes, and show the results as fast as possible. Speed is part of an overall experience.


Make it accurate. If the user is returning, ensure it’s really them. This protects the enterprise from fraud, as well as the user’s account from infiltration.


What are you comparing the images to? If it’s a first time registration, accurately compare a selfie to a document portrait. If it’s a returning user, make sure, again, it’s really them.


Don’t store actual faces when such a thing is forbidden. A variety of expanding compliance laws disallow storing actual faces and, in many cases, even certain hashes or metrics that might constitute biometric data.


This sounds difficult. But this is precisely what authID does.


We make the capture of document and facial images fast, easy, and accurate, with an intuitive UX that does all the work for the user. We process initial images in less than 700 milliseconds, for streamlined onboarding. We use the facial image as a seed to generate a public key that is stored, encrypted. This contains no biometric data whatsoever. When users return, we again use their face to generate a matching private key. Then, after an exchange of an encrypted message between keys, we validate the user (assuming a match) in less then 25 milliseconds.


Nothing is stored on the user’s device. No key, no face, no nothing. The private key is recreated with each visit, then discarded after use. Privacy and compliance are inherent parts of the system. Both enterprise and user are protected.


Passwords, KBA, SMS alone, these are in the rear view mirror. Their vulnerabilities account for the vast bulk of breaches, to the detriment of companies, agencies, and their users’ data. Biometrics are the future, if they’re done right.


That’s what authID does. We do biometrics right. Secure, accurate, friendly. Right.
