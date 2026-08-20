---
schema_version: "1.0.0"
document_id: "75f75d41c072ca294996f90dca843d70167e05706e5bd7cc64109510894f5a32"
company_key: "yc-onboard-io"
company: "Onboard.io"
source_id: "yc-onboard-io-news-import-03d553c3465d"
canonical_url: "https://onboard.io/blog/customer-health-score-key-metrics-for-saas"
published_at: "2023-01-30T00:00:00+00:00"
first_seen_at: "2026-07-22T07:14:30.527567+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:e7d0de6e3dfdf794c161d81db65c3fa38396ff5b4def628a1ae5670f04b153f7"
---

# Health Score: The Metrics That Actually Predict Churn

A customer health score is a composite number that estimates how likely an account is to renew, expand, or churn. It takes the signals you already have (how the customer uses the product, how often they contact support, whether they show up to calls, how long they've been with you), weights them by how much each one actually predicts an outcome, and rolls them into a single value you can track and act on.


That's the whole idea. The hard part is everything downstream: which signals, weighted how, triggering what, and starting when.


The last question is where most scores fail, and it's the one nobody asks.


## **What a health score is actually for**


The purpose isn't measurement. It's earliness.


Any team can tell you which customers are unhappy at renewal. That information is free, and it arrives too late to use. A health score earns its keep only if it surfaces the trajectory while there's still time to change it. The account drifting in month three, not the one that has already written the churn email.


So the test of a score isn't whether it's accurate. It's whether it's early. A score that perfectly identifies churn risk sixty days before renewal has told you something you probably already knew, at a point where your options are discounting and pleading.


**The signal it's working:** your CSMs are having conversations they wouldn't otherwise have had, about accounts that looked fine.


## **Why customers churn when your CSMs think the accounts are healthy**


This is the structural problem, and it explains the most frustrating pattern in customer success: the account your team swore was fine, right up until it wasn't.


Almost every health score starts collecting signal at launch, once the customer is live and generating usage data. That's when the inputs exist, so that's when the score begins.


But by then, a large share of the renewal decision has already been made.


Think about what the customer experienced before your score had a single data point. The kickoff that slipped two weeks. The integration that took a month longer than sales implied. The stakeholder who stopped attending calls in week three because the value hadn't shown up yet. The launch date that moved twice.


None of that reaches a post-launch health score. The customer goes live, starts generating usage data, and the score reads green, because it's measuring behavior that started after the damage was done. Then it drifts yellow in month four and everyone wonders what changed. Nothing changed in month four. It changed in week three, before anyone was looking.


Onboarding isn't a blank period. It's the densest signal you'll ever get, and the only one that arrives while you can still act on it:


-


**Tasks that stall** : and whose side they stall on


-


**Dates that move** : how often, and who moved them


-


**Stakeholders who go quiet** : particularly the ones who signed


-


**Time to first real value** : against what was promised in the sales cycle


If you can see those in real time, you have a health signal months before your usage-based score wakes up. If onboarding lives in spreadsheets and email threads, you can't, and the score starts, as it always has, too late to help.


**The signal it's working:** your earliest churn indicator fires during implementation, not after launch.


## **The five signal families**


Nearly every health score draws from the same five buckets. The list isn't the differentiator. What you do with it is.


### **1. Product usage**


Logins, feature depth, frequency, whether the product sits in the daily workflow or gets opened when someone remembers it exists.


Usage is the most common input and the most over-trusted. A login count tells you an event happened; it says nothing about why, and nothing about whether the login accomplished anything. Usage is a strong signal in combination and a weak one alone. It's the metric most likely to look fine right up until it doesn't.


The more useful version of this signal isn't volume, it's shape: is usage broadening across the account, or narrowing to one power user who will eventually leave?


### **2. Support activity**


Ticket volume, severity, time to resolution, and whether the same issue keeps coming back.


Support cuts both ways, which is why it's easy to score wrong. A customer filing tickets is a customer engaged enough to invest effort in your product, and that isn't a churn signal on its own. A customer who filed tickets and stopped might have gotten what they needed, or might have given up. Direction matters more than count, and severity matters more than either.


### **3. Relationship engagement**


Do they respond? Do they show up? Is the exec sponsor still on the call, or has it quietly become one junior person taking notes?


This is the signal your CSMs already carry in their heads and rarely write down. When a customer stops wanting the quarterly review, that's not a scheduling preference. That's a customer who has decided your product isn't worth an hour. Capture it.


### **4. Commercial behavior**


Upgrades, seat growth, renewals, expansion. The strongest positive signal you have, and a weak negative one.


An account that expands is telling you something real, because it costs them money to say it. But an account that hasn't expanded isn't necessarily unhealthy. It might just be the right size. Weight this heavily for expansion prediction and lightly for churn prediction. Many teams do the opposite by accident.


### **5. Tenure**


How long you've kept them. The simplest signal in the set.


Length of relationship implies continued value. Nobody renews four times by accident. It's also a lagging indicator by definition, and it's least useful for exactly the customers you most need to read: the new ones.


## **The weighting is the score**


Every health-score article gives you roughly the list above. Almost none tell you the part that determines whether the score works: how much each signal counts.


This is a judgment call, and your first version will be wrong. That's expected, and it isn't a reason to delay. A wrong score you review beats a perfect score you never ship.


What makes it less wrong is testing it backwards. Take the customers you've already lost. Run your scoring logic against what their signals looked like six months before they left. Did the score see it? How early? Then do the same for your best renewals, and check the score wasn't simply flagging everyone.


If your score didn't predict the churn you already know about, no amount of dashboard polish will make it predict the churn you don't.


**The signal it's working:** you can point to an account the score flagged, an action it triggered, and an outcome that changed.


## **Tiers, and the trap in the middle**


The most common structure is three tiers: green, yellow, red. It's popular because it's simple, and simple is right: a score with fifteen gradations requires training and interpretation, which means it won't get used consistently, which means it may as well not exist.


Three tiers, three responses:


-


**Red** : active intervention. Someone owns it, with a deadline.


-


**Green** : expansion or referral conversation. This is a signal, not a nap.


-


**Yellow** : the trap.


The middle tier is where health scores go to die. It's the biggest bucket, it has no obvious action attached, and “monitor” is what teams write down when they mean “ignore.” But yellow accounts don't stay yellow. Untended, they drift red, and they do it slowly enough that nobody notices the week it happened.


If your yellow tier has no defined action, you have a two-tier system with a large blind spot in the middle.


## **Three things that make it work**


None of these are about the math.


### **Keep it simple enough to be used**


Whatever you build has to be legible to a CSM on a Tuesday with forty accounts. If understanding the score requires understanding the model, it will be quietly abandoned in favor of gut feel, and gut feel doesn't scale past the number of customers one person can hold in their head.


### **Attach an action to every tier**


A score that doesn't change what anyone does is a dashboard. Before you ship it, know exactly what happens when an account turns red: who's notified, who owns it, what they do first, and by when. If that answer is vague, the score is decorative.


### **Review it against reality**


Schedule the recalibration or it won't happen. Once a quarter, check the score against actual outcomes: renewals, churns, expansions. A score built on last year's assumptions about your product is measuring a product you no longer sell.


## **Where this gets hard**


Every part of this is easy to agree with and hard to operationalize, because the signal that matters most is the one nobody's capturing. Onboarding progress lives in a project plan someone maintains by hand, or a thread, or a CSM's memory. It never becomes data, so it never becomes a score.


That's the gap[Onboard](https://onboard.io/product) closes: the plan, the owners, the deadlines, and the customer's own view in one place, so onboarding produces real signal while there's still time to use it, instead of a story you reconstruct after the renewal is lost.


[See how it works →](https://onboard.io/demo)


## **Conclusion**


A customer health score is a simple idea with a hard middle: pick signals you can measure, weight them by what actually predicts renewal in your business, tier them so each tier triggers a defined action, and recalibrate against real outcomes.


The differentiator isn't the metric list. Everyone has roughly the same one. It's how early the score can see. And on that dimension, the biggest available improvement for most teams isn't a better algorithm. It's starting at signature instead of launch, and treating onboarding as the first and most predictive health signal you have.
