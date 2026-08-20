---
schema_version: "1.0.0"
document_id: "6347ad765592f24a51fedfa2d9da2be022a7751bf43493134b5a7315a7258591"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/wp-workbench-version-2-0-5-released/135811"
published_at: "2025-11-16T20:24:05+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:1f49cd5ca253d6072e5095cb3f456e0f57e3fdaccde17fb54198c4b5fd9b0469"
---

# WP Workbench version 2.0.5 released

# [WP Workbench version 2.0.5 released](https://forum.virtualmin.com/t/wp-workbench-version-2-0-5-released/135811)


[News](https://forum.virtualmin.com/c/news/5)


[pro-only](https://forum.virtualmin.com/tag/pro-only)


[Ilia](https://forum.virtualmin.com/u/Ilia)


November 16, 2025, 8:24pm 1


Howdy all,


We’ve rolled out WP Workbench 2.0.5 plugin for all Virtualmin Pro users.


Changes since 2.0.4:


- Add support to setup and run own status collection job


Changes since 2.0.3:


- Fix to strengthen remote server support check


Changes since 2.0.2:


- Add support to clean up previously configured module settings when a domain is deleted[forum.virtualmin.com#135610](https://forum.virtualmin.com/t/fail2ban-fails-to-start-after-virtual-server-deletion/135610)


Changes since 2.0.1:


- Add ability to install WP-CLI automatically for all imported instances
- Fix to avoid calling a full cache rebuild unless triggered by a scheduled task
- Fix cache creation issues for the cloned instance


---


As always, if you run into any problems let us know in a ***new topic*** .


4 Likes


[matt.ingram66](https://forum.virtualmin.com/u/matt.ingram66)


November 17, 2025, 12:42am 2


Hello there, how do I update my wordpress workbench to the new version? I am still on 2.01. I tried apt install webmin-virtualmin-wp-workbench but it told me I had the latest version already. I have 2.01 though.


Thank you,
Matt


[Ilia](https://forum.virtualmin.com/u/Ilia)


November 17, 2025, 11:09am 3


Thanks for the heads up, Matt! We’ll fix it later today. Sorry about that.


[kdmiller45](https://forum.virtualmin.com/u/kdmiller45)


November 17, 2025, 3:59pm 4


how is it installed


Keith


[Ilia](https://forum.virtualmin.com/u/Ilia)


November 17, 2025, 6:17pm 5


This issue is fixed now. To install it, just run:


```text
apt-get clean && apt-get update && apt-get upgrade -y


```


[matt.ingram66](https://forum.virtualmin.com/u/matt.ingram66)


November 17, 2025, 8:14pm 6


Thanks Ilia, my logs are much less cluttered now! A great update.


Matt


1 Like


[system](https://forum.virtualmin.com/u/system) Closed


November 27, 2025, 8:14pm 7


This topic was automatically closed 10 days after the last reply. New replies are no longer allowed.
