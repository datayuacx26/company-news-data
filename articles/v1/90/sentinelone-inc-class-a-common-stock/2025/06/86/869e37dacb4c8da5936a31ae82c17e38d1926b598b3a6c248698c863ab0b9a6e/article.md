---
schema_version: "1.0.0"
document_id: "869e37dacb4c8da5936a31ae82c17e38d1926b598b3a6c248698c863ab0b9a6e"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-da824fadbad9"
canonical_url: "https://www.sentinelone.com/news/how-the-cybersecurity-industry-is-trying-to-keep-up-with-foreign-threat-actors/"
published_at: "2025-06-16T18:36:35+00:00"
first_seen_at: "2026-07-25T01:11:38.490639+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:b0daf74d9e73b595e9afcd23cbb3c96380fc9c765c327dfc51f23f928fb6348d"
---

# How the cybersecurity industry is trying to keep up with foreign threat actors

Support WBUR


(AP Photo/Jeff Chiu)


American companies are being targeted by foreign spies, ransomware groups and even fake job applicants from North Korea. One cybersecurity firm speaks out about the threats they're seeing and how they're combating them.


### Guests


**Tom Hegel,** distinguished threat researcher and research lead with SentinelOne, a prominent cybersecurity company.


### Also Featured


**Zaki Manian,** co-founder of the crypto finance startup Iqlusion.


**Brett Winterford** , vice president of Threat Intelligence at Okta, a large software company.


### Transcript


**Part I**


MEGHNA CHAKRABARTI: In 2021, Zaki Manian was needing help building his crypto finance startup. At the time, he'd had trouble finding competent coders with years of experience in blockchain development. But one freelancer stood above the pack and his resume showed an extensive work history.


So Manian hired him.


*ZAKI MANIAN: And then he brought on someone he said he had worked with, and collectively they were able to do the work. I was, I would frequently have to like, meet with them late at night because they said they were in Singapore, but they were quite technically competent.*


CHAKRABARTI: Because of the time zone differences and pandemic era restrictions in Asia, Manian never saw his new hires in person, but nothing seemed out of the ordinary.


*MANIAN: You send them chat messages late at night, tell them what deadlines need to be hit. They try to hit them, they get stuck. You try and help them, like, it's like having any sort of mid-level contractor that you hired and you weren't, like if you hired someone in India or you hired someone in China, or you hired someone in Vietnam, your interactions would be very similar.*


CHAKRABARTI: The freelancers eventually finished the work and got paid, and after that it wasn't something Manian gave a lot of thought to. Until two years later when an email from the FBI landed in his mailbox. Turns out those two workers were not from Singapore. They were from North Korea, and they were funneling most of their salaries into North Korea's weapons program.


*MANIAN: It was worrying because, like, you were worried that you broke the law. You didn't know what consequences there were there. What other collateral consequences there might be. You were worried about reputational consequences.*


CHAKRABARTI: The FBI investigated. The Treasury Department subpoenaed Manian's company.


Ultimately though the company was not penalized and the code that they created still works safely, but that didn't prevent many of his colleagues from viewing his work as tainted.


*MANIAN: Part of the dream of cryptocurrency was we're trying to build a global system that like anyone can contribute to. So you could be like, it shouldn't matter what school you went to, it shouldn't matter what country you live in.*


*That was the whole point of building this thing like that. And I think what we've seen with North Korea is enormously taking advantage of those values, enormously taking advantage of like the idea of an open internet.*


CHAKRABARTI: These North Korean IT worker scams are not just in crypto. The FBI says they've infiltrated hundreds of Fortune 500 companies. According to a report from the United Nations.


The fake employee program earns Kim Jong Un's regime between $250 and $600 million per year. Workers are often under duress by the North Korean regime. They're forced into cramped office conditions where they're constantly monitored, and they also work grueling hours and must fulfill strict income quotas.


Manian says now when a job applicant makes it to the interviewed stage with his company, he asks them to perform a very specific task.


*MANIAN: At this point, it's become so absurd, you know, that it's like just normal when you're interviewing anyone basically you've never met in real life, to basically be like, type in Kim Jong Il is fat, and like push enter. And like this will eliminate 100% of the North Koreans.*


*But so like for us, I'd say the screening process is just like now an annoyance and inconvenience. You feel bad for the people who are doing this work. A lot of them are good engineers. A lot of, and as far as we can tell, they're just basically slaves. But it's obviously just a huge problem for our industry.*


But I think like widely a huge problem for society.


CHAKRABARTI: In Manian's case, that screening he does eliminates almost half of the applicants. This North Korean IT worker scam is part of a recent uptick in cybersecurity threats from state-sponsored actors. Some researchers show a 47% spike in cyber-attacks just this year, we're only halfway through the year. High profile hacks have seemingly hit every part of American life.


(NEWS MONTAGE)


*WILL REEVE: A China sponsored hacker gained access to treasury workstations and documents earlier this month.*


*ASHLEY BUTTACAVOLI: What happened at Fall River Schools is in fact a ransomware attack. And the FBI is involved.*


*KARIN JOHNSON: Multiple sources have shared a pop-up message with me where the healthcare network was given 72 hours to contact the group behind the attack and negotiate.*


*STEPHEN COLBERT: And I don't mean to alarm you, but we are in the midst of a national emergency because Krispy Kreme online orders were disrupted in a cyber-attack. We can't let hackers take our donuts. They already have our cookies. (AUDIENCE LAUGHS)*


CHAKRABARTI: Funny, but also very serious because these threat actors have been so successful. They're even starting to infiltrate the people who have the most experience dealing with cybersecurity threats, cybersecurity companies themselves.


So this hour we're going to talk about the ever-changing world of cybercrime and how companies can and must protect themselves from new generations of foreign threats.


And Tom Hegel joins us. He's a distinguished threat researcher with the prominent cybersecurity company, SentinelOne and recently Sentinel Labs published an article titled[Top Tier Target: What It Takes to Defend a Cybersecurity Company from Today's Adversaries](https://www.sentinelone.com/labs/top-tier-target-what-it-takes-to-defend-a-cybersecurity-company-from-todays-adversaries/) . Tom, welcome to On Point.


TOM HEGEL: Hi. Thanks for having me.


CHAKRABARTI: Has SentinelOne itself been the target of cybersecurity attacks?


HEGEL: Yes, absolutely. We receive attacks in all types, from all types of adversaries. And the North Korean threats has been one of the most interesting that we've observed over the last few years. And that's ultimately seeking to get inside jobs, similar to the previous cases you've already mentioned.


CHAKRABARTI: Okay. So tell me more why do you think that the North Koreans wanna get inside a place like Sentinel?


HEGEL: Yeah, absolutely. So you have to remember that the North Korean cyber domain is quite complex and intertwined between multiple objectives. Now, the IT worker scheme where they're trying to get inside jobs, get salaries, the primary objective there is for funding the regimes' cyber and weapon program.


Now, when you start to involve additional targeting of trying to get inside jobs at very strategic organizations, be it banking, large cryptocurrency organizations or cybersecurity companies, the objectives start to blur. So these organizations within North Korea, they intertwine so much with various missions.


And when you start to look at the targeting of these more top tier targets, that's when we see the objective shift potentially towards long-term espionage, supply chain intrusions into our customer networks and things like that. So it complicates it and you have to remember that this is a very, like individualistic approach that they have to take, but ultimately it depends on the scenario that what can they do to benefit the most from each victim. And they'll try various things to try and make the most money or most impact for the regime.


CHAKRABARTI: And just to be clear, I guess to put it in layperson's terms, trying to infiltrate a company like SentinelOne would ostensibly also give those attackers access to the very software that you are creating to keep other companies secure.


HEGEL: Yeah, absolutely. So we were successful in stopping any of their attempts to get in. Very happy to say we stopped 100% of them to even get, trying to get in.


But the interesting piece is North Korean threat actors have a long history of doing that, very much what you just explained, which was what we call like a supply chain intrusion. Where they compromise or get access to the insides of one organization. And then use that access to get into a separate organization.


So there was a case just a few years ago where an entity very closely intertwined with the IT workers group out of North Korea performed multiple layers of supply chain intrusions, where they compromised one organization to get into a separate organization, to get into a third organization for the final goal of financial theft through cryptocurrency.


So very strategic, long play missions here.


CHAKRABARTI: Wow. Okay. What I'd love to do is spend a little bit of time talking through each of the different kinds of attacks that SentinelOne itself has rebuffed. Because as you said in the article that you published, it's very unusual for a cybersecurity company to even talk about the fact that they themselves have been targeted by international, especially state sponsored hackers.


Why did you decide to write up this article and just talk about the fact that it had also happened to you?


HEGEL: Yeah, that's a great question. And I would say we are in a unique position within my team, which is Sentinel Labs. We're focused on the ultimate community benefits of our research.


So we, day to day, we track adversaries of all types, and that includes defending our customers, defending our own organization. But instead of turning around and trying to monetize that, our goal is to turn around and try and impose some sort of cost on the actors themselves, to make it more difficult for them to do what they're doing and try and directly help impacted organizations.


So in cases like this, where we see them coming after us. It's like the goldmine for allowing us to collect the most threat, cyber threat intelligence that you can. As a target yourself, you get really interesting perspectives of their capabilities, their persistence in trying to get in and so forth.


The ultimate goal here is to share more widely. We do a lot of collaborating under the table with our partners across the industry, public and private sector. And when it comes to releasing things publicly like this, it's just like waving a flag, being like, everyone needs to pay attention to this.


This is a huge problem. And cybersecurity vendors have a history of being very quiet about the things they see, unless it's strategically benefit to them. So saying we are a target, raising that flag to be like everyone else is likely a target too.


CHAKRABARTI: And not only a target, but it seems like most other companies would have, what, a more difficult time in understanding when or the volume of attacks that they're experiencing versus SentinelOne, who, this is like, what you do.


HEGEL: Yeah, exactly. Yeah. This is what we do. So we tend to be a little bit more successful in stopping it and even being able to like identify it overall, we can have applicants, and we can collaborate with our partners across the industry and get really high confidence on the exact locations of these applicants and the attribution of the efforts. But if it's an everyday organization, small business, or even just like a an IT consulting company or crypto company the chances of them being able to link it to North Koreans or just the IT worker scheme in general is in many cases nearly impossible.


Yeah. It's like raising the flag, trying to show everyone this exists and give people the opportunities to try and catch this stuff.


Yeah. When we come back, we're gonna talk about exactly the kinds of attacks that you've noticed and how they work and how people and other companies can protect themselves from it.


**Part II**


CHAKRABARTI: So Tom, the job applicants from North Korea.


I have to say, it wasn't until recently that I heard that this was even a phenomenon. But in your article, you said that there were early reports that drew attention to these efforts. How the infrastructure that the North Koreans used to get the jobs and to launder the money that they're earning.


But you said neither gave a sense of the staggering volume of ongoing infiltration attempts. This particular vector far outpaces any other insider threat vector we monitor. So how many fake North Korean job applicants have you received?


HEGEL: Yeah, it's a great question. So SentinelOne in particular as a one target in this case, we have a history of about two year, two or three years of very clean attribution to North Korean applications in the numbers of about a thousand plus applicants.


And the unique personas around each of those applications is around 350. So you have about 350 fake individuals applying for thousands of jobs for our organization alone.


CHAKRABARTI: Okay, so fake individuals. So these are like entirely fabricated resumes, pictures, even, like how fake are we talking?


HEGEL: Yeah.


When you open up the session, you said create, they're very creative and that cannot be more true. So they will go through any means possible to have a very realistic persona, and that includes completely fabricating names, emails, resumes or just going to straight up stealing it from public individuals that are out there that may not be aware of it. So forged identity documents, forged resumes. We've seen cases where they're using AI tools to generate resumes specific to this application. So it's all forged to make them look as great of an applicant as possible for that role.


And it works quite well.


CHAKRABARTI: And I presume they're also masking the origin points of the information they're sending you.


HEGEL: Yeah, absolutely. A lot of it they tend to reuse, which is, and to be honest with you, it's an interesting way of how we do a lot of the linking to a lot of these different personas. Back to North Korea, they reuse a lot of attributes about it, and it's an automated toolkit to ultimately get these names in the resumes, get fake phone numbers, and they're using tools and technologies that kind of muddy the North Korean link, but also make it more difficult for us to see where it came from.


Sometimes they will just go and copy a name of a well-known individual though.


CHAKRABARTI: Okay. And then they're also oftentimes using each other. These fake personas are using each other as references.


HEGEL: Yeah, absolutely. It's not uncommon for one persona or one individual to get into an organization and for them to attempt to hire more colleagues out of North Korea.


And we've also seen cases where they try and contact employees of organizations that are openly trying to recruit and get referral bonuses for companies and ask them to refer them, so they bypass that application process and get an insider to help refer them internally just for a potential bonus.


CHAKRABARTI: Okay. I'm going to ask you a little bit more about the other things they do, because you talked about how the adversaries adapt to the friction they encounter, but let's hold onto that thought for a second. It occurred to me that I keep calling them fake. So the personas are clearly fake, right?


You just described how, but when they're, not in SentinelOne's case, but I'm thinking back to the blockchain example we had at the top of the show, when they're actually, in his case, they were actually hired, and they did the work. So in that case, the work wasn't fake, it's just the person who was doing it was fake.


I'm just trying to understand like what to call these attempts.


HEGEL: Yeah. It you're not alone in this kind of being very confusing. It's important to remember that when one of these individuals gets hired, in the application process and the interview process. This is often done by multiple individuals that work as a team, that are specialized in that task.


So after the fact, when they're hired, it's usually managed by multiple people that are technically capable and trained from a young age to be able to do that task. Typical software engineering, IT work. Et cetera. So you have a legitimate technically savvy individual working for you. Now, how they're abusing that access is a whole different story.


But yeah, you're looking at multiple individuals just to get into a single organization.


CHAKRABARTI: Okay. And then also you write about how it's not just the personas of the individual applicants, but there's a sort of this sprawling network of front companies, of fake front companies. What is that?


HEGEL: Absolutely. And this is where it gets really messy, because when you have to pay these individuals or money comes into the equation here, how does the money get from a U.S. company back to the North Korean regime? So they go through multiple money laundering attempts, and this is a very dynamically evolving process for the regime.


So these front companies, we wrote about a lot of 'em being based in China. The FBI has seized these front companies, in some cases, their websites, their infrastructure, and they just pop up like crazy. What you're seeing here is an individual that claims to be an American or is working for an American company.


They're getting paid into a bank account and that bank account transfers it to like a bank account in China or a cryptocurrency firm, and then it goes through multiple hops. And these front companies are used to both launder the money, to eventually get it back to the regime, but in some cases have another persona as an organization, like a consulting organization, rather than an individual.


So back to that main point I mentioned earlier, is this is all so dynamic and so individually operated that you see a lot of experimentation, a lot of creativity in how these things operate.


Is that what you're talking about in the article when you say the way they adapt to the friction they encounter?


HEGEL: Absolutely. It's just like when we raised the flag with this article saying, here's how we see them. We see an immediate shift. We know they watch what we're talking about, and they use our tools, they test our technology, and they're using that to quickly adapt. Because it's an endless cat and mouse game with these guys.


As with any cyber actor, whatever we do, they are going to react and try and make it more difficult to catch new techniques, new methods and so forth.


CHAKRABARTI: How did you determine that there was a thousand of fake applicants coming from these 300-ish personas?


HEGEL: Yeah, I'm really lucky to have a great team of individuals that their careers are based on how can we track these personas through different means, different technology sources like emails, open-source intelligence around location data, network traffic.


So it's a long history of tracking one persona that we're really confident on and seeing how they shift and what they link to over the years. And this is all not possible without the partners in the public and private sectors that work tirelessly to disrupt these guys under the table outside of public view.


So there's a lot of collaboration, but there's a lot of really skilled individuals, that this is their careers to focus on tracking these guys.


CHAKRABARTI: Okay. I want to put a pin in that because for companies that are not SentinelOne, okay, you could buy a cybersecurity firm's products for sure, but I'm thinking that there's a great associated cost with being the victim of these kinds of increasingly sophisticated attacks. But Tom, I just wanna take a moment to talk about how there are also links to this activity on U.S. soil.


Because scammers will sometimes recruit Americans to build laptop farms, to assist these overseas IT workers that are posing as U.S. citizens. Elizabeth Pelker is a special agent on the FBI's Cyber Crimes Task Force, and she talked about this during a panel discussion at an IT security conference on April 28th.


*ELIZABETH PELKER: Generally, these individuals have been recruited online to just host these laptops. Thinking that the overseas actors are based in China and they're just doing these guys a favor.*


*It starts with maybe one or two laptops, and then we'll see upwards of 90 laptops at one person's residence.*


CHAKRABARTI: Tom, can you talk about this? So they're also recruiting Americans to go to Micro Center and buy a bunch of laptops and just plug them in for Chinese use.


HEGEL: Yeah. It's a surprisingly successful way for them to appear as U.S.-based.


And this is one of those things that makes it even more difficult to defend against these as a corporation in the United States. How do you detect these if they're already inside your network? So what we're seeing here is these North Korean operators, when they get hired, they have to use the corporations, the victim's equipment.


In many cases, they're getting like a corporate laptop. And that corporate laptop has typical security controls on it that need to be used to access that company's network. You get hired for a job, you're using that company's equipment. So when they get hired, they have a proxy entity that they pay within the U.S.


We see it in Europe as well. It's not just the U.S., and they pay those individuals to just basically plug the laptop in. Let us use your internet connection and everything we do from North Korea, it's going to hop through your home network to that laptop and that laptop looks like a U.S. based individual, to that corporation.


So they go through ways of trying to recruit individuals to this. Some know they're working for North Korea, that have been caught. Some might not know, they might just be technically illiterate, that just don't see the risks and sketchiness of the whole thing. But it's a very successful way for them to, from a technical perspective, appear that they are based in the location of the employee rather than China or North Korea.


CHAKRABARTI: Okay. In the news, you can use category folks. If someone seems to send you an innocuous request to buy a bunch of laptops and plug them into your home internet, you should probably alert authorities about that.


Okay.


HEGEL: Definitely.


CHAKRABARTI: So then, so we talked, the fake personas was just so fascinating to me, Tom, I'm sorry I spent a lot of time on that. But the other one, which I think most people have heard of is ransomware. And you talked about that in your article too, and it just seems like with year after year, we see this exponential growth in ransomware attacks and successful ones.


And so SentinelOne has even been, has detected attempts at ransomware coming up against your own company.


HEGEL: So the interesting thing about the ransomware part is rather than ransomware attempting to be used against our organization, that is more so attempting to test our ability to defend against that organization, to help defend our customers.


We have, our product is deployed on machines worldwide and these ransomware operators are encountering our product and want to know how to bypass it so ransomware can run. So what we ultimately see is them trying to get access to our product for testing, detection, evasion, and basically trying to white glove a scenario, looking like a legitimate business to try and get access to a product.


They'll attempt to purchase it; they'll attempt to gain access to it illicitly through stolen product from other victims. But yeah, the ultimate goal in this case is to look at our product and see how they can avoid, how they can successfully infect people that are using it to defend themselves.


It's another cat and mouse game, but yeah they're trying to avoid getting caught.


CHAKRABARTI: Okay. And you mentioned something called Black Basta. Can you talk about that?


HEGEL: Yeah, it's ultimately another one of the many ransomware clusters of activity. Ransomware is interesting because you think of like a group of hackers using ransomware.


That's how it started, is like there's this group of close-knit individuals that work together to create this malicious software that infects people and holds their networks ransom. And then that same group demands a ransom and makes them money. Nowadays what we see are what we call ransomware as a service, where we have one group of individuals create a product that is used to deploy ransomware or manage ransomware infections, and they sell it to whoever's gonna pay.


I, as an individual, can go and buy a ransomware tool that works very well and deploy it to whatever organization I want, and then that ransomware operator makes a little bit of a cut out of it. But I get the money and I operated the infection, I operated the payment coordination and so forth.


So it's a whole complex economy of trying to make money and it's working incredibly well. But again, just like the North Korean stuff, this is another adversary that is evolving based off of new ways for us to catch them and track them. It's an endless cat mouse game.


CHAKRABARTI: Wow. Okay. So we've been focusing on North Korea a lot.


But is it fair to say that behind this there's also state sponsored activities coming from places like China?


HEGEL: Oh, 100%. Yeah. We, many, on my small team, we have a long history of tracking specific Chinese state sponsored entities that are using ransomware as a distraction to very strategic espionage and collection against government, private sector, across the world. So we see ransomware used for financial gain by individuals, but it's also an amazing way to cloak your true intentions and attribution to very strategic, politically motivated intrusions, across governments, critical infrastructure, and anything you can imagine.


CHAKRABARTI: Tell me more about that. I'm not quite sure. So it's like the ransomware isn't actually the point of some sort of deeper kind of espionage.


HEGEL: Yeah, absolutely. Amazing examples existed in the Ukraine conflict when that conflict kicked off. It's a really good example to show multiple intentions of malware.


So we saw cases where malware that was deployed in attempt to hold networks ransom, appear, trying to appear as financially motivated operators, but the goal was to just disrupt operations of that victim and never actually try and facilitate ransom and restoration of that company. So modern day today, like more recently, that's exactly what's going on. As we see this Chinese espionage group that their objective is to either steal intellectual property or gain access to telecom communications.


They'll go into this network, and they will deploy this ransomware that they purchased from somebody like Black Basta or anybody that is well known and they'll deploy it widely and that organization is in complete chaos there.


There's machines for all 30,000 employees are all shutting down, operations are going offline, and at the same time, this very technically skilled organization out of China is going through and pilfering out this very valued data that they've been seeking for years.


**Part III**


CHAKRABARTI: In a world of automation, is it pretty safe to say that in some sectors, let's say the tech sector, that these fake job applicants are coming into? What? I don't know, like almost every job that gets posted.


HEGEL: Yeah, great question. We see them basically apply in an automated fashion at mass scale.


So if you are a company that has jobs posted online that you can apply remotely from, and those jobs are technically, are technical in nature, and it's a remote job, they will often apply. So we see them apply almost to everything out there. It's a completely automated process that is then fine-tuned after they make that first application review, make it past that first review process.


But yeah they're pretty much applying to everything out there.


CHAKRABARTI: And not just in the United States, right?


HEGEL: That's correct. The start of this right around the pandemic was predominantly the United States and I think we have caught onto it quite a lot and have made some significant strides in disrupting bad efforts in the United States.


And since then, they have since moved to throughout Europe and they've moved out of just IT software engineering and now they're doing a bit more like consulting in terms of like graphic design, CAD engineering, anything on like technical level that they can do remotely with success.


CHAKRABARTI: Wow. Okay. So Tom, hang on here for just a second because let's listen to a bit from Brett Winterford. He's vice president of another big player in the cybersecurity industry. It's called Okta Threat Intelligence, and he's been following foreign hacking campaigns for years, especially from North Korea.


*BRETT WINTERFORD: We're learning about this as we go, and the sophistication of their scams is pretty surprising. I think it's a bit dangerous for anyone to assume that they haven't been touched by this threat at all, particularly in the technology sector.*


CHAKRABARTI: And Winterford says the newest threat is AI, which can craft convincing resumes, cover letters, headshots.


*WINTERFORD: So let's say they want to go for a job as a full stack developer for a particular company. What they'll do is they'll actually advertise. Precisely the same role, but it's a fake role in order to take in CVs and cover letters from legitimate job applicants and use that as a training set to then identify what are the kind of features or traits in a job application that are likely to be successful, at least getting past the first stage of most recruitment, which is automated applicant tracking systems.*


CHAKRABARTI: I don't know. You kinda have to give them credit for being that creative, creating a job posting to get data on what kind of applicants would want that job. Now, Winterford says it's important for hiring teams to be aware of red flags, when, of course, screening their applicants, such as if they refuse to be on camera or won't show an ID card, or if they have answers that seem a little too scripted.


*WINTERFORD: If the candidates face for even a second flickers in such a way that it looks like it could be digital altered and they refuse to hold their hand up or an object up in front of their face, that is a very good sign, because the deepfake technology at this stage, it's very noticeable. If you ask the candidate to put their hand in front of their face, you'll be able to tell that there is a deepfake overlay.*


CHAKRABARTI: So that's Brett Winterford, vice President at Okta Threat Intelligence. Okay, Tom, so what else should companies do when, on the job applicant front to screen out potential bad players?


HEGEL: Yeah. It's an ever-evolving effort for sure. As AI technology, as Brett just explained, quickly evolves, ways of catching them are rotating on a, like, a weekly basis.


So some of the more interesting ways of filtering these individuals out, that we've seen, are just operating with a sense of skepticism of why they're applying, communicate with them exactly why they're applying. If they can't be specific in terms of what they applied for. That's another interesting thing, but from like the visual perspective, you obviously have the face mimicking, like Brett explained, using AI tools and so forth.


The interesting thing is these are all remote jobs. So if there's ever an opportunity to ask them about the location they're at, for specifics of what they recommend in that area and so forth, they will tend to not have an understanding of the source of their application process, they're saying they're from Boston.


They very much will likely not have any concept of local details of Boston, but these are all games, in my opinion. Like these are all very temporary, interesting things to go about the way of catching them. Reminding that these are all remote jobs. The number one way that we've caught them, if they've ever attempted to get even close to a job, is asking to meet in person.


If they have the ability to fly them into the location or visit somebody that's at that location in your employments already, having somebody vet them physically and that they actually exist is the easiest way to get past a lot of these games.


For sure. But yeah, you have to be, you have to be careful because everything they're doing evolves.


Constantly. So they are going to now this hand waving thing in front of your camera. They're gonna come up with ways to avoid that or being able to say anything malicious or offensive to the regime. Those are short-lived techniques of catching them. So you just have to be thorough.


References, lots of vetting, lots of confidence building, clear communication, and don't just hire without being able to even turn on a webcam.


CHAKRABARTI: Okay. So it actually seems very plausible to me, even if expensive, that when you get to a sort of final group of applicants for a job that a company should consider like you said, flying them in IRL, we can just put it that way. And see if this person exists. But then again, I can already imagine that maybe they're not going to send someone from North Korea. But as you talked about, the international nature of these scams, they could just fly someone else in to be a fake person.


HEGEL: Yeah, exactly. There's gonna be some scheme, if you're that strategic of a target for them, they're going to put in significant effort to get past a lot of that.


I think another important piece is to not forget that these individuals that are applying tend to reuse or try and have the same face for the interview, for the in-person meeting, for the jobs.


See what they look like at each stage of the interview, if they're the same person, the voice changes and so forth. But yeah.


CHAKRABARTI: Okay. So let me ask this though, Tom, given not just the political environment we're in, but I'm thinking of the companies that firms like yours serve, in terms of providing cybersecurity products.


This seems like a national security issue as well, right?


HEGEL: Absolutely.


CHAKRABARTI: And so why not take the next step. It might seem extreme, but why not say job applicants have to be based in the United States, even if they're working remotely.


HEGEL: Yeah, it's a very tough problem to solve, and it's very difficult to tell a small business that they have to hire a U.S. based individual to do their engineering.


Just like at the top of the hour when you mentioned that there, or when there's an individual that needed kind of some cheap labor to be able to develop a product. It's very expensive and very difficult for these small businesses in the United States to be able to justify that out of this potential threat that it could be an adversary.


They're a small business. They're trying to just save the pennies rather than worrying about national security. So it's tough, and I think it's just a lot of it tends to be knowing that this threat exists and knowing the risks of not properly vetting your employees is critical. If this individual at the top of the hour was held accountable for funding the regime, that's a very big problem for that individual.


Luckily that didn't happen in their case, but at what point does the blame shift from the adversary to negligence on U.S. companies to do proper vetting of these people that they're hiring and paying U.S. currency.


CHAKRABARTI: I guess that's a question for lawmakers and lawyers.


HEGEL: Yeah. Yep, exactly.


CHAKRABARTI: But you're raising a really important point here. I completely hear you about small businesses. So they can do as much as they can to vet their employees, but at a certain point there will always be cracks in the system. It sounds like this is yet another kind of arm's race going on, that like SentinelOne will continue to get better and better, but so will the hackers.


HEGEL: Absolutely. Yeah. And this is the goal of a lot of our public and private collaboration on these fronts. Are we might have the intelligence of the many applicants that are out there that use these emails that are reused across many organizations or these same personas, so these small businesses that are using these job boards and so forth.


We might not be able to go to every small business and help them defend themselves, but we can go back to the technology that small business uses to defend them at that level. So rather than stopping the job application, let's go to the email that fake applicant uses and get them shut down at the email level and disrupt them there.


And then ultimately, it's provide our intelligence to the people that can go and knock on doors and try and make a difference at that level, like the government.


CHAKRABARTI: Okay, so this is a perfect segue to another thing that Brett Winterford at Okta Threat Intelligence told us. Because we asked him what can government agencies like the FBI do to help private companies combat these foreign cybersecurity threats?


WINTERFORD: I think the U.S. government has done a great job historically on disrupting cyber-crime ecosystems, ransomware groups, et cetera, with some of their offensive security capabilities. And I guess my message is that we need those individuals in the U.S. government that are doing that super important work to feel supported and to be resourced appropriately given the magnitude of the threat.


CHAKRABARTI: So Tom, specifically, what would you like to see being further resourced or made more vigorous in terms of government investigation or regulation?


HEGEL: Yeah, it's a great question and a lot of it comes down to greater coordination in resources to this specific threat in mind. Over the years past when cyber threat intelligence really kicked off, the predominant topic was Chinese intellectual property theft and nowadays it needs to shift more towards insider threats of North Korean individuals just because of the pure scale of this activity. So greater collaboration between public and private sector. We are SentinelOne. We will openly share or privately share with our business competitors and governments that are friendly to us and so forth.


So if there's a way to collaborate to make impact. We need to do more of that, and that includes collaborating with and forcing large technology companies that don't work in cybersecurity like email, telecom, to also help make an impact or be required to make an impact when we identify malicious activity domestic within our own countries.


CHAKRABARTI: Tell me more about that. What do you mean?


HEGEL: That would be things like if they have social media profiles, LinkedIn profiles, Twitter accounts, these companies should be responsible for properly disrupting those accounts to stop those personas from further abusing it. Think about things like email.


A lot of these applicants will use specific email providers, and they'll just go and make thousands of these fake emails and then use those to apply for these jobs. If we're tracking these emails, let's go to that email provider. And they should be forced to shut those down when there's evidence that they're being maliciously used.


And that collaborative effort needs to be very fluid, needs to be very quick. And that's ultimately how we defend these small businesses that are getting attacked and have no idea that's even happening, is we stop them at the application process before they even have the opportunity to target these small businesses.


CHAKRABARTI: Okay. So then what you just described, that process of the tracking, for example and then bringing that evidence to a large email provider, is that not already part of the culture of cybersecurity and companies and their clients writ large?


HEGEL: It is in a way, but it's not formal enough to make a very recurring level of success. A lot of this is based off of heroes in these individual tech companies that are just going outside their box to be able to make an impact. There's great teams that, like you look at Google's Mandiant team, they do amazing work at tracking this and collaborating at this level.


But then there's other organizations that these operators are using that also have met really well known email services and those are organizations that aren't really inclined, because they're not required legally to collaborate or disrupt or share intelligence back. So it depends on the organization, but a lot of this is based off of just really lucky teams within these larger corporations that are just going outside their capabilities to be able to make this happen for us.


CHAKRABARTI: Lucky meaning what? Like they're lucky that they get to do the work, or they're lucky in terms of what they find.


HEGEL: They're lucky. They're lucky in ways that they're not constrained by legal requirements, by their own legal staff. In addition to having access to the right data, the bigger the corporation the more difficult it is for these researchers to be able to see the data because of privacy restrictions and so forth.


Or they're not allowed to share with others in the industry that they know this email address, they're abusing their service, is tied to this actor. So there's a lot of hesitation restrictions internally in the corporation. And then culturally, you can't, you also have to mention that this is happening in the U.S. and across Europe.


There needs to be cultural barriers broken down to make this happen.


*The first draft of this transcript was created by Descript, an AI transcription tool. An On Point producer then thoroughly reviewed, corrected, and reformatted the transcript before publication. The use of this AI tool creates the capacity to provide these transcripts.*


*This program aired on June 12, 2025.*


[Latest episode](https://www.wbur.org/radio/programs/onpoint/tag/latest-episode-on-point)
