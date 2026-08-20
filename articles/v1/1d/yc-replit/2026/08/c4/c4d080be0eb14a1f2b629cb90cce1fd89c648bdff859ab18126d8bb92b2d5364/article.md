---
schema_version: "1.0.0"
document_id: "c4d080be0eb14a1f2b629cb90cce1fd89c648bdff859ab18126d8bb92b2d5364"
company_key: "yc-replit"
company: "Replit"
source_id: "yc-replit-news-import-9d99ff8f4466"
canonical_url: "https://replit.com/blog/black-box-pen-tests"
published_at: "2026-08-17T16:52:49.101+00:00"
first_seen_at: "2026-08-18T02:59:46.934003+00:00"
fetched_at: "2026-08-18T02:59:48.718038+00:00"
content_hash: "sha256:46e40c5d4424ef12579a005e0d4c54dca4c1fe46e415d48de255b776e189d0a3"
---

# Black-box pen tests on Replit

Replit enables you to build apps to create and launch apps that hold real customer data within a day.


Before coding agents, a full pre-launch security review required procuring a pen test from a vendor that would cost thousands, and weeks of back-and-forth. A penetration (”pen”) test is a safe, authorized mock cyberattack that helps you fix security flaws before bad hackers find them.


As we have democratized software creation, we have also sought to democratize securing it. A new generation of creators needs to ensure their apps are hardened against attackers, who look at apps built with AI as an opportunity as a potential opportunity.


Our[existing scans](https://replit.com/blog/meet-replit-security-agent) have an agent read your code and look for patterns that tend to be dangerous. That catches a lot, and it runs every time you build.


But a malicious hacker doesn’t see your code. Instead, they open your app and start poking around to find any doors you might have left unlocked. It’s a totally different way of searching that leads to different results. The gap between these two approaches is where most actual break-ins happen: even if nothing in the code looks wrong, the app might still hand over data it shouldn’t.


## Your built-in security team


Replit can now run **black-box pen tests** : security scans that test your Replit apps the way an external attacker would, over the network, through a browser, with no access to your app's source materials. This supplements our existing security scans that have full access to your codebase, called white-box scans. Together, Replit gives every builder a built-in security team.


To get started, run a “Level 3” scan from your project’s Security Center. This will spin up two parallel scans in parallel: a white-box scan with full access to your code, and a black-box scan, which gets one input: your app's link.


Replit runs each scan against a full copy of your app running in a private sandbox, so nothing it tries can escape or reach your users. The black-box scanner first clicks through the app while watching every request it sends, which reveals the features your app really has, including the ones with no button. It works out what your app is built with and goes after the failures common to that setup.


The agent runs once with no account, checking what any visitor can reach. Then, it tries again as an ordinary authenticated user, checking whether that user can open someone else's records or reach admin-only areas. When it finishes, you receive a short list of confirmed problems that you can review and fix with the Replit Agent.


## Security findings between both scans rarely overlap


Running both scans against the same apps showed a split we did not expect.


The source-code (white-box) scan is strong at detecting flaws in subtle logic. In one case, the white-box scan caught that a user whose access had been revoked could keep operating, because the app never rechecked whether their old sign-in was still valid. That is a genuinely hard bug to spot.


Meanwhile, the black-box agent found the admin dashboard sitting at a guessable address with no login on it. The source scan had read that same page and moved on, because nothing about the code was wrong. Only a person typing the address finds out that anyone can walk in.


In a second example of a multiplayer game built on Replit, only the black-box scanner found that you could flood an endpoint to crash everyone’s ongoing match.


Deep flaws and unlocked doors are different problems, and you can only get both sides by giving agents different sets of access and instructions.


## Get Started


The black-box pen test joins white-box security scans, and all of the capabilities in Replit Auto-Protect to keep your built applications secure: malicious package firewall, WAF firewall, and SSL/TLS encryption.


There are now 3 levels of ad hoc security scans that you can run in Replit:


- Level 1 scans run dependency checks and a static code analysis scan. It’s free.
- Level 2 scans run a deep security scan led by the white-box agent scanner.
- Level 3 scans run both the white-box scanner and black-box scanner at once.


Learn more at[replit.com/security](http://replit.com/security)
