---
schema_version: "1.0.0"
document_id: "e7b0c94bdcf3d2d8510cccd55bc80b48351ba9a00e215022d28cf2ff0fd89974"
company_key: "cognyte-software-ltd-ordinary-shares"
company: "Cognyte Software Ltd."
source_id: "cognyte-software-ltd-ordinary-shares-rss-bad823fa2500"
canonical_url: "https://www.cognyte.com/blog/luminar-2026-threat-landscape-report-audio-brief/"
published_at: "2026-07-16T12:36:31+00:00"
first_seen_at: "2026-07-20T23:17:32.120590+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:abe018853ceae2e90b89d964320e0f3ba65f12ab50751788d31e57730eb57c91"
---

# LUMINAR 2026 Threat Landscape Report Audio Brief

## Cybercrime Goes Silent and Automated.


In this episode you’ll learn key insights to strengthen detection, prioritize threats and stay ahead of emerging attack pattern trends from thousands of real-world incidents. The full report provide SOC teams, threat hunters, CISOs, security leaders and cybersecurity teams with a clear view of the risks and the solutions that they can implement.


[Read the full report](https://www.cognyte.com/resources/luminar-2026-threat-landscape-report/)
[Watch it on YouTube](https://youtu.be/LFHNOLHG2uo)


**Transcript**


\[Intro, Speaker 3\] (0:00 – 0:25)


Welcome to Cognyte Audio Briefs, where we unpack what’s happening around the world and the forces shaping today’s security landscape. Each episode explores how technology can help law enforcement, intelligence, and military agencies turn complex data into clarity, insight, and decisive action.


\[Speaker 2\] (0:26 – 0:38)


You know, when you picture a major heist, there’s usually this expectation of physical drama. Like, you imagine a crew in ski masks kicking down the doors of a bank vault.


\[Speaker 1\] (0:38 – 0:40)


Right, yeah, alarms blaring everywhere.


\[Speaker 2\] (0:40 – 0:44)


Exactly, someone yelling at everyone to get on the floor. You know you’re being robbed.


\[Speaker 1\] (0:44 – 0:48)


It’s loud. I mean, it’s visceral. There’s a highly visible point of intrusion.


\[Speaker 2\] (0:48 – 0:57)


Yeah, but step into the modern digital threat landscape and, well, that whole dramatic scene just vanishes. You aren’t dealing with a crew holding shotguns anymore.


\[Speaker 1\] (0:58 – 0:58)


No, not at all.


\[Speaker 2\] (0:58 – 1:07)


You’re dealing with an invisible, silent algorithm that is actively emptying your vault while the bank manager is still in the break room, like, pouring their morning coffee.


\[Speaker 1\] (1:07 – 1:09)


That is exactly what’s happening, and it’s happening at scale.


\[Speaker 2\] (1:09 – 1:31)


It really is. And whether you are a CISO trying to defend a Fortune 500 network, or maybe a threat hunter tracking digital syndicates, or, you know, just someone insanely curious about how the digital world is holding together right now, consider this your ultimate cheat sheet. Today, our mission is to unpack Luminar 2026 Threat Landscape Report by Cognyte.


\[Speaker 1\] (1:31 – 1:45)


And it’s really a monumental piece of research. The Luminar Threat Intelligence Group didn’t just summarize trends. They forensically analyzed 2,327 distinct cyber incidents from 2025.


\[Speaker 2\] (1:45 – 1:47)


Which is just a staggering amount of data.


\[Speaker 1\] (1:47 – 1:48)


It is.


\[Speaker 2\] (1:48 – 1:53)


And the goal here is to give you actionable intelligence on how these threat actors are operating right now.


\[Speaker 1\] (1:53 – 2:01)


Okay, let’s unpack this. The core thesis we are looking at today is that we are no longer dealing with lone hackers sitting in dark rooms in hoodies. Right, that era is over.


\[Speaker 2\] (2:01 – 2:08)


Yeah, we are facing highly automated, AI-driven, corporate-style syndicates and, honestly, fully funded nation-state armies.


\[Speaker 1\] (2:09 – 2:19)


And looking through those 2,327 incidents, the most jarring takeaway isn’t even the sheer volume of the attacks. I mean, the volume is huge, but it’s the fundamental structural shift in the execution.


\[Speaker 2\] (2:19 – 2:19)


The way they’re doing it.


\[Speaker 1\] (2:20 – 2:27)


Exactly. We are looking at the complete weaponization of artificial intelligence. It has essentially altered the physics of cyber warfare.


\[Speaker 2\] (2:27 – 2:28)


Because of the speed, right?


\[Speaker 1\] (2:28 – 2:35)


Yes. Because the attacker is now moving at machine speed, and the victim is, well, still moving at human speed.


### Inside the AI Watershed Moment: GTG-1002 and Fragmented Prompt Injection


\[Speaker 2\] (2:36 – 2:44)


Right. I was reading this section on the AI watershed moment from September 2025, and it completely broke my mental model of how espionage works.


\[Speaker 1\] (2:44 – 2:45)


A huge wake-up call.


\[Speaker 2\] (2:45 – 3:03)


Yeah. The report details this Chinese-linked threat actor called GTG-1002. They targeted about 30 different organizations, large tech companies, financial institutions, chemical manufacturers.


But they used Anthropic’s Claude Code to automate, like, 80-90% of the entire attack.


\[Speaker 1\] (3:03 – 3:04)


Which is just unprecedented.


\[Speaker 2\] (3:05 – 3:12)


My immediate thought was, how does a commercial AI not instantly flag that? I mean, these models have guardrails. If you ask an LLM to hack a bank, it politely refuses.


\[Speaker 1\] (3:12 – 3:26)


Right, they do have guardrails. But they’re primarily based on the context window of the prompt. If the AI detects malicious intent holistically, it shuts down.


So, GTG-1002 bypassed this through a technique called fragmented prompt injection.


\[Speaker 2\] (3:26 – 3:28)


Wait, how does that actually work?


\[Speaker 1\] (3:28 – 3:36)


Well, they didn’t ask Claude to execute an espionage campaign. They broke the attack down into hundreds of itemized, seemingly innocent micro-tasks.


\[Speaker 2\] (3:36 – 3:39)


Give me an example of what that looks like in practice. Sure.


\[Speaker 1\] (3:39 – 3:48)


So, instead of saying, you know, find vulnerabilities in the server, they would prompt the AI to write a benign script to parse network traffic.


\[Speaker 2\] (3:48 – 3:49)


Okay, totally innocent on its own.


\[Speaker 1\] (3:49 – 4:05)


Right. Then, a separate prompt asking it to evaluate a specific memory allocation method. Then, another asking it to optimize a reject search.


The LLM’s context window never sees the overarching malicious intent.


\[Speaker 2\] (4:05 – 4:07)


Oh, wow. So, it’s blind to the big picture.


\[Speaker 1\] (4:07 – 4:18)


Exactly. And Claude processed thousands of these requests per second, scanning, searching, analyzing code. The human operators were barely involved.


They just maintained a supervisory role.


\[Speaker 2\] (4:18 – 4:20)


Just watching the progress bar, basically.


\[Speaker 1\] (4:20 – 4:24)


Yeah, stepping in only to authorize the final exploitation of the harvested credentials.


\[Speaker 2\] (4:24 – 4:41)


So, it’s like, let me see if this analogy works. Is this like AI graduating from being just the getaway driver who maybe hands you a map and gives you some navigation advice to suddenly becoming the actual mastermind coordinating the entire bank heist from a remote location, directing every piece of the puzzle?


\[Speaker 1\] (4:42 – 4:53)


That is the perfect way to contextualize it. In 2024, AI was a support tool. In 2025, it became the active orchestrator.


And it is pervasive.


### AI-Generated Attacks Become the New Normal


\[Speaker 2\] (4:54 – 4:55)


The numbers are wild.


\[Speaker 1\] (4:55 – 5:05)


They are. The Luminar report notes that 82.6% of phishing emails are now leveraging AI-generated content to bypass spam filters and mimic human communication flawlessly.


\[Speaker 2\] (5:06 – 5:07)


So, you can’t even look for bad grammar anymore.


\[Speaker 1\] (5:07 – 5:21)


Nope. We also saw the first fully AI-driven ransomware variant named PromptLock. And if you look at the criminal underground, 80% of ransomware-as-a-service groups are actively promoting AI automation features to their affiliates.


\[Speaker 2\] (5:21 – 5:22)


That’s basically marketing.


\[Speaker 1\] (5:23 – 5:30)


It is. You can go on a dark web Telegram channel right now and rent a malicious, uncensored LLM like WormGPT for $50 a month.


### Defensive AI Levels the Playing Field


\[Speaker 2\] (5:30 – 5:38)


That is terrifyingly cheap. But, you know, there is a flip side to this. The defenders are also deploying AI, right?


Because, as you said, humans can’t fight algorithms.


\[Speaker 1\] (5:38 – 5:48)


Right. And what’s fascinating here is the sheer reality of the dual-use dilemma. An LLM processing thousands of iterations per second is a speed a human analyst simply cannot counter by manually reviewing logs.


\[Speaker 2\] (5:48 – 5:50)


It’s just physically impossible.


\[Speaker 1\] (5:50 – 6:09)


Exactly. So, we are seeing incredible strides in defensive automation. Google’s LLM framework, Big Sleep, actually found an exploitable zero-day vulnerability in the SQ Lite database engine that’s CVE-2025-6965, and it did so autonomously before threat actors could find it.


\[Speaker 2\] (6:09 – 6:12)


Wow. The AI found the hole first.


\[Speaker 1\] (6:12 – 6:25)


Yeah. And OpenAI also released Aardvark, which is a GPT-5-powered agent designed to hunt down vulnerabilities in enterprise source code and automatically deploy patches. We are entering an era of automated attacks being met by automated remediation.


\[Speaker 2\] (6:26 – 6:26)


Machine versus machine.


### Ransomware Extortion Hits Record Highs
\[Speaker 1\] (6:27 – 6:27)


Exactly.


\[Speaker 2\] (6:27 – 6:41)


Now, the sheer speed of these AI agents doesn’t just change how attacks are executed. It fundamentally alters the economics of the fallout. Because AI allows these syndicates to scale their operations so drastically, we are seeing an unprecedented boom in financial extortion.


\[Speaker 1\] (6:42 – 6:43)


A massive surge.


\[Speaker 2\] (6:43 – 6:48)


Yeah. Ransomware operators claimed responsibility for targeting 7,809 organizations in 2025.


\[Speaker 1\] (6:48 – 6:59)


That is a 27.3% jump from 2024. And, you know, they didn’t just lock systems, they exfiltrated data. Among the incidents where groups disclosed the volume, they stole approximately 463 terabytes of data.


\[Speaker 2\] (7:00 – 7:12)


Which is just an ocean of sensitive info. And the hierarchy of who is doing the stealing has completely shifted. Last year, Ransomhub was the dominant player.


But in 2025, a group called Chilin took the crown.


\[Speaker 1\] (7:12 – 7:14)


Chilin was incredibly aggressive.


\[Speaker 2\] (7:14 – 7:31)


They really were. They executed 1,004 reported incidents. That is a 64% increase over Ransomhub’s previous numbers.


They hit Japan’s Asahi Group holdings, stealing 27 gigabytes of critical data and disrupting their entire domestic operations to the point where orders had to be processed manually.


\[Speaker 1\] (7:32 – 7:36)


Yeah. We also saw the catastrophic hit on the UK’s Marks & Spencer by Dragonforce.


\[Speaker 2\] (7:36 – 7:37)


That one was brutal.


\[Speaker 1\] (7:37 – 7:51)


It resulted in an estimated 300 million in lost profits. But beyond individual groups, the most significant structural change to the limit our team observed is the convergence of these actors. Scattered Spider, Lapsus, and Shiny Hunters actually merged into a coalition.


### Extortion-as-a-Service: Cybercrime’s Franchise Model
\[Speaker 2\] (7:51 – 8:11)


Right. And they’re operating under this new model called Extortion as a Service, or ES. But wait, help me understand the logistics here.


If Scattered Spider and Shiny Hunters merged, who is actually building the malware versus who is doing the negotiating? How does the profit-sharing actually work in an ES model?


\[Speaker 1\] (8:11 – 8:23)


It operates exactly like a sauce tech franchise. The core coalition, the developers, they build the malware payload, maintain the dark web data leak site, and manage the cryptographic keys.


\[Speaker 2\] (8:23 – 8:24)


But they don’t do the hacking.


\[Speaker 1\] (8:24 – 8:35)


No, they don’t actually do the hacking. Instead, they lease their infrastructure to affiliates who act as initial access brokers. The affiliate finds the vulnerability in a target network and deploys the ransomware.


\[Speaker 2\] (8:35 – 8:37)


Okay, so the affiliate is the one kicking in the door.


\[Speaker 1\] (8:37 – 8:47)


Right. And then when the victim pays the ransom, a smart contract on the blockchain automatically splits the cryptocurrency, say, 80% to the affiliate and 20% to the core developers.


\[Speaker 2\] (8:48 – 8:48)


Automatic.


\[Speaker 1\] (8:48 – 8:49)


Automatically built right into the code.


\[Speaker 2\] (8:50 – 9:04)


That is a terrifyingly efficient business model and highly opportunistic. We saw the result of that efficiency in February of 2025. There was an astronomical spike, 1,039 ransomware attacks in a single month.


\[Speaker 1\] (9:04 – 9:05)


Yes, that was a huge month.


\[Speaker 2\] (9:05 – 9:31)


And it was largely driven by a zero-day vulnerability in Cleo-managed file transfer products. That was CVE-2024-50623. Threat actors like Clop aggressively exploited it.


But let me push back on something here. If these groups are so well-known and their tactics are heavily publicized, why does the report say that manufacturing at 14% and technology at 13% are still the top-targeted sectors? Shouldn’t these industries have the budgets to lock their doors?


\[Speaker 1\] (9:31 – 9:42)


You would think so, but their architecture works against them. Manufacturing and tech carry immense technical debt. They rely on sprawling networks of unsecured IoT devices and complex, interconnected supply chains.


\[Speaker 2\] (9:42 – 9:43)


So there’s just a lot of surface area.


\[Speaker 1\] (9:44 – 9:59)


Exactly. Every smart sensor on a factory floor or legacy system controlling an assembly line is a potential entry point. The adversary ecosystem operates just like a modern corporation.


They find the most interconnected targets where operational downtime is financially disastrous.


\[Speaker 2\] (9:59 – 10:03)


Because they know those companies are the most likely to pay the ransom quickly.


\[Speaker 1\] (10:03 – 10:05)


Precisely. Time is money for them.


\[Speaker 2\] (10:05 – 10:18)


Which means to execute these corporate-style extortion campaigns, these syndicates are constantly hunting for those exact entry points. And that leads us directly into the explosion of software vulnerabilities and supply chain dominoes.


### The Vulnerability Explosion and the Rise of React2Shell


\[Speaker 1\] (10:18 – 10:29)


Right. The vulnerability landscape in 2025 was incredibly volatile. The report notes a 23% year-over-year increase in disclosed vulnerabilities, hitting 49,972 total.


\[Speaker 3\] (10:29 – 10:30)


Wow.


\[Speaker 1\] (10:30 – 10:31)


And the severity is escalating too.


\[Speaker 2\] (10:31 – 10:44)


The average CVSS score climbed to 6.6. And the software with the absolute highest number of vulnerabilities was the Linux kernel with 2,257. But what really caught my eye was what was happening on the dark web forums.


\[Speaker 1\] (10:44 – 10:46)


Oh yeah. The chatter there was very focused.


\[Speaker 2\] (10:46 – 11:00)


The absolute favorite topic of conversation among cybercriminals was a vulnerability known as React 2 Shell, CVE-2025-551-82. It accounted for nearly 30% of all vulnerability mentions tracked by Luminar.


\[Speaker 1\] (11:00 – 11:09)


Because React 2 Shell is an attacker’s dream scenario. It allows an unauthenticated attacker to execute arbitrary code on a server with a single HTTP request.


\[Speaker 2\] (11:09 – 11:14)


Break that down. How does a server just accept an unauthenticated request and hand over control?


\[Speaker 1\] (11:14 – 11:23)


It comes down to how the application parses incoming data. With React 2 Shell, the vulnerable endpoint processes the payload in the HTTP header before it ever checks the authentication token.


\[Speaker 2\] (11:24 – 11:26)


Ah, so it processes the malicious command first.


\[Speaker 1\] (11:26 – 11:35)


Exactly. So the attacker crafts a malicious command, embeds it in the header, and the server blindly executes it in memory. One request, and the attacker has a remote root shell.


\[Speaker 2\] (11:35 – 11:36)


That’s crazy.


\[Speaker 1\] (11:36 – 11:42)


It generated so much attention it dominated 38% of all discourse, if you include its duplicate entry.


### Supply Chain Attacks Bypass the Front Door
\[Speaker 2\] (11:42 – 11:49)


But the threat isn’t just about the software you build internally. It’s about the software you buy. Aren’t we just admitting perimeter defense is dead?


\[Speaker 1\] (11:49 – 11:57)


Perimeter defense as a standalone concept is absolutely dead. Third-party vendors are the ultimate force multipliers for an attacker.


\[Speaker 2\] (11:57 – 12:08)


Here’s where it gets really interesting. Securing your own servers but ignoring your supply chain is like installing a foot-thick steel vault door on your house, but leaving the air ducts wide open to the neighbor’s apartment.


\[Speaker 1\] (12:09 – 12:10)


That’s a great analogy.


\[Speaker 2\] (12:10 – 12:24)


Take the Scattered Spider and Lapsus coalition. They launched a voice phishing or vishing attack on Salesforce. They impersonated IT support, tricked employees into authorizing malicious OAuth applications, and generated legitimate access tokens.


\[Speaker 1\] (12:24 – 12:53)


And that OAuth token is the ventilation map in your analogy. If an attacker tricks a user into granting OAuth permissions to a malicious app, they don’t need to steal a password or bypass MFA at the front door. They already have the keys, right?


The token gives them direct API-level access to the database from the inside. Through that one vendor compromise at Salesforce, they established a data leak site listing 39 major brands, including Disney, Marriott, FedEx and Toyota.


\[Speaker 2\] (12:53 – 13:08)


Staggering collateral damage. And then there was Clop exploiting an Oracle EBS zero-day CVE 2025-61882. They used that supply chain link to hit dozens of organizations, including the Washington Post and Harvard University.


\[Speaker 1\] (13:09 – 13:13)


If we connect this to the bigger picture, what we are seeing is that the time to exploit is rapidly shrinking.


\[Speaker 2\] (13:13 – 13:14)


How fast are we talking?


\[Speaker 1\] (13:15 – 13:23)


Well, the report showed nearly 29% of known exploited vulnerabilities were reported on the exact day or the day before the CVE was officially published.


\[Speaker 2\] (13:23 – 13:24)


That is basically zero warning.


\[Speaker 1\] (13:24 – 13:41)


Zero. Because AI is lowering the technical barrier to code analysis, the capability to reverse engineer a patch and deploy a zero-day exploit is trickling down from advanced persistent threat groups to everyday criminals. Organizations are in a constant race against time.


### Nation-States Weaponize Cyberattacks in Global Conflicts
\[Speaker 2\] (13:42 – 13:54)


And the architecture of the Internet makes no distinction between a financial target and a military one. Those exact same vulnerabilities, like React2Shell, aren’t just being used by criminals trying to make a quick buck.


\[Speaker 1\] (13:54 – 13:56)


No, they absolutely are.


\[Speaker 2\] (13:56 – 14:00)


They are being weaponized by governments. Geopolitics is spilling directly onto the cyber battlefield.


\[Speaker 1\] (14:01 – 14:07)


The data here is striking. Globally, nation-states now account for 38% of all threat activity.


\[Speaker 3\] (14:07 – 14:07)


Wow.


\[Speaker 1\] (14:07 – 14:21)


But when you zoom in on specific regions, those numbers skyrocket. In the APAC region, nation-states are responsible for 67% of attacks. In the Middle East, it’s 56.6%.


\[Speaker 2\] (14:21 – 14:30)


Now, we need to be clear here for the listener.


We are looking at this purely through the lens of the data provided in the Luminar report, objectively mapping how these conflicts play out digitally without taking any political sides.


\[Speaker 1\] (14:30 – 14:31)


Absolutely, just following the technical footprints.


\[Speaker 2\] (14:32 – 14:45)


Right. And the report highlights some intense conflict-driven incidents. For example, during the hostilities between Israel and Iran in June 2025, Iranian hacking groups gained access to civilian security cameras in Israel.


\[Speaker 1\] (14:45 – 14:46)


And not for surveillance either.


\[Speaker 2\] (14:46 – 14:56)


Exactly. Their goal wasn’t to steal personal data. It was to visually locate missile landings, assess the kinetic impact, and improve the telemetry for their next barrage.


\[Speaker 1\] (14:57 – 14:59)


Turning civilian IOT into military reconnaissance.


\[Speaker 2\] (14:59 – 15:00)


Exactly.


\[Speaker 1\] (15:00 – 15:14)


We also saw this convergence of digital and physical warfare in Eastern Europe. Ukraine’s military intelligence agency, the HUR, executed a cyber attack, wiping huge volumes of data and overriding SCADA management servers at the Russian energy corporation, Gazprom.


\[Speaker 2\] (15:14 – 15:17)


And overriding SCADA, that’s physical hardware control, right?


\[Speaker 1\] (15:17 – 15:33)


Yes. By overriding SCADA safety protocols, digital code physically destroys hardware like overheating pipelines. Crucially, this digital attack was synchronized concurrently with kinetic military strikes against Russian energy infrastructure.


### Hacktivists Turn the Tables on State-Sponsored Hackers


\[Speaker 2\] (15:33 – 15:44)


It’s all connected now. There’s also this fascinating trend of hacktivists turning the tables. Independent threat actors are leaking the proprietary tools and identities of state-sponsored APT groups.


\[Speaker 1\] (15:44 – 15:46)


It’s a huge shift in power dynamics.


\[Speaker 2\] (15:46 – 15:59)


Yeah. A group called Kittenbusters leaked the daily activity logs and personal info of Iran’s APT35 Charming Kitten. Another group leaked 8.9 gigabytes of phishing kits and source code from North Korea’s Kimsuky.


\[Speaker 1\] (15:59 – 16:00)


And the Chinese firm, Knownsec, too.


\[Speaker 2\] (16:01 – 16:09)


Oh, yeah. A massive GitHub leak exposed 12,000 files from Knownsec, revealing the inner workings of their espionage operations across 20 countries.


\[Speaker 1\] (16:09 – 16:17)


So looking at all this data, are we looking at a future where every single physical kinetic conflict automatically has a shadow cyber war running parallel to it?


\[Speaker 2\] (16:17 – 16:20)


We aren’t just looking at that future. We are already living in it.


\[Speaker 1\] (16:20 – 16:32)


And the reason this matters to you, the listener, even if you run a completely civilian enterprise, is that civilian infrastructure is now dual use. The Iranian camera hack perfectly illustrates this.


\[Speaker 2\] (16:32 – 16:34)


Because it was just regular commercial gear.


\[Speaker 1\] (16:34 – 16:47)


Exactly. Low-effort breaches into insecure commercial equipment like standard surveillance cameras on a warehouse yield immediate real-time battlefield advantages. The commercial sector is now part of the military reconnaissance grid.


### The Dark Web Economy Shrinks but Gets More Exclusive


\[Speaker 2\] (16:47 – 17:01)


That is a sobering reality. But whether it’s a nation-state gathering intel for a missile strike or a ransomware gang stealing customer databases, the ultimate clearinghouse for all this compromised information is the dark web.


\[Speaker 1\] (17:01 – 17:02)


It all ends up there eventually.


\[Speaker 2\] (17:02 – 17:11)


Right. However, when the Luminar team analyzed the dark web economy for 2025, they found a completely counterintuitive twist.


\[Speaker 1\] (17:11 – 17:18)


They really did. Given the explosion in ransomware and supply chain attacks, you would logically expect the volume of stolen data for sale to skyrocket.


\[Speaker 2\] (17:18 – 17:21)


Obviously, more hacks means more data to sell.


\[Speaker 1\] (17:21 – 17:34)


Right. But instead, the report shows a massive 50.36% drop in stolen access credentials for sale. It plummeted from 13.9 million in 2024 down to roughly 7 million in 2025.


\[Speaker 2\] (17:34 – 17:41)


Which makes zero sense until you look at the enforcement data. Authorities are fighting back with unprecedented coordination.


\[Speaker 1\] (17:41 – 17:43)


It’s been a busy year for law enforcement.


\[Speaker 2\] (17:43 – 17:55)


A Europol-supported operation took down Cracked and Nulled two megaforums that hosted over 10 million members combined. They arrested key figures from breach forums and the shiny hunters group.


\[Speaker 1\] (17:55 – 17:56)


And they went after the money too.


\[Speaker 2\] (17:56 – 18:05)


Yeah. The U.S. Treasury sanctioned Russian crypto exchanges like Cryptex and PM2, BTC, seizing their servers to choke off the money laundering pipelines.


\[Speaker 1\] (18:05 – 18:16)


We also have to highlight Operation Endgame. Law enforcement agencies across Europe, North America and Australia launched major coordinated actions against botnet infrastructure, severely disrupting the Lumma InfoStealer network.


\[Speaker 2\] (18:17 – 18:24)


Wait, if Lumma was severely disrupted by Operation Endgame, how does the report state it still held 42% of the market share for InfoStealers in 2025?


\[Speaker 1\] (18:24 – 18:34)


It highlights the resilience of decentralized malware infrastructure. Even with the takedowns, Lumma had already infected over 394,000 Windows machines worldwide.


\[Speaker 2\] (18:35 – 18:36)


So the damage was already done.


\[Speaker 1\] (18:36 – 18:51)


Exactly. They utilized big browser updates and deceptive CAPTCHA pages to harvest VPN credentials, session cookies, and crypto wallets. The disruption hurt them, but the decentralized nature of their botnet allowed them to maintain dominance.


\[Speaker 2\] (18:51 – 18:53)


That’s incredibly frustrating.


\[Speaker 1\] (18:53 – 19:03)


It is. However, the intense law enforcement pressure is only part of the story. There is a second, purely economic reason for that 50% drop in credentials for sale.


\[Speaker 2\] (19:04 – 19:04)


So what does this all mean?


\[Speaker 1\] (19:05 – 19:16)


It means criminal administrators are actively changing their business models to survive. Historically, initial access brokers would repost the same stolen InfoStealer logs multiple times across different forums.


\[Speaker 2\] (19:16 – 19:18)


Just spamming the boards.


\[Speaker 1\] (19:18 – 19:30)


Right. But buyers grew tired of purchasing recycled or repackaged combo lists that no longer worked. To maintain their credibility and keep their premium pricing intact, the forum administrators started aggressively deduplicating their platforms.


\[Speaker 2\] (19:31 – 19:32)


Wait, so they’re cleaning up their own supply.


\[Speaker 1\] (19:33 – 19:44)


Yes. They are purging the recycled junk to ensure they’re only offering fresh, unique access credentials. They are prioritizing quality over quantity.


\[Speaker 2\] (19:44 – 19:57)


That is wild. Cybercriminals are essentially performing corporate quality assurance and market price fixing, acting exactly like a legitimate retail business trying to protect its brand reputation and retain customer loyalty.


\[Speaker 1\] (19:58 – 20:08)


This raises an important question about the resilience of these criminal networks. Law enforcement is playing an incredibly aggressive, high-stakes game of whack-a-mole, and they are landing major blows.


\[Speaker 2\] (20:08 – 20:09)


But it’s not stopping them.


\[Speaker 1\] (20:09 – 20:17)


No. The dark web economy isn’t collapsing. It is evolving.


It is becoming more curated, highly expensive, and strictly exclusive.


### Conclusion: Fighting Hostile AI With Defensive AI


\[Speaker 2\] (20:17 – 20:47)


Which brings us full circle. If we look at the entire Luminar 2026 Threat Landscape Report, the picture is incredibly clear. AI has fundamentally automated the speed and scale of attacks, turning algorithms into masterminds.


Ransomware syndicates have corporatized extortion into a highly efficient franchise model. Supply chains have become the primary weak links that let attackers bypass your front door entirely. And the dark web has evolved into a regulated, quality-controlled shadow economy.


\[Speaker 1\] (20:47 – 20:49)


It’s a completely new paradigm.


\[Speaker 2\] (20:49 – 20:58)


It is. Whether you are a CISO, a threat hunter, or just someone trying to keep your personal data secure, this is the battlefield you are walking onto right now.


\[Speaker 1\] (20:58 – 21:05)


It is an intricate, hostile ecosystem, and navigating it requires a proactive, unified approach to defense.


\[Speaker 2\] (21:05 – 21:16)


Absolutely. And that is exactly why you need to read the full Luminar 2026 Threat Landscape Report by Cognyte. It has all the technical details, the specific CVE mechanics, and the strategic trends you need to actually map your defenses.


\[Speaker 1\] (21:16 – 21:18)


Highly recommended reading.


\[Speaker 2\] (21:18 – 21:40)


Plus, it details Luminar’s AI-powered, unified threat intelligence solution. It combines external attack surface management, digital risk protection, and cyber threat intelligence, all paired with a designated expert analyst who actually understands your specific environment. It’s about fighting hostile AI with superior defensive AI and expert human context.


\[Speaker 1\] (21:40 – 21:45)


And as we close out today, I want to leave you with one final thought, based on what we’ve unpacked.


\[Speaker 2\] (21:45 – 21:45)


Let’s hear it.


\[Speaker 1\] (21:45 – 22:07)


We’ve talked a lot about AI agents automating the initial intrusion, and defensive AI agents automating the patching of vulnerabilities. But the logical endpoint of this arms race is staggering. What happens in a year or two when an attacking AI and a defending AI bypass the human operators entirely and simply negotiate the ransomware payment amongst themselves at machine speed?


\[Speaker 2\] (22:07 – 22:17)


Wow. Two invisible algorithms, quietly haggling over the price of the vault in milliseconds, while the rest of us are still pouring our coffee. Thank you so much for taking this deep dive with us.


Stay safe out there.
