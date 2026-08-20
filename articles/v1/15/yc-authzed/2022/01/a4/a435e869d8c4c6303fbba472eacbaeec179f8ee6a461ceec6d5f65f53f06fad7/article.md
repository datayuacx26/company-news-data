---
schema_version: "1.0.0"
document_id: "a435e869d8c4c6303fbba472eacbaeec179f8ee6a461ceec6d5f65f53f06fad7"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/m1-to-the-max"
published_at: "2022-01-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:c67242faecfe3ff63b5f6ced5260b1624f073e39716d7be9d6fca39efb63788d"
---

# M1 to the Max

## Previously on…


In the[Onboarding with an M1 post](https://authzed.com/blog/onboarding-with-an-m1/) , I documented my experience being the guinea pig for getting our applications and development environment running on the then newly released[M1 MacBook Pro (2020)](https://support.apple.com/kb/SP824?locale=en_US) . At the time, many apps and libraries weren’t being built to target the M1’s arm64 architecture so it wasn’t a given that I’d be successful. There were a few minor hurdles to overcome but our applications built and ran great. And it turned out that the M1-based laptop was a pleasure to code and work on. So much so that the rest of my teammates vowed to upgrade their Intel-based laptops when the timing was right…


## Enter the Max


Fast forward to October 18, 2021. Apple[announces](https://www.apple.com/newsroom/2021/10/apple-unveils-game-changing-macbook-pro/) 2 new MacBook Pros available with either the M1 Pro or the M1 Max system-on-chip. We were all eagerly following the announcement live stream that day, taking in the garish benchmark performance numbers and graphs. However, the thing that most captured our collective attention: “and up to 64GB of fast unified memory.”


Shortly after the announcement, engineers at various companies began tweeting about surprisingly fast builds on their new MacBook Pros and subsequently many of their[employers tweeted](https://twitter.com/onmyway133/status/1456910913081159680) about new equipment policies that would give their engineers maxed out M1 Max laptops because **“Engineering time > hardware cost”**


> M1 Max at[@Shopify](https://twitter.com/Shopify?ref_src=twsrc%5Etfw)[https://t.co/JzVmhsFLU3](https://t.co/JzVmhsFLU3)
>
>
> — Khoa 🔥 (@onmyway133)
>
>
> [November 6, 2021](https://twitter.com/onmyway133/status/1456910913081159680?ref_src=twsrc%5Etfw)


We were sold and began looking for in stock maxed out M1 Max MacBook Pros (which is a mouthful to say) for everyone.


*Side note: all the ‘M’ sounds may trigger “semantic satiation” which was explored recently in a[Ted Lasso episode](https://ted-lasso.fandom.com/wiki/Semantic_satiation) .*


## How it started vs how it’s going


Coming from the 2020 M1 MacBook Pro, I’m happy to report that many of the benefits carry over to my M1 Max, namely great battery life and essentially fanless, whisper quiet computing. Arm64 support has continued to improve such that missing arm64 builds are more the exception rather than the rule. And lastly, multiple external monitor support (the downside from the previous post’s epilogue) is heartily addressed with support for up to 4 external monitors.


There are still[some](https://9to5mac.com/2021/11/26/three-monterey-bugs/)[gotchas](https://www.theverge.com/2021/10/19/22734676/apple-macbook-pro-notch-botch-face-id-missing-feature-design) , of course, but we’ll focus on a set of issues we continue to encounter in our day to day work.


### Docker for Mac performance


This is the issue we encounter the most frequently and consistently. Docker for Mac runs in a[Linux VM](https://www.docker.com/blog/the-magic-behind-the-scenes-of-docker-desktop/) and integrates with the host machine for networking and the file system so we expect some overhead. We proactively remove file sharing directories to eliminate them as potential I/O bottlenecks.


Another source of performance issues stems from running emulated images. We’ve noticed a qemu-system-aarch64 process memory usage balloon up to 30GB! Thankfully we can allocate an abundance of CPUs and memory to the VM.


### VpnKit silently stops working


We use Docker for Mac’s kubernetes integration to run our applications locally and specifically use[Contour](https://projectcontour.io/) for ingress. We haven’t been able to track down a root cause but every few weeks all connections to our applications will begin failing and we’ve found that[VPNKit](https://www.docker.com/blog/docker-unikernels-open-source/) has stopped working.


Our solution has been to attempt increasing more dramatic resetting of Docker for Mac state:


1. Reset Kubernetes cluster
2. Reset to factory defaults
3. Uninstall and reinstall Docker Desktop


### No arm64 Discord app


We frequent the[SpiceDB discord server](https://authzed.com/discord) to connect with other SpiceDB users. As such, Discord has become another YACA (Yet Another Chat App) that we keep open throughout the day. ([Join in](https://authzed.com/discord) on the conversation. We’ll be there!) Unfortunately, the app is emulated on M1 machines and noticeably lags when switching between servers.


Thankfully, native M1 support is in the works and currently available to test in their[public beta](https://ptb.discord.com/) .


### Working with x86


If you find yourself without access to an Intel-based Mac and need to work with x86, I’ve come across a useful trick for M1 machines. To run a one off image, you can specify a platform e.g.` --platform=linux/amd64` .


Instead if you want your tools and dependencies to default to x86, you can intentionally run a terminal under emulation.


1. Duplicate your terminal app. Apple’s provided Terminal.app works well for this.
2. Open the “Get Info” panel, select “Open using Rosetta”, and rename the app to remind you it’s emulated e.g.` Terminal x86.app`


3. Run` arch` to confirm


1


2


```text
user@localhost ~ % arch
i386


```


1


2


```text
user@localhost ~ % arch
i386


```


The best part of the M1 Max may be that we simply get to forget about managing system resources during development. In meme form:


#### How it started


*Docker resources on M1*


#### How it’s going


*Docker resources on M1 Max*


We get to throw resources at Docker and go about our day. As a bonus, I no longer see this window after a few days of uptime:


## Is it worth it?


A fully loaded 14 inch M1 Max is approximately $4,000 😱. Is providing these machines a worthwhile investment for an engineering team as the aforementioned tweets proclaimed? We’ll add our build time improvements in for consideration. The following times are from building our entire application stack with no dependencies cached.


Build Wall User System


M1 (serial) 873.05 real 4.29 user 3.90 sys


M1 (parallel) 478.84 real 3.68 user 3.15 sys


M1 Max (serial) 640.30 real 4.38 user 2.82 sys


M1 Max (parallel) 290.25 real 3.05 user 2.13 sys


A ~27% decrease when building our images serially and a ~40% decrease for parallel builds using docker compose. *Note: the posted build times are accurate but not from statistically rigorous sampling.* Turns out repeatedly building with` –no-cache` and thus repeatedly downloading dependencies will get you rate limited.


While a 40% decrease in “waiting for the build” can add up to a significant sum of saved time, I’ve found that the more valuable productivity gain has been tighter feedback loops while coding and debugging. It’s difficult to say quantitatively if and when this investment would be worthwhile.


## Unexpected surprises


On the contrary, we have ample qualitative evidence to justify an M1 Max dev machine and further, even say it’s a joy to use daily. A sampling of unexpected but notable surprises:


My laptop is mobile again. It’s nice to be able to get a change of scenery, sit on a couch, and still run through my full development workflow without my battery draining, burning my lap, or squinting at my screen set to the dimmest setting to conserve power.


The newly “old” keyboard is nice to type on, on its own merits and not just compared to the[maligned butterfly keyboards](https://support.apple.com/keyboard-service-program-for-mac-notebooks) . It’s also nice to have an esc and fn keys back again too.


Websites pop. On a decent internet connection, websites seemingly render instantaneously. My terminal scrolls so satisfyingly smoothly. And Google Meet video calls don’t bring my machine to a halt.


I can’t attach a price to these daily experiences but they are delightful and I would recommend an M1 Max or M1 Pro machine to anyone for these reasons alone.


Thanks for reading if you've made it this far. Look out for Part 3 of this series if and when the M1X or the M2 is announced 🤞!


On this page


- Previously on…
- Enter the Max
- How it started vs how it’s going
- Docker for Mac performance
- VpnKit silently stops working
- No arm64 Discord app
- Working with x86
- Is it worth it?
- Unexpected surprises


## Related


[Company Introducing Authzed Our Journey to Permissions as a Service. Feb 10, 2021 · 8 min](https://authzed.com/blog/introducing-authzed)[Company Introducing Authzed Our Journey to Permissions as a Service. Jake Moshenko · Feb 10, 2021 · 8 min](https://authzed.com/blog/introducing-authzed)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Melissa Smolensky · Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)
