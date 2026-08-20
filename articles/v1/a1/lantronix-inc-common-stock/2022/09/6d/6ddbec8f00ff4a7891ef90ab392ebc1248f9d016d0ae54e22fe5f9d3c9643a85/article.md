---
schema_version: "1.0.0"
document_id: "6ddbec8f00ff4a7891ef90ab392ebc1248f9d016d0ae54e22fe5f9d3c9643a85"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/when-you-have-too-much-to-do-automation-is-everything/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:9e36ec07693310b58a0d26bb0664a6fdf00b245ddda94e303c683d78e37298e8"
---

# When You Have Too Much To Do, Automation Is Everything

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### When You Have Too Much To Do, Automation Is Everything


There’s a quote that gets attributed to Bill Gates a lot but was likely said by Clarence Bleicher in 1947 that goes “ *If you get a tough job, one that is hard, and you haven’t got a way to make it easy, put a lazy man on it, and after 10 days, he will have an easy way to do it.”* That’s pretty much been my mantra since the 11th grade when I programmed my TI-83 calculator to solve physics equations. The reason wasn’t *just* because I was lazy, but because I wanted the answers to be accurate. Sometimes, there’s a connotation that automation is simply the lazy worker’s answer to a job they don’t want to do. However, if you look a little deeper, you’ll find the value in automation is not just that it *does* work but that it *does work accurately.* Every time. Repeatedly.


## A Unique Position


Over the years, we’ve described our product as everything between a *terminal server on steroids* and a *virtual technician in a box* . The phrase that resonates most with me, though, is *automated crash cart* . It brings to mind the image of a datacenter and a simple rolling cart with a monitor and keyboard on top of it. Or, if you’re managing routers and switches, there might just be a laptop with a console cable. Someone might even leave the cart and equipment hooked up so they can come back and check on it later.


But what if that person simply stayed there? What if they were in the middle of a DDOS attack and needed to know when the router’s CPU got above 90% utilization? What if there was an older switch that needed to be rebooted whenever it took too many errors on its FastEthernet0/1 interface? If that person were always there, always checking, and always reporting back, you would know the *second* something went wrong.


If you automate that monitoring, you get the Lantronix Local Manager.


## Satisfying Requirements


Network management may not be the type of *tough job* Bleicher imagined in 1947, but every Network Admin knows that with enough requirements and large enough scale, even the most mundane task can become unwieldly. Say you need to back up the configuration of a router. Pretty easy, right? Log in, run the copy command to send it to some FTP or SCP server, and you’re done. But what if you needed to do that multiple times a day? Or on 500 routers? 5,000? A single fully configured Local Manager can back up the configurations of up to 104 routers as often as you’d like, all day, every day.


Need both the startup and running configs saved? We do that. Need the OS image and vlan.dat file backed up? We do that too. Need all of those files offloaded to a secondary site in case the datacenter goes up in flames? Yep, you guessed it: we even do that, automatically.


Backing up files is all about preparing for disaster, whether that’s a typo in the latest config someone tried to push or a complete hardware failure of a router. Most companies have a disaster recovery policy in place, and Lantronix helps satisfy those requirements by always being ready for the worst-case scenario.


**Did you know?** If you’re managing a router and it simply dies in the rack, you can drop-ship a new factory-fresh router to the site, plug it into the Local Manager, and we’ll automatically detect the loss of configuration and restore it? We call it Bare Metal Restore, and it’s available on every Local Manager.


## Dynamic Response


Our automation is powered by a Rules Engine that can read data from any resource on the Local Manager. All of this data can be combined to trigger automatic responses to specified conditions. When it comes to out-of-band, we often start with Pulse, a feature that automatically spins up the out-of-band connection when it detects a network outage. But what if a network outage isn’t as simple as *not receiving an echo packet back?* What if a true network outage in your environment is an AIS alarm not just on a router’s interface but on a related switch’s interface too? We can write rules that look at both of those values and take action.


We can look at packet errors on multiple routers. We can check bandwidth usage on multiple switches. All the diligent automated monitoring we do creates a vast array of actionable data, and our Rules Engine takes advantage of that. If you have a scenario that is unique to your site, we can help you build rules that would look at the things *you* would look at and take the actions *you* would take.


Preprogrammed automation may try to be one-size-fits-all, but there’s no replacement for your experience and intelligence. Our Rules Engine allows you to leverage that resource and make the Local Manager an extension of yourself.


## Let’s Get Arbitrary


Going back to the crash cart analogy, it’s clear we can’t have someone standing in the racks all day because A) it’s probably really cold in there and B) it’s not a good use of time. So you automate some actions, pull some running configs, and write some rules that look at data we’re already collecting. But what about the edge cases? What about a device that’s not a router, switch, firewall, etc.? Well, we’ve got you covered there too.


With our Rules Engine, **you can run any command you want, gather a response, and take action based on that response.** You can essentially automate any command on any device with a console port and a CLI. No more standing in the cold datacenter. Write the rule, schedule the monitor, and sit back—we’ll take care of the rest.


## Reclaim Your Time


When you consider the breadth of automation available on an Lantronix Local Manager, you can start to see how all of these little things we do add up to one result: more time for you to focus on something else. We’ll back up the configs, we’ll push them down if the router gets wiped out. We can seek out gear running old versions of OS code and upgrade them automatically. Enable enough of these features, especially the ones that take corrective actions, and you’ll see how Lantronix allows you to manage more efficiently, be more secure, and provide accurate automation powered by you.


So what are you waiting for? Put us to work today, and start reclaiming your time tomorrow.
