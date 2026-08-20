---
schema_version: "1.0.0"
document_id: "69717a296187083aa30e39aaba2dcada16e5074cfb4b29d6c8986419a47e7f9b"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/the-runway-2024-year-in-review"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:1c69dd6fc4a36dbef8667ad1ffc632bf93bd3d059aaab5e8f883a7d93508381e"
---

# The Runway 2024 year in review

2024 has now passed Runway by. It’s probably passed you by too, unless you live backwards through time like Merlin the Wizard or those people from Tenet. If you are one of those people from Tenet (they need streamlined mobile releases too, we’d think), don’t read this as we don’t want to spoil the past for you.


As Runway, we’re already hard at work on all our plans for 2025, but we’d still like to take a moment to stop and look back on everything we did in 2024. If you haven’t done so for your own 2024 yet, you should. It all goes by in such a blur, but all the work you do every day and every week and every month really adds up.


Also, thank you so much as always for being a Runway user, potential user, happy hour attendee, conference booth visitor, booster, investor, future shareholder or friend. In the spirit of the season, we even thank you if you consider us your arch nemeses. Whatever brings you to this post, we hope your year has been a good one!


## What did we ship in 2024?


We built more big features, a lot more enhancements, and a longer list of new automations & integrations than any year before.


Here are some of the highlights.


### Fixes


[Fixes](https://www.runway.team/blog/introducing-fixes-by-runway) offers a safer and more consistent way to get late-arriving changes into a release whenever needed. Fixes applies real guardrails that allow you to track, review, and approve any late additions, and automates away the busywork and context-switching required to actually get changes pulled in.
‍


### AI-powered user review analysis


Your app gets reviews. Most of them are good, but some of them are not so good and may indicate a bug or other problem with your app. Still others might be because someone had a late delivery, didn’t think their wings were cooked long enough, felt that your subscription plans were too expensive, or had a bad experience talking to support.


Simply evaluating these reviews based on sentiment isn’t enough, since that would include those reviews which have nothing to do with how well the app is performing. So Runway considers the context of your company and app in order to single out bugs and broken functionality, using AI to analyze all of your incoming app store reviews and identify common issues mentioned in them.


### Release digest emails


You and your teammates can subscribe to recurring digest emails that pull together info on recent and upcoming releases. Stay in the know on what you’re shipping, when you’re shipping it, and key metrics about post-release app health. These emails include things like adoption stats across live versions, detailed stability and observability metrics as configured in Runway, info about release schedules and timing, an overview of pending and completed work and fixes, upcoming release pilots, and direct links into Runway for each release in question.


### Ping all pending owners


Never again will you need to track someone (or someones) down in order to keep your release on schedule. We’ve added a “Ping all pending owners” action to the Feature Readiness step. When you click the button to ping, Runway will send a notification to Slack tagging the respective owners of any outstanding code and/or project management tickets associated with the pending work.


### Build Distro v2


We added[Build Distro](https://www.runway.team/features/build-distro) endpoints to our public API that allow you to upload builds, as well as create, read, and update buckets. Plus, we now automatically post comments containing build information and install links for release candidates that contain code associated with a given ticket. Finally, we added full search functionality to Build Distro so that you can search by build number, commit message, PR title, and other fields to find the right build in seconds.


### Release pilot scheduling


Runway’s existing release pilot rotation management can now connect to a scheduling tool like PagerDuty or OpsGenie. Runway will manage your release pilot rotation accordingly, automatically assigning pilots to releases based on a given on-call schedule, swapping folks when coverage changes, and re-assigning pilots if a release rolls over into another team member’s shift. Reminders and alerts can be sent along the way, so there are no surprises or gaps in coverage.


### Update metadata and release notes, whenever


As soon as a release exists in Runway, you can update your metadata and release notes so that they’re there and ready to go whenever you submit to the app stores. This also means that translation work can begin immediately (via our new translation integrations noted below). No more rushing to copy and paste at the very end of your release process.


### Checklist items and gating for Rollouts


We brought Runway’s checklist items functionality to the[Rollout](https://www.runway.team/features/rollouts) page so you can assign and track any post-release tasks the team needs to perform. With these here, you can also now pair checklist items with our rollout automations. So we’ll monitor your rollout and configured health metrics as usual, but even if health and adoption conditions are met, Runway won’t trigger the acceleration to 100% unless all required checklist items are also complete.


### Timeline event search


Search the timeline to track down specific actions, or understand a sequence of events within a release. This search is available anywhere your release timeline is accessible within Runway.


### Quick Actions menu


This surfaces shortcuts to many of the most important views within Runway. Enter Command + K / Ctrl + K from anywhere in Runway to start a search. You’ll never have to touch your mouse again, at least if your only goal is to find something in Runway.


### Automatic user review translations


Runway will automatically translate any non-English reviews and surface those translations alongside the original reviews in Slack (if you have store review notifications enabled). No more asking ChatGPT what a French customer meant when they wrote “Cette application plante toujours à chaque fois que je l'ouvre. Mais j’aime et respecte toujours les ingénieurs qui l’ont construit.”


### Sync app store metadata automatically


If you check your app’s metadata into version control in your repo, you can now have Runway automatically read all the store metadata and sync it with the app stores. Leveraging your existing version control integration in Runway, you simply enable the new automation, specify the file path, and Runway will automatically grab metadata and populate it in the stores as each release is kicked off.


### More integrations for everyone


We also shipped nine new integrations:


- New Relic (Monitoring)
- Huawei App Gallery (Distribution)
- Pager Duty (On call)
- Opsgenie (On call)
- Crowdin (Translations)
- Localise (Translations)
- Samsung Galaxy Store (Distribution)
- Dynatrace (Monitoring)
- Split.io (Feature flags)


See them all on our[Integrations page](https://www.runway.team/integrations) . And you want to really dig into all our new features from the past year, read our continually updated and very detailed[Product Updates](https://www.runway.team/updates) page.


## Events


This year wasn’t just about features, it was also about traveling on planes to distant locales to meet other mobile folks.


In 2024, we sponsored more conferences and hosted more happy hours than we ever have before. We traveled all around (part of) the world to spend time with other folks from the mobile community at:


- [AppDevCon](https://appdevcon.nl/) (Amsterdam)
- [ReactNativeConnection](https://reactnativeconnection.io/) (Paris)
- [Android Makers](https://androidmakers.droidcon.com/) (Paris)
- [Deep Dish Swift](https://deepdishswift.com/) (Chicago)
- [Swift Craft](https://swiftcraft.uk/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-8tGWHxB-HRAjSiDKlbWc0hkWmcKSnLMbx-9_E38l254CqLHITkd3s9xDQ6fDRCU6up6C9B) (Kent, UK)
- [droidcon San Francisco](https://sf.droidcon.com/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-8tGWHxB-HRAjSiDKlbWc0hkWmcKSnLMbx-9_E38l254CqLHITkd3s9xDQ6fDRCU6up6C9B) (uh, San Francisco)
- [One More Thing Conf](https://omt-conf.com/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-8tGWHxB-HRAjSiDKlbWc0hkWmcKSnLMbx-9_E38l254CqLHITkd3s9xDQ6fDRCU6up6C9B) (Cupertino)
- [droidcon Berlin](https://berlin.droidcon.com/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-8H4SOYLff88q3loaSzpT9r0z256GXiguwzNloOy8i7gCA2AhmmyFVbb4kALK9Vd3lCoqH5) (Berlin)
- [Chain React](https://chainreactconf.com/) (Portland, Oregon)
- [NSSpain](https://2024.nsspain.com/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--rHlsuw4wAimx2Fy2xFnWxTJ1dH4RDt3Zd0Vurfqa-3BabHaOL_9QShkoVwti98lgBcUkJ) (Logroño, Spain)
- [droidcon NYC](https://nyc.droidcon.com/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--rHlsuw4wAimx2Fy2xFnWxTJ1dH4RDt3Zd0Vurfqa-3BabHaOL_9QShkoVwti98lgBcUkJ) (Queens)
- [Swift Connection](https://swiftconnection.io/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--rHlsuw4wAimx2Fy2xFnWxTJ1dH4RDt3Zd0Vurfqa-3BabHaOL_9QShkoVwti98lgBcUkJ) (Paris)
- [SwiftLeeds](https://swiftleeds.co.uk/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--nQVNXUOCIiSiA3VRDkrkQuw0NrG1G7MkB1iloi2V4QxEQ0_felKrYC6Y4SD8Cke-UN_cq) (Leeds, UK)
- [droidcon London](https://london.droidcon.com/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--nQVNXUOCIiSiA3VRDkrkQuw0NrG1G7MkB1iloi2V4QxEQ0_felKrYC6Y4SD8Cke-UN_cq) (London)


Lots of trips to Paris and the UK there. In these post-Covid years, it feels like Europe has become the center of the mobile conference community while the US is just starting to come back with strong events like Deep Dish Swift (which we’re thrilled to be sponsoring again in 2025).


On top of these events, we also hosted a ton of happy hours in Amsterdam, Chicago, San Francisco, Cupertino, Berlin, Portland, NYC, Copenhagen, and Sao Paulo, with anywhere from 20 to 200 people in attendance at each.


Shot taken four hours into our WWDC happy hour across the street from Apple HQ


Regardless of where and when we did it, we met with over 3,000 people and gave out over 1,500 of our signature, miniature lego airplanes, and countless t-shirts, stickers, pilot wing pins, postcards, Biscoff cookies, and (for a few lucky winners) LEGO space shuttles and LEGO Concordes.


And we gather for a company offsite in April in Barcelona. We collaborated, connected, brainstormed, hacked, ate too much good food, played too much Quiplash, and got too sunburned while floating around on a boat.


We’re spread across North America and Europe, with team members in NYC, Seattle, CDMX, Baja, the Canary Islands, Montevideo, Boston, Austin, Dallas, Lisbon, and Valencia. Being so spread out means we don’t often get to see each other outside the confines of Zoom and Slack, which is why we all travel to an offsite on occasion to spend time together in one place.


Barcelona is a very nice place to have an offsite


2024 was a wonderful year for Runway. We hope it was also a great one for you and your team. And we’re looking forward to an even more wonderful year in 2025.


Happy Holidays, Happy New Year, Joyous January, Fabulous February, and many additional alliteratively awesome months ahead.
