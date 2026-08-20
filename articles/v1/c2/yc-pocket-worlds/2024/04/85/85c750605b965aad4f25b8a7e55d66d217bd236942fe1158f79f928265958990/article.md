---
schema_version: "1.0.0"
document_id: "85c750605b965aad4f25b8a7e55d66d217bd236942fe1158f79f928265958990"
company_key: "yc-pocket-worlds"
company: "Pocket Worlds"
source_id: "yc-pocket-worlds-rss-93d814af52ac"
canonical_url: "https://medium.com/pocketworlds/tiny-giants-how-small-teams-can-change-the-world-26cb16678e20"
published_at: "2024-04-04T20:27:29+00:00"
first_seen_at: "2026-07-25T19:20:19.968753+00:00"
fetched_at: "2026-07-28T22:26:02.237076+00:00"
content_hash: "sha256:3cd656d95d920e7122ce6dba2dff380c2c4f80de7f5981470690340c873a2980"
---

# Tiny giants: how small teams can change the world

Unity Game Development


Mobile App Development


Creators


Gaming


Virtual Reality


# Tiny giants: how small teams can change the world


[Mariana Aguirre](https://medium.com/@pocket_manjee?source=post_page---byline--26cb16678e20---------------------------------------)


12 min read


·


Apr 4, 2024


--


## Jimmy Xu, cofounder and CTO of Pocket Worlds, shares how a team of 16 engineers revolutionized creator empowerment through Unity.


Sitting at the intersection of gaming and social, Highrise has lived in a liminal zone of creative play for 8 years. Offering a global community of over 300,000 Daily Active Users (DAU) tons of virtual collectibles and social experiences, Highrise is beloved for its quirky avatar styles, Instagram-like newsfeed, and player-hosted games. And it’s on the cusp of its biggest transformation yet.


A new game engine built in Unity just finished rolling out, promising creators new ways to connect through Worldbuilding tools. It’s a massive update that required ***entirely rewriting the app.***


What should have taken months with hundreds of engineers happened within weeks… by a team of just 16.


Cofounder[Jimmy Xu](https://www.linkedin.com/in/jimmyfxu/) **** shares what made this possible: and how our small, agile team is only just getting started.


Press enter or click to view image in full size


## 💭 MAKING THE UNITY TEAM


*Over the last year, the engineering team significantly restructured and consolidated to focus on Unity development. How did you approach these massive organizational changes?*


The engineering team was originally composed of an **iOS team** of iOS engineers, and an **Android team** of Android engineers. They were very specialized in their specific craft and their specific platforms; so when we made the change to Unity, we educated everyone and brought everyone up on the Unity platform. It was a very big switch for essentially *mobile* developers into a completely new technology. But, we successfully navigated that over the last year, and now everyone that ***was*** a mobile developer is now ***also*** a Unity developer and operating at a very senior level.


Press enter or click to view image in full size


Moving to Unity enables creators to build 3D rooms


*The team must have had to learn a LOT to make that happen!*


Yeah, it was definitely challenging. We didn’t send anyone off to a coding bootcamp or anything like that. Instead, it was a lot of **learning as we built** because we were rebuilding 4.0 from scratch, with every single line rewritten.


We have a great engineering team, so I think learning the technology *wasn’t* the toughest thing for them: everyone picked it up very quickly and within two, three months they were up and running at speed.


For a little bit of context: Highrise before 4.0 was built half as an app, which is why we had the iOS and Android developers as separate teams. And then the other half was a game. So it’s like two separate platforms with one game engine that’s being shared.


With Unity, the whole thing is now unified, and the big unlock for us is we now we only need to write once to deploy to all platforms, and we have a lot closer access to the game engine.


The big opportunity for us is now we’re really focusing on growing the game developer muscle for us internally at the team. So we’re[actively recruiting a senior game engineer,](https://www.pocketworlds.com/careers) and we’re getting much deeper on the game engine side of things.


*And why go with Unity in the first place?*


Unity’s interesting for us: it’s always been on our radar. Even from the beginning, we looked at Unity asking, is this the right move for us? And for the longest time we decided the answer was no. Really, the answer was no for ***any*** game engine — not just Unity — because the idea of Highrise is that we wanted to build something that’s ***very accessible, especially to the mobile generation.*** These are people that grew up with phones and iPads, who are not necessarily gamers.


I mentioned before that Highrise is 50% app and 50% game, and that’s how we expect users to see it. Up until about two years ago, no game engine had UI capabilities powerful enough to actually build an ***app-like experience*** . We basically built this custom thing that was very convoluted, but the end result was great for the user because it recreated the Instagram app experience in terms of UX, or the Snapchat speed of interaction. You didn’t see this giant loading screen for 30 seconds before you can even jump in.


And then two years ago, Unity released this thing called[UI Toolkit](https://unity.com/features/ui-toolkit) , which is essentially their move to being better at supporting UI. And that was the signal for us: *Hey, I think it’s ready.* It’s time for us to jump into a game engine, rebuild all of our app features inside of this game engine and create an experience that’s 99 or 100 percent the same as building it natively in iOS or in Android.


A handmade discotheque with a giant stage, just waiting for a DJ


*What would you say is the team’s most impressive technical achievement from the last year?*


Two things!


One is we have taken Unity, especially the UI side of it, beyond any other company out there. We know this for a fact because we talked to the Unity team that works on the UI Toolkit, and they’ve been consistently blown away by what we’ve been able to do with their tool. We’re gonna work on a case study with Unity to show off our tech to the broader world, to show people out there that you can build like an app-like experience with Unity; so that’s certainly a huge technical achievement. I’m very proud of it.


Second one is **Studio** . This one’s more in terms of just the raw speed at which we move.


The way that studio came together initially is that we built a standalone desktop application. We built basically everything about it over the span of three months. Two of those months was just spent fighting edge cases and bugs: because it turns out building a tool like Unity is very difficult!


And then we realized: there’s a better way. We can actually leverage Unity Editor itself and ***build our system as a plugin into the Unity editor*** ; and in the process, unlock all the power and functionality of Unity itself without having to reinvent the wheel.


From the day we made that decision to the day that it was ready to ship? **Two and a half weeks.**


We basically built a fully functional version of Studio comparable in power to a Roblox studio, or to Unity editor itself in a span of two and a half weeks with just two engineers. I think that’s another testament to our speed and power as a team.


Press enter or click to view image in full size


Editing interactive rooms through Studio unlocks the creation of custom experiences


## 💡🤏 THE ENGINEERING TEAM: TINY BUT MIGHTY


*What behaviors have you observed developing naturally among your team? What are* ***emergent values*** *exhibited by your team?*


I think the obvious one is bias towards action. Because our team is so small, we actually **don’t have any management structure** on the engineering team. There are no engineering managers; there are no product managers; no program managers: there’s ***just no managers*** . The only person who has a semi management role is me: so everyone on paper reports me, but I’m not a manager– nor am I trying to be one.


> [A company value which is certainly true for engineering is, “let builders build.”](https://www.pocketworlds.com/company)


We try to set the minimum amount of process and just try to **get out of the way.**


What we do is we hire excellent people who are really good at what they do; who are experts in their field. They come in, look at our mission, our vision, and roadmap at a very high level, and understand what they need to do. They understand their individual responsibilities, their ownership, and take the projects as far as they can take them.


That’s definitely very unique to us. For me personally, I have an extreme predilection towards velocity and speed.


But that’s the general theme in broader company as well: when in doubt, bias toward action. Just do it. Most, if not all, decisions that we make are two-way doors. If we make a mistake, it’s okay. Walk it back, try something else. Much better to ship rather than to wait and get a committee to sign off, and *then* do something.


Just ship it. If it doesn’t work, we’ll ship something else.


Press enter or click to view image in full size


*What’s something that one might be surprised to learn about the team or Highrise?*


The team is very small for the number of users that we have, the scope of the world that we support, and the success of the platform in general. Our team is ***tiny*** .


Our engineering team is about 16 people, split between all the different engineering disciplines.


Server team has four people running the entire backend: all the DevOps, all the infrastructure, all the feature development.


The data team is one person.


Web team is two.


Unity team, which is building everything that’s user facing, ( *both the game client and the studio)* is something like six people.


So… we’re a very small team building a huge, huge product, serving a ton of users (300k DAU). **This just means that every single person is extremely impactful and we all individually get a lot of things done.**


*To give some context, what do engineering teams look like at other companies at similar scales?*


For a similar product at our scale, you’re looking at an engineering team that’s probably in the hundreds.


And then at a scale beyond — the next milestone we’re trying to hit — is Roblox. And at Roblox we’re talking thousands.


It’s a statistic that I’m very proud of. As a leader of the engineering team, I think we built a great culture, a great team, and, we get it done.


Press enter or click to view image in full size


The engineering team hangs out at annual offsite


## 🕹️ DAVID AND GOLIATH: POCKET WORLDS IN THE BROADER GAME INDUSTRY


*Tons of big tech & gaming companies having been going through major rounds of layoffs recently. What are some challenges facing this industry, and what is Pocket Worlds doing to face these challenges?*


That’s never really affected us. I think a lot of this is a reaction to the explosive growth for a lot of these companies in terms of headcount over the last two or three years. During Covid, everyone just really juiced up hiring. What’s happening now is pulling back.


We never went through that because we’ve always had the objective of keeping the team as small, elite and nimble as possible. We never grew the headcount extremely quickly, so we never had layoffs.


And we don’t plan to! Because with a team of say, four people on the back end, we’re not looking trim off headcount at all. But I think the opposite of that is also true: we are very diligent about who we bring onto the team.


*Is Pocket Worlds planning to scale up dramatically at any point? Or will the principle of keeping the team small, elite, and nimble, always be the forefront of the hiring philosophy?*


Definitely keeping the team small.


> I personally believe that one great engineer with the right set of tools and without the right set of shackles can achieve more than a team of 10.


Plus, we just don’t have — nor are we trying to get — the management structure to support a big team. It’s always been very successful for us to keep the team small, and I definitely want to stay that way.


## 🖌🏗️ ️EMPOWERING CREATORS WITH STUDIO


*So we’re moving away from providing content to our players, to empowering them to make their own worlds. What are we doing in order to bring people into the fold and help make that transition easier?*


I think Highrise is a little bit unique in that we cultivated a community that’s very focused on creativity: UGC is not new for us at all. The very first version of Highrise we launched back in 2015 had the feature called **Design Mode** , where you can basically build your own rooms. Initially we saw that feature as, *oh, someone will decorate their bedrooms.* Very basic — and it’s evolved a long way since then where people built amazing experiences using a very simple tool.


> Our community is already starting off very creative focused and is just waiting for better tools to come along; and that’s exactly what we’re giving them.


Also, last year we released our first open API called the[Bots API](https://create.highrise.game/learn/guides/bots/creating-a-bot) , ***allowing anyone to write a game server themselves*** and use that game server to talk to the Highrise backend using the Bots API.


It’s difficult to use. You have to know Python and run your own game server. Even for me! We didn’t really expect a lot of adoption, but over the last 6–9 months, if you go into any popular room in Highrise, there will be a bot in there doing something, which is pretty incredible. People have built really elaborate games, utilities like viewing your room visitor count, and tons of things way beyond what we thought possible. And I expect a similar experience with Studio and with these tools we’re about to release.


Player-made JIMMY bots take over a room!


*What are some of the most important ways that a platform can empower their creators?*


There’s probably two different kinds of creators. The first class is the folks that are coming ***from the community*** . They started out as users first, and then they converted into creators because they wanted build something for themselves, their friends, or wanted to give back to the community that they came from.


The second class is external. They’re the professionals, the people that come in specifically looking to create, and maybe build a business around it.


I think for the first class, you just **give them more powerful tools** . For them, the incentive is the creation itself; they don’t actually care about the monetary or the financial side of things because they’re just building for themselves.


> The most important thing for these makers is just giving them the audience to consume their creation so that there’s an actual feedback loop there, and they’re not building into the void.


For the second class, I think that’s where the economics comes in. It’s a little bit more straightforward: give them powerful tools so they can build what they want, and then have the incentives and the economics in place for them to be rewarded for their investment.


*On the subject of economics: what’s our status regarding blockchain? We were looking into it a while ago.*


So we’re definitely **not** a blockchain company. We were interested in blockchain technology 2–3 years ago as a way to pay our creators. We’e always sent out creator payments, and we are always looking for better ways to make that process more seamless. The ideal for us is anyone could jump in, create something, and earn from it.


Maybe designing a cool hat or a nice pet, or you’re building an experience and you’re charging for admission. The ideal for you to be able to cash out and buy yourself coffee or pay rent at the end of the month.


The current systems are in place are not ideal. Typically for these kind of online payments, you have to go through KYC, which is a **Know Your Customer** to make sure there’s no money laundering, there’s legal, etc. etc.


Plus, there’s usually a 30 plus day buffer before you can even withdraw. When we originally looked at blockchain, the promise was that you can withdraw instantly, and don’t have to go through these cumbersome processes in order to become a creator.


On the subject of digital ownership: this is very important for us. Highrise as a platform has always been about collections and ownership; just in a different way than blockchain. Blockchain is essentially a distributed database, whereas we’re centralized, but that doesn’t mean that our assets have no value. In fact, Highrise assets have a tremendous amount of value because we have a secondary market where players buy, sell, and trade with each other. There’s defined monetary values of the things that just exist on our database.


## 🚀 WHAT’S NEXT?


*What are some milestones you want to accomplish in the 6 months? Year? Three years?*


Last week we publicly released Studio, our standalone creator application, alongside our first 3D Worlds experiences. With this, we will start seeing user-created worlds that are rich, that have custom code, custom assets, and will taking Highrise way beyond what it’s historically been capable of doing.


That’s a really exciting lot for us. The plan there is we’ll give it a few weeks for these worlds to start ramping up, and then we’ll open up the platform to serve these user-created worlds to everyone.


This is very short term: we’re only looking forward to the next month, but it’s action packed. We’re looking forward the next three months, next six months; there’s just a whole lot more on the roadmap.


### 👋STAY IN TOUCH


[🌐 Our site Pocket Worlds](https://www.pocketworlds.com/company)


[✖ Twitter @itspocketworlds](http://twitter.com/itspocketworlds)


[🗣 LinkedIn](https://www.linkedin.com/company/pocketworlds)
