---
schema_version: "1.0.0"
document_id: "a3b7b6921f540743459938a145ce939fdedd1be5fc4b246195129a6eea4c9240"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/feature-spotlight-unencrypted-sms-initiated-automation/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:fde2f6bff087d2eeaedb0a3d42e08eaa707b37df83e78deaefd58395d82dcf67"
---

# Feature Spotlight: Unencrypted SMS-initiated Automation

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### Feature Spotlight: Unencrypted SMS-initiated Automation


“


When you design for automation, you spend a lot of time looking at edge cases. 95% of the time, a standard approach will work, but for that other five percent, we need to get more creative. That’s how we approached the SMS-initiated PPP feature we introduced several years back. It allows you to remotely activate your out-of-band connection on a Local Manager by sending a text message over the cellular network via the Lantronix Control Center. We needed the feature because some customers weren’t able to use Pulse to automatically enable PPP, and others didn’t have any in-band connections at all. Imagine it: a completely network-isolated Local Manager only called into action by a text message that activates its out-of-band connection.


Wild stuff.


As with any solution, there are holes—edge cases. The biggest one is the Control Center itself. Under normal operation, you can log in and send these SMS messages every day all day. But what happens when the Control Center goes down, or more likely, the network between the user and the Control Center? For security reasons, SMS-initiated PPP uses an encrypted message to signal the Local Manager to turn on its out-of-band connection. Only the Control Center knows how to properly encrypt that message. You can’t just text the word *pppOn* to any Local Manager in the world and expect it to do something; this isn’t NORAD in the 80s and you’re not Matthew Broderick.


To that end, we introduced the High Availability Control Center—two separate instances running in (hopefully) different geographic regions. Losing a VM server or network connection at one site is understandable, but two at the same time? Not in *your* five-nines network!


Except… improbable doesn’t mean impossible, so we have to be ready when the absolute worst happens. When you’re isolated from the Control Center, whether through network failure or because you’re on the road and your VPN isn’t working, you still need to be able to turn on PPP at a remote site when you need access.


This brings us to LMS version 6.2, which added a feature to the rules engine that allows it to read **unencrypted SMS messages** and take predetermined actions. I did a full write-up on the feature here, but the gist is that because the message is now unencrypted, *any device capable of sending text messages can trigger your Local Manager to take action.* Want to use your iPhone? Done! Send an email to an SMS gateway using Gmail? Done! Type your message into Microsoft Excel like Kelly Rowland? No, sorry, that doesn’t work (or does it?).


By making the sms.message object part of the rules engine, you can use it to trigger any of the available actions when the rule runs—like a reboot, pull tech, or even pppOn. Now, you’ll probably *not* want to use a related word like “pppOn” to trigger pppOn; something obfuscated like “operationPandaBear1984” might make more sense. The important thing to remember is that you will configure the rule to look for a specific text string, and that string can be anything. When that string is detected, other actions take place, so it’s not like you can trigger a reboot on a Local Manager by texting it the message *reboot* . That is, unless you specifically craft a rule like that. Pro-tip? Don’t.


Going beyond the use case of “unencrypted SMS-initiated PPP,” you can start to feed the SMS trigger out to other resources on the Local Manager, allowing the rules engine to take action on different ports when a system-wide variable is set. You could conceivably write rules to power off all routers and switches when the Local Manager receives a test saying “runSilent.” The rules engine is already pretty powerful, and now it has yet another condition it can evaluate.


We’re sure there are still more edge cases out there that we haven’t addressed; it wouldn’t be any fun if there weren’t. But at least now we can cross one more off our list. We don’t expect any customer to encounter the “both Control Centers are down” scenario, but if you do, you won’t be locked out of initiating PPP at remote sites via SMS just because the Control Center isn’t able to send email (hey look, SMTP is another weak link!).


If you’d like to learn more about unencrypted SMS, take a look at my write-up, and if you have any questions,[shoot our support team a note](https://www.lantronix.com/technical-support/) .


“
