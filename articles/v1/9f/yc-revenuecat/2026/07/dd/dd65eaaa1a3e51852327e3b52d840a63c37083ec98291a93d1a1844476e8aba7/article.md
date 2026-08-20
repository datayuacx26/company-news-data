---
schema_version: "1.0.0"
document_id: "dd65eaaa1a3e51852327e3b52d840a63c37083ec98291a93d1a1844476e8aba7"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/growth/andy-allen-not-boring-software-launched-podcast-2026"
published_at: "2026-07-01T12:48:15+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:097ebad026f34b2a2830189ad5b7b4a48c3a0ee2394e4f0dbc9dc9074f927161"
---

# A patron plan with no extra features outsold expectations — and proved design alone can be a business

## The artist who burned his paintings at 40


Not Boring Software didn’t start with a market opportunity or a gap analysis. It started with the conceptual artist John Baldessari.


Baldessari spent the first half of his life as a self-described “mediocre landscape painter.” Just before turning 40, he took his entire body of work — decades of paintings — and burned it all. Then he wrote “I will not make any more boring art” thousands of times on camera, in a kind of performance art ritual. He went on to become one of the most influential artists of his generation.


Andy Allen found Baldessari at almost exactly the same point in his own career. He was approaching 40, had spent the prior few years living in Amplitude dashboards at WeTransfer (which had acquired his previous company, FiftyThree), and felt himself slipping into the kind of B2B product work that was fashionable but soul-deadening.


“I felt like sometimes there’s just like good very simple heuristics,” Allen says. “If you can just say, ‘I will not make any more boring software,’ that might lead to some good outcomes.”


The timing was deliberately uncool. In 2020, nobody was starting an app business. Everyone was chasing SaaS. “It’s so funny because I just found that oftentimes the things that I’ve done that have had the most staying power or resonated the most have often been these very uncool things,” Allen says. “Whenever you try to do something cool or of the moment, it just doesn’t really go anywhere or it flubs.”


## The patron plan that shouldn’t have worked


Not Boring launched with three apps — Weather, Calculator, and Timer — available individually or as a collection via subscription. For Weather, the subscription pitch was straightforward: premium data sources. For Calculator and Timer, what you were unlocking was… skins. Different visual themes. Alternative app icons.


Conventional wisdom says this shouldn’t work. People don’t pay for aesthetics in utility software. But Allen and his co-founder added something else: a “patron plan” priced at five times the standard tier. It included essentially nothing extra — a physical gift, and the knowledge that you were funding the studio’s work.


“It was shocking how many people went for that plan,” Allen says. “And that was kind of the moment that we realized, oh yeah, this is a little bit of a different business model. We’re not selling features here. People want to see this work and see us maybe as the knights fighting this battle against boring software and they want to fund this effort.”


The model has compounded over five years. Early subscribers got their price locked in permanently. Every new app (one per year) is automatically included. The incentive structure pushes Allen toward making new, interesting things rather than grinding on feature parity with competitors. “We’re not trying to chase features that are going to unlock some new audience for us or that we think are going to suddenly bump up our conversion rates,” he says.


## Why every Not Boring app ships as a “complete” product


Allen’s previous company, FiftyThree, made Paper — the drawing app that became one of the first native-feeling iPad apps and eventually sold to WeTransfer. That experience taught him what happens when you raise VC money for a product that’s a great small business but not a billion-dollar outcome: you get trapped.


“The biggest fear was getting trapped in a business that you didn’t want to run,” Allen says. “And I see that happen with so many founders.”


The structural solution at Not Boring is radical simplicity. Each app ships as a complete product. No V3. No feature bloat. No chatbot in the corner. “I prefer to say like, ‘This is the calculator that we made.’ It’s complete and it does its job really well. We’re not going to change it on you.”


This creates a forcing function: if you can’t go back and iterate on old apps, you have to make new ones. And making new things — re-envisioning old software categories through a design-first lens — is the work Allen actually wants to do. The constraint is the point.


## A dozen sound files for one button


The Not Boring apps feel like video games. Everything runs in Apple’s 3D engine (SceneKit), with low-poly models inspired more by the PS1 era than photorealism. But the detail that most people notice first is the sound.


Allen plays sounds even when the iPhone’s mute switch is on — something that would get most apps destroyed in reviews. It works here because the sound is the experience. But the real trick is subtler: every button tap doesn’t play one sound file. It plays one of a dozen slightly different versions of the same click.


“No button ever sounds exactly the same,” Allen explains. “Why does it sound so great to listen to someone typing on an old clickety-clackety keyboard? It’s because of all the variation. When you hit a physical button, it hits differently every time.”


This is a technique borrowed directly from game audio, where footsteps and punches need to repeat without becoming grating. Allen published the technique — along with free sound kits — hoping other developers would adopt it. “It doesn’t take much, honestly. It just takes working with a sound designer,” he says. “It’s a great way to amplify your experience that really doesn’t add much overhead or complexity to the app itself.”


## 3 years to build a camera that launches faster than the competition


Not Boring Camera took three years of false starts before shipping. The team explored 3D photography, spatial photos, and various experimental directions before arriving at a simpler insight: don’t reinvent what a camera does — reinvent what it feels like to use one.


Every control in the camera app is modeled in 3D. Dials turn. Sliders have physical weight. Buttons tilt based on where you press them. The preview isn’t full-screen — it’s inset, like looking through a viewfinder. And despite running in a game engine, the app launches faster than every third-party camera app Allen has tested.


But the real philosophical bet is on the image pipeline. Not Boring Camera strips out all of Apple’s computational photography — the multi-frame composites, noise reduction, and HDR tone mapping — and works directly with raw sensor data. Allen calls it “Super Raw” (a nod to Apple’s “ProRAW,” which he argues is still heavily processed).


“The default camera apps — it’s not just Apple, it’s Google, it’s everyone — their goal is to make sure that you never take a bad photo,” Allen says. “But I think the result is that you never can really take a great one.”


The app applies film-inspired LUTs in real time, lets you edit raw exposure and temperature before shooting, and embraces grain and blur as creative choices rather than flaws. “Some of my favorite photos from our app are accidents,” Allen says. “It’s like I accidentally hit the shutter button as I was putting it away so it’s like this really cool blur. I miss some of that and I think we’re trying to bring some of that back.”


In[the full episode](https://www.youtube.com/watch?v=TCrwOUDifVM) , Andy also talks about designing the never-shipped Microsoft Courier tablet, why the early Macintosh shaped his view of computers as creative tools, and his recommendations for checking out the artist John Baldessari and musician Brian Eno as models for creative thinking.


## Guest links


[Andy Allen on X/Twitter](https://x.com/asallen)


[Not Boring Software](https://notbor.ing/)


[Not Boring Camera on the App Store](https://apps.apple.com/app/not-boring-camera/id6504163047)
