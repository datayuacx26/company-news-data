---
schema_version: "1.0.0"
document_id: "3ba1a3fd212bea34ce883b6457996bac5b94afbd60865980eaa05f19019cc971"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/why-you-should-shift-left-when-testing-your-mobile-app"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:8f5a252b182d924159ab05555adf112a20dbed8129a82a8f741b5626377174a6"
---

# Why you should shift left when testing your mobile app

If you’re unfamiliar with shift left, it’s the idea that testing, QA, and performance evaluation should be moved as early in the mobile development cycle (and every other development cycle) as possible. Instead of leaving testing to QA or acting like your new feature will be functionally perfect as designed and built — and then being deeply disappointed when you test closer to launch and tons of issues come up — you begin poking at and scrolling through both your code and UI using a plethora of automated and manual testing methods from almost the moment you write your first new line of code.


We don’t usually focus too much on buzzwords at Runway, yet here we are writing the words[“shift left”](https://www.runway.team/blog/the-whys-and-hows-of-setting-up-pre-release-build-distribution) for the second time in as many weeks. Have we become shift left-pilled? Are we in our shift left era? Is this clear interest in an easily labeled, long-running software development trend turning us into a shift left “pick me” as an organization? The answer to these questions is no.


As a broad concept, shift left is about more than just testing your code and your user experience, it’s also part of a philosophy of bringing development, security, and operations (DevSecOps) to every single stage of development. But for the purposes of this blog post, we’re going to focus on testing.


## Why shift left when you can just keep your mobile testing right where it currently is?


The era of moving fast and breaking things is long over. No one says “f*ck it, ship it” anymore. Instead, with every passing year, software development teams embrace more of a “take good care of it, ship it” philosophy that aims to cut down on bugs, user issues, and technical debt, while still moving just as quickly to get new features out to customers.


Shift left is particularly impactful for mobile teams because mobile app updates have inherently slower release cycles. The App Store review process and slower adoption of updates across devices mean it takes longer to find and fix issues that are discovered later in the development cycle. This means issues have a bigger impact on users, and we know that mobile app users in general are less tolerant of buggy, crashy apps.


Shifting left leads to:


### Time and money saved


Retroactively fixing bugs and errors after features are shipped is always going to be necessary. Pobody’s nerfect, as they say, and no amount of early stage testing can eliminate that. It can, however, significantly cut down on problems. As we noted in our[pre-release build distribution](https://www.runway.team/blog/the-whys-and-hows-of-setting-up-pre-release-build-distribution) blog post, bugs shipped to production and fixed after launch are up to 100x more costly than bugs caught and fixed in an earlier stage of development.


That cost multiplier doesn’t only apply to pre- versus post-launch. Within the software development lifecycle, costs go up as you move “right”, closer to shipping a feature. During later stages of QA (e.g. regression testing), the cost of catching and fixing bugs is[3x](https://www.researchgate.net/figure/IBM-System-Science-Institute-Relative-Cost-of-Fixing-Defects_fig1_255965523) more than during development, which is itself[6x](https://www.researchgate.net/figure/IBM-System-Science-Institute-Relative-Cost-of-Fixing-Defects_fig1_255965523) more expensive than simply designing the perfect, issue-free feature from the very beginning (impossible…. for now).


This isn’t only about bugs; validating user experience is key. Getting early feedback on design and implementation of product functionality will save you from spending time on additional iterations down the road, as even the most bug-free app on earth will run into issues if features aren’t implemented in a way that intuitively solves a user’s problems and/or isn’t easy for them to find or use. Users may even forgive buggy apps if they really like what those apps do, which is why Bethesda Software has made tons of money from the Elder Scrolls and Fallout series instead of going out of business.


### Faster delivery of your mobile app


Issues, slowdowns, and problems aren’t only experienced by your end users, they’re also experienced by you and your team. Extensive early testing means continually delivering new builds for other folks to try out, and this in turn means many more opportunities to find flaws, blockers, slowdowns, and other issues with your own tooling and delivery process that might not have otherwise been detected until launch.


### Happier mobile developers


Individual developers do not exist in a silo, their code written in secret and hidden from the world until the day it’s merged to master. Everything an engineer creates is meant to fit into and enhance a greater whole. It’s comforting to know that other engineers, colleagues, and even end users are trying out their work before launch, increasing confidence that what they’re building is functioning as expected, while contributing to an open culture of shared responsibility and collaboration. Instead of worrying about how customers are going to react to a new feature when they get their hands on it, engineers can feel happy knowing that their work has been put through the paces and is in an excellent state.


### Happier product managers


Shifting left plugs PMs into the development process from the very beginning, allowing them to see new features and WIP along the way, give continual feedback and make more iterative changes with a tighter feedback loop. This speeds up product development and (likely) results in a better overall product.


Plus: PMs love getting features out the door bug-free and on time. They often have a whole bunch of KPIs tied to their ability to shepherd engineers towards that goal. The more PMs trust engineers to get a nice, successful, stable release out the door, the more everyone can focus on collaborating towards building new features for your customers instead of developing an adversarial relationship about whether requested deadlines can be met.


### Happier customers


[87% of users](https://www.applause.com/blog/app-abandonment-bug-testing) claim they’ll stop using an app if they regularly encounter bugs or other performance problems. The survey that claims this is from 2013, so maybe people have gotten so used to bugs that they don’t care anymore. But that seems unlikely.


And as noted in the Bethesda example above, customers are obviously happier with better products regardless of bugginess. Shifting left means more iterations, a better fleshed out product, and more frequent updates.


### Happier leadership


Leadership loves this stuff. They eat it up — both because buzzwords like shift left look real good on slides when presenting to the board and also because better results are good things that naturally make them, co-workers, customers, and everyone else feel a lot better about your app.


### Happier world


You get the idea where this is going, so we can probably stop talking about how happy everyone is going to be when you shift left. Shifting left can’t fix all problems and make every mobile app perfect, but it is genuinely helpful.


But is shifting left the only option?


## Why shift left, when you can shift right?


An even better question is: why do one, when you can do both? Shift left, shift right, shift up, shift down, shift everywhere (keeping in mind that shift up, down, and everywhere aren’t terms that have been coined, at least not yet).


As you might guess, shifting right is about moving your testing as late in the development lifecycle as possible. No matter how much pre-release testing you do, the reach of that testing cannot come close to matching the scale of a production environment — even 0.005% of your user base is likely far bigger than the number of people involved in a manual pre-release testing program. A larger audience spread across a greater variety of mobile devices will almost certainly find problems that you simply could not have caught outside of production.


Teams shift right through the use of:


### Feature flags


Shipping new code to production, but hiding it behind a feature flag so that only a small subset of customers (or none) are able to interact with it, allows your team to test new features without worrying about shipping a breaking bug to a large number of customers. After a small percentage of users have interacted with your new feature without issue (or after you’ve fixed any bugs they encountered) increase the feature flag’s rollout or remove it entirely so every user can try out the new functionality.


### Chaos engineering


It’s like a fire drill for your app.[Chaos engineering](https://en.wikipedia.org/wiki/Chaos_engineering) introduces failures and faults to check for unexpected areas of fragility that would only potentially arise during an “emergency” or other disruption. This runs the risk of causing actual problems, so the “chaos” is generally introduced into a non-production environment before being spread elsewhere. Software like[Gremlin](https://www.gremlin.com/) and Netflix's[Chaos Monkey](https://netflix.github.io/chaosmonkey/) assist in managing the chaos so you gain actionable insights from it.


### Error and performance monitoring


Relying on your customers to write angry notes to support or yell at you on Twitter is not widely viewed as an acceptable way to discover problems with your app post-launch anymore. Tools like[Firebase Crashlytics](https://firebase.google.com/products/crashlytics) and[Sentry](https://sentry.io/welcome) can capture every individual crash and slowdown, report on whether your app is meeting arbitrary thresholds that you set (like ensuring 99.95% of sessions are crash free), and provide details as to what’s causing issues so you can fix them before too many people encounter problems. Some issues are simply way too difficult to surface without the volume you get when everyone has access.


Learn more about[shifting right](https://learn.microsoft.com/en-us/devops/deliver/shift-right-test-production) .


## How can your own mobile team tackle shift left testing?


In an ideal world you could shift left all the way back to the invention of the earliest computer, keep it from being created, and save us all a lot of trouble. Your software can’t have any problems if there isn’t anything for it to run on. This world does not exist, so you can instead do these things.


### Test automation


Unit tests, UI tests, integration tests, checking API endpoints; if you were to pick any mobile team at random to see if and how they’ve already shifted left, automated testing would be the one thing they’re likely already doing, just as it’s likely the one thing you’re probably already doing (or are trying to!).


### Pre-release build distribution


There is no combination of planning, design, and test automation that can ensure a perfect product, which is why you should establish an internal testing program so colleagues can tap, scroll, and zoom around your builds to discover bugs and help improve usability.


This means generating installable builds that the testers can actually put on their own mobile devices without jumping through a lot of hoops, and providing them with useful and necessary context so they know exactly what they should be on the lookout for with each new build. As well as collecting and prioritizing their feedback to ensure that action is taken with the information they share.


Pre-release build distribution involves a lot of setup and moving parts, and could take up an entire blog post of its own, which is why we wrote an entire blog post about it. Read all about setting up an[internal testing program](https://www.runway.team/blog/the-whys-and-hows-of-setting-up-pre-release-build-distribution) .


## Beta testing


Establishing pre-release build distribution is also the first step to launching an external beta testing program, which is essentially a splinter of your internal program that you make public.


Fresh eyes from outside the org can come in real handy for new ideas, bug discovery, and drumming up interest in an upcoming launch. What happens when numerous testers with less familiarity with your product and on a wider variety of devices take actions you were not necessarily expecting? Beta testing will help you find out.


## How can LLMs — specifically ChatGPT — help you shift left?


With all the talk of AI replacing engineers, here are ten ways you can leverage ChatGPT to shift your whole team left, becoming a 10x engineer in the process. Crush goals and never ship another bug to production ever again.


1. Pair code with ChatGPT….


Wait, I’m sorry, for a moment I thought I was an AI influencer writing a post on LinkedIn. Never mind, please continue to the closing section.


## Can Runway support your mobile teams’ efforts to shift left?


We can, specifically by supporting your pre-release build distribution through our[Build Distro](https://www.runway.team/features/build-distro) tools; that’s one of the reasons we wrote this post. But we’re not the only ones who can help you do so, even if we consider Runway Build Distro to be the best available option.


There are many options out there: TestFlight, Firebase, App Center, Build Distro (as already noted) and several others. Take time to consider how simple each platform makes it for your team to put builds in front of the right users, provide those users with context around each build, and then helps support you in collecting feedback from them. If you need help deciding, read our post[comparing the top pre-production distribution tools](https://www.runway.team/blog/comparing-the-top-pre-production-and-beta-app-distribution-tools) .
