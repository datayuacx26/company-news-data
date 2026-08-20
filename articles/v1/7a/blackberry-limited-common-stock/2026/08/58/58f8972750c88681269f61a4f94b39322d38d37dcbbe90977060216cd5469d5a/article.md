---
schema_version: "1.0.0"
document_id: "58f8972750c88681269f61a4f94b39322d38d37dcbbe90977060216cd5469d5a"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/addressing-overconfidence-consumer-messaging-app-security"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T02:03:34.346972+00:00"
fetched_at: "2026-08-19T02:03:36.997027+00:00"
content_hash: "sha256:5bb2ec1543490b5ea1e1d8029b945c916980e7cfef690ec501f0f305092042d5"
---

# An Interview with Christine Gadsby on the State of Secure Communications

# An Interview with Christine Gadsby on the State of Secure Communications


In conversation with KBI, BlackBerry Chief Security Advisor Christine Gadsby discussed widespread misconceptions around consumer-grade messaging app security.


Aug 18, 2026


·


Blog


·


Christine Gadsby, Vice President and Chief Security Advisor, BlackBerry


## Key Takeaways


-


**Encryption does not protect against compromised devices**


-


**Metadata is increasingly valuable to attackers**


-


**Most organizations lack communications sovereignty**


-


**Crisis response often depends on consumer tools**


-


**Quantum migration planning needs to start now**


*[Reposted with permission from KBI Media.](https://kbi.media/access-granted/blackberry-state-of-secure-communications-2026/)*


### Karissa Breen: If employees are bypassing security and attackers don't need to break encryption, are organizations already on the back foot? Your research says we're seeing very high confidence in consumer-grade messaging apps, so that's a much lower understanding of what encryption actually protects. Are organizations overestimating how secure these platforms really are?


**Christine Gadsby** : Absolutely. That was one of the most shocking details in this report. 88 percent of security leaders express confidence in their messaging app security. But that's just built on a fundamental misread of what encryption does, because encryption protects data in transit, but it doesn't protect from phishing, account takeover from those apps, compromised devices, or metadata harvesting, which we are seeing.


> **That has resulted in fresh advisories from agencies in the U.S., the U.K., Europe, and now Australia that are specifically talking about state-backed attacks targeting Signal and WhatsApp accounts of their government officials and journalists.**


You see that playing out in the news almost daily now. Those are exactly the vectors that nation-state attackers are actively exploiting. Think of the armored car: You have a message — it's what's in the armored car, like from a bank. The armored car is protecting that message, but you can still see who the driver is. You can still track the vehicle as it's rolling down the street. You still know that it shows up for breakfast at nine o'clock in the morning every time at this one place. Attackers really watch that and that's the new vector that these nation-states are coming after.


There are fresh articles like one this morning about the Pentagon and U.S. service members being targeted. And Australia just released a couple of days ago that staffers were hacked in WhatsApp by a foreign state actor. This is coming up on a daily, regular basis now. That has resulted in fresh advisories from agencies in the U.S., the U.K., Europe, and now Australia that are specifically talking about state-backed attacks targeting Signal and WhatsApp accounts of their government officials and journalists. We are overestimating how secure those platforms are. That threat surface has really shifted from the apps themselves onto the network. That's where we're not having the hard conversations.


### KB: Do you think that from a few months ago when we spoke to now there is more understanding about the potential pitfalls and where this sort of sits in terms of risk?


**CG:** There is. As a culture in security, we take a long time to shift our mindsets. We're like the moat that's just floating out there. As we hear and digest things as security professionals, we start to look towards mitigations. The mindset is changing. We are seeing more attention towards it and the attitude towards the conversations we're having is shifting. I'll give you an example. We, and security professionals alike, love WhatsApp and Signal — we use them for different conversations. I talk to my children on those apps because they're convenient. We're not saying those aren't appropriate apps for all messaging. What we're saying is that some messages are not appropriate to be had on those apps — the ones that need to be secured at a different level.


### KB: If you communicate with your daughter around, "Hey, I'm going to pick you up from X at this time," it's fine. But then how do you get people to do that context switching to, "I'm moving into a more sensitive conversation," but we're already mid-conversation? Especially since COVID, that blur between having a casual chat and then business has all blended into one. Is it hard to say, "Now I need to move to a different platform to continue this conversation?"


**CG:** This is where threat modeling in an organization hasn't contextually gotten out of the enterprise — it hasn't left the building and reached the handset. Organizations have to look at the mobile device, and all of the things that happen on it, as an endpoint in its entirety that they need to manage. Not just, "Can we bring our own device and are we going to set up policies to make sure the operating system is up to date?" It needs to go well beyond that.


We need to take a step back when we talk about that behavior and ask what companies and governments have as a policy that says, "For these conversations, here's where you can have them and here's where you can't." That is the behavior we are starting to see shift.


It's baby steps, but I've been doing a lot of consulting around what policies governments and highly regulated organizations should have to direct staff — these are the kinds of appropriate conversations to have on free messaging apps after the work day and these are the types of conversations where we need to make sure device integrity is verified, identity is authenticated, and the encryption keys are held in our jurisdiction. That is what we're calling platform purpose — the platform purpose-built for the sensitivity of the work needs to match. We are seeing a shift in governments understanding that they need to drive that policy rather than just leaving it open.


### KB: What policies are you talking about when you're speaking with people in governments?


**CG:** We're really starting at the beginning — setting up policy for things like this. You need to analyze the conversations that you are having. I tell anybody who will listen the same thing: Your level of conversation is not going to be the same as other companies' or governments' levels of conversation. Even if you are running a roof shingle manufacturer, you are not having the same conversations over a device as the government of any entity. Those need to be modeled on their own.


The first step is understanding what your risk is. I've been having a lot of conversations around threat modeling. If your messages are compromised — and for anybody listening who hasn't started this, just pull up some news articles and ask yourself, "What would happen if that happened to us?" That's a great way to start threat modeling. I have seven categories that I put mobile messages into and then we threat model against all of those messages with the understanding of what happens if they're compromised, what happens if the metadata is compromised, what happens when the head of one government is talking to the head of another government and that metadata is compromised, and now attackers know the location of those individuals. That's really the best place to start.


That's not going to be the same for everyone — a roof shingle maker may collect PII on their customers that they need to protect, but that's a different level of threat modeling and it also means a different level of policy. That might just mean saying, "Employees, no sharing of personal identity or customer information that can be considered PII within messaging apps." Policy done. It's going to be more than that, but you get my point. For governments, it's going to be pages and pages of thought.


But what we are seeing is that once that level of transparency and clarity happens, the decisions that follow are much easier, because you can just look at it and say, "I really do need to take action."


### KB: And what is happening when people are compromised on that front?


**CG:** Those headlines are what we're seeing. The bigger picture of what is happening is much broader than what we are talking about.


You have a message from one device, a message from another device, all that metadata floating around on either side. That message is traveling over a network that we know is hostile. You can't throw a stick at any part of the world and find somewhere that hasn't had Salt Typhoon or some kind of APT threat actor compromise the network in some place or form. You've got a message starting across a hostile network to a message ending. What we're seeing is that coming to the surface — not just the message itself and the metadata, but the risk that that metadata travels across.


It's not just one problem — it's a bunch of problems. In a supply chain world of exploitation, which we have seen rise over the last three or four years, attackers are just getting smarter. They're testing the supply chain and saying, "You want one mitigation here? Fine, we're going to go plug in over here, because we can just bypass that." That is what's happening and it's going to continue to be more in an attacker's favor, mostly because some of that stuff isn't fixable.


### KB: There's another stat here: 88 percent say they're confident in app security for sensitive work. What does that confidence look like?


**CG:** At its purest form, that statistic is showing our misunderstanding of what encryption means. You hear the word *encryption* and you automatically associate it with, "Then I can talk about whatever I want," not really understanding what encryption means.


Take my armored car analogy: you can protect what's in that armored car, but it doesn't matter if it's the strongest armored car on the entire planet — if it's compromised, they can still get at what's inside. That is exactly how messages travel through a handset **.**


That 88 percent confidence is just the fact that we're not talking enough about what happens if the device is rooted or what happens when metadata attackers are going around the encryption. We aren't talking enough about the entire gap, instead of just saying, "Great, the message is encrypted end to end, that's enough." That's clearly not enough.


### KB: There have been some recent headlines around that being not enough and the proverbial wheels are starting to fall off. Would you say that customers, companies, and governments are starting to cotton on?


**CG:** We've had a lot of discussions around the culture shift that needs to happen. The problem is — it's not the fault of one thing — this is genuinely a culture problem, an encryption literacy gap, which we just talked about, and then really putting in the confidence of how to solve it. In security, we talk about these big broad-brush-stroke security problems, but we aren't always great at providing solutions or saying what we should be doing, because policy is always delayed and critical infrastructure moves slow — as it should, because they're cautious.


What we really have is a gap — a gap between app security confidence and a misunderstanding of what end-to-end encryption means and then a gap in policy and regulation around what we should be doing. What should our watermark or baseline be? What behavior should be happening? And then that's crosshatched with a bunch of things that need to change. It needs to be start-to-finish — it isn't just one thing. Where we start to get the confidence and practice piece — it's moving slow, but it is moving. Especially in the last month, we're seeing a huge uptick in exploits happening.


### KB: The last month of exploits, news headlines, and so forth — do you think that will have compound growth as we move forward through the rest of the year and beyond?


**CG:** I do, because the first stage in anything new in security is just accepting that we have a problem — and we're there. Now the ecosystem has accepted there's a problem and these compound. Now we're looking for it and that is another half of the battle. These new headlines that are starting to pop up — there was a great one recently, not a good thing, but a good article that got into some of the research around the Pentagon.


As those start to come out, we're going to get more reports of it because more people are watching for it. It doesn't necessarily mean it's happening more often. It means we're watching for it and we're seeing it. When people are on the ball and watching for it, I can say with certainty we will see more news headlines come out which will start changing behavior.


### KB: Do you think it's one of those things that slipped under the radar, but it's crept up on us as an industry and hit us hard?


**CG:** I do, and again, this is not the fault of any one area of the security industry. You can't point to one place and say we haven't done a good job. That is not the case at all. These apps are awesome. They allow people in the world to do what they need to do, which is communicate clearly and quickly, and we need that in our lives. And think about the fact that it's not just an app problem either — they're communicating over a hostile network.


Even if you take care of one problem, you still have to look at the others. We know Salt Typhoon has infiltrated all these cell networks. And to take one more step back, when you text somebody on WhatsApp, by design — whether it's sent over Wi-Fi or cell, it doesn't matter — it goes over this technology stream. It could stop and be handed over six or seven times. Some of these technologies are from the 1970s. By the nature of it, when you send a WhatsApp or Signal text, or just a regular text, it travels between your cell carrier over a 1970s network, makes six to eight jumps in some places, and then reaches another device with another carrier on a compromised network. Think about that: There are so many places an attacker can hide. And if they really want the metadata — which we know is what they're after, not the message itself — they're making dossiers and targeting specific individuals for attack patterns because they just want to know where you are at nine o'clock in the morning when you're sending that message. That’s what we're going to start seeing a shift on: Focus on the metadata.


### KB:


If I were to send you a message about something sensitive, am I safer to send you a text message or am I safer to send you something over these messaging applications?


**CG:** It depends. Every messaging app maker is collecting a different set of metadata — that's the first thing to consider. You send me a message on WhatsApp and I receive it on WhatsApp. WhatsApp/Meta is collecting all of that metadata. They are very open about it. This is not a secret — it's public knowledge. If you search what metadata WhatsApp is collecting, they're collecting it, they're selling it, and now they're using AI to harvest it with no opt-out. We are the money — we are their profit. It isn't free. The messaging apps are free for a reason: They're making money off the metadata.


What is the safest bet? The safest bet is to use a local messaging system that you own, where you own the keys and the metadata, and that metadata doesn't go anywhere. There are lots of solutions. We have one with BlackBerry ®


SecuSUITE® where BlackBerry administratively controls the metadata and it doesn't leave the building of where it's sent from. If I'm sending you a message on another BlackBerry SecuSUITE account, then your company owns that metadata on the other end — it isn't anywhere else. The encryption keys are stored locally, it's sovereign, and the encryption meets NSA-approved standards, so there's no question about whether it meets encryption requirements. That's the safest option. The least safe thing to do is to go WhatsApp-to-WhatsApp across other parts of the world, because how far the message travels depends on how many handovers it's making and each one adds to that attack surface.


### KB: 52 percent of respondents say they are concerned about telco infrastructure being monitored or disrupted by an adversary. Should there be regulation enforced on telcos to address these network concerns? You mentioned 1970s infrastructure. What's happening here?


**CG:** Some things defy gravity in security and this may be one of them. The concern is justified — 52 percent of our respondents flagged it and the evidence supports it.[Citizens Lab published research a few weeks ago identifying over 1,700 SS7 attacks](https://citizenlab.ca/research/uncovering-global-telecom-exploitation-by-covert-surveillance-actors/) — that's the handover technology in the network — from a single source in an 18-month window, over 92 percent of which were actual location tracking operations against real targets. That SS7 technology is literally the handover piece in this archaic network that we have built all our mobile communications on. Who owns that? Our actual telcos aren't necessarily responsible, because that technology spreads across the world. You can't really point at one part of it and say, "Go fix this 1970s technology that the world is built on," because it's like Jenga — you pull one piece out and the whole thing falls.


That said, tides are turning and things are happening. We are not without resolve. The UK Telecom Security Act is the most current model. It mandates carriers to implement security measures and report threats they see to the National Cyber Authority. That's the floor, not the ceiling. And I was excited to see that happen, because part of what I'd love to see more of is these telecoms sharing their intelligence — they see a lot. Are they sharing that with cyber authorities who can gather the data and report it back to people like us who are trying to solve the problem? What regulation alone can't solve is that visibility problem. Carrier-level access is effectively invisible to most enterprise and government security programs. The movement is entirely at the network layer and we know those carriers are compromised. Yes, there are steps they need to be taking. But we also need to understand that even if we force carriers to do some of this security work, their hands are tied — it defies gravity at some point at the network layer.


That said, the regulatory conversation absolutely needs to, at a minimum, extract that threat intelligence. We need carriers sharing it with each other and with governments because that will drive infrastructure hardening. That's where we need to start and we're seeing different levels of adoption from telcos, either voluntarily or, as in the UK, mandated by their Security Act. I applaud that work.


### KB: If you're saying that's the floor, then where's the ceiling, ideally?


**CG:** First, if they're sharing intelligence that allows action — because from the defender's standpoint, we just see the crime, we see the output of it. We don't see what the telcos are seeing. If they share the intelligence, the next step is coordinating that intelligence, and what comes out of that coordination will be policy. Because if enough of those telcos get together and start sharing intelligence and governments start gathering it, it allows them to surface the highest priorities and then begin enforcement of behavior change.


The icing on the cake is not just enforcing that behavioral change, but also looking longer term at how we address the places in the network that do defy gravity — how do we look at SS7 technology and what's the plan? And how do we look at areas of the world that are still on 2G connections? That is a reality: Some of the Global South hasn't had a technology upgrade to defend against any of this. We need to step in and help them with that basic technology gap.


### KB: Are the telcos going to gather around and come up with a better solution moving forward? What is their position now?


**CG:** Among the few telcos I've talked to — most have been in the US — there is a definite interest. At Mobile World Congress last year, there was a lot of talk on this. It was the first time in my career — and I've been serving telcos for a very long time, almost 20 years — that I actually saw the CEOs of these telcos show up at a World Congress and identify that we have a policy gap and a policy problem and we need to come to the table to look at how we develop policies that we can band together and support. I was excited to see that, because they have to be on the same page. They are changing their behavior. In the last few months, I've been contacted personally to help drive some of what should be in those policies. We are moving in the right direction — it's just slow.


### KB: What's stopping organizations from enforcing these policies, especially when employees are using consumer messaging apps for sensitive work?


**CG:** The enforcement part is a three- or maybe four-fold problem. You've got friction, because people are doing it constantly. There's fear of friction and the familiarity — it's easy, everybody knows the app, everybody uses it. There's a finance meeting at four o'clock and I need to message everybody. It's the quickest way to do it.


Then you've got implicit leadership-granted behavior — if you know your leader is doing it, then you're doing it. This is where the lack of policy comes in. It needs to be driven from the top and it needs a solution. Consumer apps aren't free — they're instant and they're universal, but they're not enterprise-grade and they were never designed to be enterprise-grade. They serve such an important purpose in our lives and connect people all over the world, but that really is a security leadership topic and that's where the biggest gap is.


> **Attackers aren't trying to break encryption and they don't need to.**


When senior officials use consumer apps for sensitive government business — and that's playing out publicly for the whole world to see — it tends to raise questions. That is what is stopping people. It really starts with policy: knowing where you want to be and driving the behavior change.


### KB: Speaking of gaps, 41 percent assume that encryption already protects compromised devices. What's the gap that people are missing here?


**CG:** The device is the gap. Encryption protects the pipe, not the endpoint. If a device is compromised through spyware, a zero-click exploit, or a malicious application running on the device, the attacker reads the message before it's encrypted on the sending side and after it's decrypted on the receiving side. Encryption is bypassed without being broken.


Attackers aren't trying to break encryption and they don't need to. They're well beyond that. They want to compromise the device, the account, the user. They want the metadata. CrowdStrike has documented this explicitly on the attacker side. The mental model is what needs to change here. What needs to shift is understanding that encryption is one control in a stack — without device integrity, verified identity, and network sovereignty underneath it, encryption is just a control in the stack, not a posture.


### KB: Sovereignty is high on the list as well: 55 percent say that sovereign control over communications is a priority. How are organizations addressing this?


**CG:** On a global scale we're seeing a lot of acknowledgment, a lot of realization that without sovereign control of communications infrastructure, it's impossible to secure. Whether it's legal jurisdiction or encryption keys held by a vendor rather than by you or looking at where data routes from — we had some big players this year have their hand forced on this. Some said, "Your data is not going to leave this country," and then couldn't prove it. That was a big step back and it made national news headlines. Then you started to see countries pull their data out because when push came to shove, it was marketing language.


But some governments are moving. European sovereign communications deployments are accelerating. As a company, we’re seeing more of that. Australia formally concluded in March 2025 that it hit their record-keeping standards, framing sovereignty as a governance issue, not just a security one. It's not just that they wanted sovereignty — they were saying, "If it's not sovereign, it doesn't pass our security controls."


Organizations, even non-government enterprises, are moving. They are looking at air-gapping sensitive environments and deploying purpose-built communications platforms.


Messages can go through consumer apps — that's fine — but purposely build a communications platform for when they shouldn't. If I were to call it a phase right now, it's the prioritization-to-action phase. Some parts of the world are moving very fast and are headstrong about it. Whoever isn't quite there yet is at least talking about it, so it's somewhere in the process.


### KB: But 98 percent are using platforms that can't provide sovereign control. What's going on? Why are these platforms even being used then, given that 55 percent of people are worried about it?


**CG:** That's an education piece. The gap between aspiration and action is where budget, procurement, and organizational decision-making live — and that's really where it's caught up. There's a lot of conversation about it, but we have to drive change.


Lots of conversation on platforms like Signal — which relies heavily on AWS, a US-headquartered infrastructure provider — and WhatsApp, owned by Meta. Neither offer any level of sovereignty and both were designed for consumer use. Organizations using those are starting to understand that.


The 98 percent number is the most alarming data point in our report. It means that sovereign control — which 55 percent of respondents call a priority — is something half the people say matters, yet 98 percent are using platforms that aren't capable of providing it. What closes that gap is part leadership mandate, part future deployment planning, and part using policy and governance to enforce it. The world is changing, and we're just slow to change. It's frustrating, but it is moving.


### KB: When a crisis hits, people's first response is to contact someone, usually via their mobile phone. 51 percent admit they lack a unified communications platform. How is this going to play out?


**CG:** This is the attack scenario we're not talking enough about, but one I see as a future problem. Attackers are smart — really smart. They know that coordination of security events is a great attack surface because it causes fatigue.


If you think about some of the toughest things that happen in the world daily — can you imagine people on the other end without a unified crisis response platform? They're using spreadsheets, making phone calls, using email — and attackers know about that fatigue. The best time to strike through an open window is during that fatigue session.


> **The gap between self-assessed readiness and actual capability is where the outcomes diverge.**


Human behavior when an incident breaks is to reach for that familiar tool, and what you get is a fragmented, uncoordinated, unaccountable response. These messaging apps aren't built for human safety and they're not built for accountability — for accounting for your people. You have 528 employees, everybody's on WhatsApp. Who's safe and who's not? How do you determine that? That is not something those platforms are capable of.


Our own research demonstrates this: in a multi-track simultaneous attack scenario, the 30-minute mitigation window isn't lost because of the exploit, it's lost because the response coordination breaks down across organizations and jurisdictions. A couple more stats in the report speak to this — 90 percent of organizations call themselves crisis-ready, but only 49 percent have a unified crisis communications platform. And what they're calling a unified crisis communications platform has a lot of things in it that aren't going to work. Those are self-assessments.


The gap between self-assessed readiness and actual capability is where the outcomes diverge. It requires things like what we've been talking about: independence from commercial networks that might themselves be part of the attack surface. If you're using those networks for a cyber crisis and your network goes down or it's a geography-based crisis and the cell phone network goes down, you can't use them at all. Coordinating that understanding across an organization is difficult, but it's another conversation we're having.


### KB: Sometimes I'm mid-conversation with someone on Instagram, then we're on LinkedIn, then Signal, Telegram, WhatsApp. I can imagine trying to disseminate a message when everything's gone wrong to thousands of people — how does anyone know where to look?


**CG:** Part of the challenge is getting leadership to look at those crisis response plans and analyze where they're dependent on call trees, free messaging apps, or email. I've been doing advisory work on this lately: how do we create an actual crisis response plan that will allow us to protect our people in any crisis event, whether that's a cyber outage, a tornado, or a flood? I have not yet come across one that doesn't depend on one of these technologies or worse, several technologies that may or may not be working. And your duty of care for your people — how are you reporting who's safe and who's not? That's also flowing back into spreadsheets. What happens when the spreadsheet isn't available? It's really just a bunch of taped-together, outdated critical event management pieces. Especially now, when we're looking at the threat of AI and what that's going to do to the cyber world in general — that is the crisis we're not talking enough about, and it is not going to be managed by pasting together spreadsheets and consumer tools.


### KB: Looking forward: quantum. It's a big topic starting to trickle through from a media point of view. 61 percent expect quantum computing threats within the next five years, but 78 percent haven't implemented defenses. Talk me through some of the stats here.


**CG:** This one was shocking to me, too. It's the same pattern we see across every long-horizon threat in security — we believe it's real and assume the runway is longer than it always is. The active risk we're talking about right now, similar to the messaging app problem, is not that a quantum computer is breaking your encryption today. It's the harvest-now, decrypt-later challenge. Adversaries — and we know this is happening because we're seeing it — are collecting and storing encrypted government and enterprise communications, possibly traveling over the compromised app ecosystem, with the explicit intention of decrypting them once quantum capability arrives. Let that sink in for a second. Criminals are thinking ahead and saying, "Eventually this is going to be breakable, so I'm going to stockpile now so that the day it is, I have a war chest."


The Federal Reserve published research in 2025 specifically on this. The G7 declared quantum security a priority for 2026 and NIST has finalized the first post-quantum cryptography standards. Those who are talking about it are aware and regulatory standards are moving. Most organizations, unfortunately, are not — so there's quite a big gap there.


The expert consensus puts relevant quantum capability around 2030, plus or minus two years. That sounds distant until you calculate how slow migration efforts are. Think about it: 2030, plus or minus two years, means potentially 2028 — that's two years from now. That's not much time at all.


All the conversations we've just had point to this. All those chats where we say, "The message is encrypted so it's safe" — that metadata is being attacked and those messages are absolutely being stored. The question isn't whether quantum is going to arrive and break things. It's whether you've migrated before it does.


### KB: How are we going to solve this problem when we're still looking at 1970s telco infrastructure?


**CG:** You've got to take it in bite-sized chunks. There's no magic wand that any of us in security have for all these problems. You have to take it in bite-sized chunks.


A few thoughts.


One: On quantum, the first step is taking inventory. A lot of companies, and even some government players, don't know what's encrypted and what's not or how to find that out within their own data stack. Step one: map where you need to make sure changes happen. Step two: hold your vendors accountable. If encryption is going to come into play with your vendors, ask them what their plan is to be quantum-ready. If they don't have one, that should tell you something — push them to develop one.


Two: On the messaging app ecosystem, the number one action is to make sure that metadata is administratively stored and controlled. Where is it? If it's flowing through free messaging apps right now, that's your first challenge. With that comes the need to shift the conversation away from treating encryption as our primary safety net. Attackers aren't targeting encryption the way they used to — they've moved on, saving it for *harvest now, decrypt later* . What they're really attacking is the metadata and the broader device. Shift your focus to how you are protecting the metadata of your employees in their mobile communications.


Three: Set policy. By setting policy, you're signaling to the ecosystem and the industry that you're going to enforce stronger security measures to protect your people. You're also sending an important message to the telcos, who are moving in the right direction. Support the telcos in sharing threat intelligence with their governments, so that intelligence can be coordinated and used to drive actual mandates for change — as we've seen the UK do.


Those are my three things that we as an industry can start pushing forward to drive change.


Get updates about the latest in-depth knowledge for secure communications.
