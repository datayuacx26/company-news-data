---
schema_version: "1.0.0"
document_id: "f2c98fc6795974229f16fa7d1fc8c64f4b4f2d3be9bb8c7e31e82e496123d95c"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/what-should-you-do-when-something-goes-wrong-with-your-mobile-app"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:f6c3a60084a364481ee68e9a7746b4b40235fa6a973a182b054199a783a2c325"
---

# What should you do when something goes wrong with your mobile app, besides panic?

Mobile isn’t the web.


When something breaks on a web app, you can instantly roll back or ship a fix as quickly as you can write the code. This is not how things work on mobile.


Needing to package up a binary for a user to install on their phone creates a wall between you and that user, which significantly alters the process of getting fixes into production. Feature flags can help, but in the inevitable situation when nothing short of a code change in an updated binary will solve a pressing problem, you have no choice but to spin up a fix, submit it to the App Store or Play Store, wait for the new version to go live, and then hope your customers download that latest version ASAP.


This is why careful[Mobile Release Management](https://www.runway.team/features/mobile-release-management) is so important. From thoughtful planning, to thorough testing, to having a very organized process for pre-release fixes, to feature flagging (as already mentioned), to phased rollouts… following regimented steps for each launch is critical to ensuring your app faces as few problems as possible.


But no matter how well you prepare, how thoroughly you test, and how carefully you roll out, there will come a day — perhaps an otherwise beautiful Saturday afternoon — where your on-call engineer will get paged that your app is crashing for 20% of users. They'll leave the movie theater where they had been watching Deadpool vs Wolverine (having annoyed half the audience with their audible PagerDuty alerts) and pull out their laptop to connect to the mall wifi and investigate the problem.


What now? How can you make sure they see the full scope of potential issues and take action on them so that problems are fully resolved, ensuring that the next on-call engineer doesn’t end up getting paged about a similar problem while watching Inside Out 2? What can you do to prepare on-call engineers for this inevitable scenario, so they have the tools and knowledge they need to act quickly during high-urgency situations?


## Create an incident runbook (and keep it up to date). You have a runbook right?


Time is of the essence any time an engineer is pinged or paged. No one likes feeling uncertain or unclear about what to do when figuring out the root cause of a severe problem while sitting outside a Panera Bread (or even sitting at home or in the office). A detailed guide that outlines what to do when a certain kind of alert is triggered is critical to ensuring a speedy and appropriate resolution.


Your runbook should provide guidance on questions like:


- How should an engineer attempt to reproduce the issue? Do they do so on their device? What if the issue impacts a different OS version?


- What level of severity requires an immediate response vs letting it wait? Should they pause a staged rollout?


- Who else should they ping on a weekend and how quickly should they ping them? If it's happening late at night, should they be waking people up to discuss or escalate the problem?


A mobile on-call runbook can and should provide guidance for all of this and more, providing mobile engineers the confidence and support needed to address issues swiftly even in the most time-sensitive and high-pressure situations.


No matter how detailed your runbook is, exceptions that aren't covered will crop up. The runbook must be a living document that evolves with learnings after each incident, even after each on-call rotation. If it becomes a stale Confluence doc that no one has updated in six months, it'll steadily become useless and on-call engineers won't be able to rely on it. With an up-to-date and comprehensive runbook, incident management becomes more efficient, which makes your on-call rotations and monitoring more effective.


One sneaky caveat with runbooks: each manual step they contain creates an opportunity for human error, particularly when it comes to actually spinning up and shipping a critical hotfix for an issue. So, you can also look to your runbook for cues on areas of your process to automate — e.g. in the hotfix case, automatically creating a branch off the last release's tag, cherry-picking a fix in, triggering the build, submitting for review as soon as the build is ready — so that on-call engineers can efficiently run the process half-asleep (because they may very well be half-asleep or otherwise feeling the effects of the boozy Mountain Dew Baja Blast they got at the Taco Bell Cantina on their way to watch Deadpool & Wolverine).


Of course, problems don't only arrive via page when an on-call engineer is watching a movie. What if a member of a particular team comes to you with a bug they claim to be critical but it hasn't yet surfaced as such through any other channels? What should you and your mobile engineering team be on the lookout for?


## Identify key health indicators that can be used to monitor issues that are impacting your users


If you can't see it, you can't fix it. You're reading this post, which implies you're working on (or maybe even leading) the kind of team that either already closely monitors the many problems that can quickly arise during and after a new rollout, or is actively looking to start doing so. So, let's consider everything you and your mobile engineers should be keeping an eye on to successfully understand and take action on the health of your apps — including some things that can't even be measured by error and performance monitoring tools.


Crashes are the best-known type of error that iOS and Android apps experience (which is why they're the example we've used up to this point), and one of the most serious: users are unexpectedly thrown out of their app and have to restart it, which is not a great experience. Most stability monitoring platforms emphasize crash reporting and improving visibility into what happened leading up to a crash — detailed stack traces and breadcrumbs can make a big difference during debugging. Some platforms even offer smart suggestions tying crashes back to commits that might have been responsible.


However, other signals can be equally important in understanding whether your users are seeing issues in your app. For iOS apps, be specifically on the lookout for OOM terminations and app hangs, which are incredibly frustrating for users. If the OS is killing your app because of a memory issue or if your app is unexpectedly unresponsive, you should know about that.


Beyond crashes, app hangs, and OOM errors, issues with your app's performance like slow network requests, frozen frames, lagged renders, and slow startup times could cause a mostly crash-free app to give your users a sluggish, less-than-ideal experience. For many apps, even slight variations in perceived speed and responsiveness can mean the difference between an app that lives on a user's home screen and one that gets uninstalled. Monitor performance, not just crashes.


Issues that aren't easily detected by stability or performance monitoring tools — like problems with usability and functionality — might be more challenging to capture but should get a close look just the same. These kinds of issues may surface first through[app store reviews](https://www.runway.team/product-updates/ai-powered-issue-detection-from-app-store-reviews-custom-user-groups-search-for-build-distro-and-more) (which need close monitoring), and/or users contacting your CX team directly, or could even show up through unexpected changes to DAU numbers or average session lengths.


Sometimes, problems may not be surfaced via errors or complaints, instead arising from changes in user behavior around key flows, conversions, or other events in your app. These sorts of changes are often far from what engineers would typically monitor (a PM is more likely to be watching product and business analytics). Still, they are just as important to key in on as they can indicate an underlying issue that isn't being surfaced elsewhere or help an on-call engineer to get to the root of a problem they’re digging into. As with any other problem, this directly costs your company money. Having this data easily accessible to folks who have the power to take quick action with problems ensures they can catch and fix issues before they cost your org even more money.


## Teach on-call engineers how to understand the scope of the problem and take action to limit its impact


While a runbook is a necessity, all it can ultimately do is point mobile and on-call engineers in the right direction. They still need to make quick decisions and act based on their own judgment.


First, they must ascertain whether the problem is hitting only iOS users, Android users, or both. This information should be accessible through your crash monitoring software, but it may require a little digging as the errors will often be tracked as two separate problems and won't get surfaced at the same time or in the same way.


Simultaneously, the investigating engineer also needs to determine the severity of the problem and use this determination to take action. Our initial example showed crashes impacting 20% of users on at least one platform. This is a running-around-with-your-hair-on-fire problem that clearly demands an immediate halt to the rollout (and potentially even a[quick rollback](https://www.runway.team/rollbacks) ). But the course of action for issues will not always be so obvious. If crashes are impacting 1.5% of users, should the on-call engineer immediately halt the rollout? If not, what should they do? Who should they contact?


As already noted, crashes are not the only problem you have to worry about. What about a 10% decrease in visits to the subscription page? What about a 1 second slower startup time? Or a sudden increase in 1- and 2-star reviews for your newest version? At what level do these issues need immediate action versus being minor enough for a fix to go out with the next scheduled release?


There are an enormous number of judgment calls required here, as even problems that appear marginal can be symptoms of more significant underlying issues — that may one day suddenly become much more serious. Problems that seem small can begin to add up and create a frustrating experience for your users.


Your runbook should include a series of guidelines laying out the steps to take when any kind of problem from several to minor arises, and you should set expectations across teams and disciplines (including product, QA, CX, etc.) as to what the mobile engineering team should and will do in such a scenario. This can help ensure an issue doesn't devolve into the sort of one-off bargaining and politicking that can make it even more stressful to manage problems when they come up. And whenever there is one-off bargaining and politicking (which is bound to happen), it's essential to discuss this and add it to the runbook for future reference.


The best way to ensure an on-call engineer (or any engineer) can take rapid action on problems as they arise is by putting critical information at their fingertips. The more guidance you can give them and the more data you can put directly in front of them from your crash and application health monitoring tools, the less chaotic, stressful, and drawn-out the process of fixing those problems will be.


Imagine if there was a tool that, among many other features, provided a dashboard where all of your pertinent application health data from multiple platforms could be viewed and even automatically acted upon after each rollout. Let your imagination run wild and checkout[Rollouts by Runway](https://www.runway.team/features/rollouts) .
