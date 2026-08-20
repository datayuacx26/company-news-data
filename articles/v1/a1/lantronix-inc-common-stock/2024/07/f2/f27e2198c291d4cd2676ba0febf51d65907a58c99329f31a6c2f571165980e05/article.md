---
schema_version: "1.0.0"
document_id: "f27e2198c291d4cd2676ba0febf51d65907a58c99329f31a6c2f571165980e05"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/when-theres-no-time-to-be-thunderstruck-are-you-ready-for-the-next-crowdstrike-outage/"
published_at: "2024-07-23T19:35:11+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:1c90b5cfaca283dff652c29d400fc6c06cf6ed6da28922d91ad38c4e26cfa2db"
---

# When there’s no time to be thunderstruck… are you ready for the next Crowdstrike outage?

July 23, 2024


- [Industry Trends](https://www.lantronix.com/blog/category/industry-trends/)


### When there’s no time to be thunderstruck… are you ready for the next Crowdstrike outage?


Every time there is a major news event about a global IT outage like this weekend’s Crowdstrike event, my wife asks if Lantronix could have prevented it. The answer is usually, “probably not?” But could our out-of-band products help recover faster? That answer is usually a fairly confident, “maybe.”


Rarely is enough specific information released publicly (at least initially) to conclusively answer the question about if Lantronix products could have reduced downtime from outages like Crowdstrike’s and the resulting flight delays, dead air on broadcast TV, and disrupted healthcare and banking services. But there are some key functionalities in the Lantronix out-of-band portfolio that can make recovery faster.


The Crowdstrike Falcon system is designed to prevent cyberattacks on Windows machines (among others) where it has privileged access to the kernel, which is the core of an operating system. In this outage, a Crowdstrike update with problems impacted the Windows OS kernel, which could impact everything from memory and process management, file systems, device control, and networking. Most of the news reports showed the “blue screen of death” on monitors at airports, hospitals, and other locations running Crowdstrike for Windows. With computers unable to boot into the Microsoft OS, applications were down and the administrators under pressure to recover their operations have limited options—even when the vendor patch is released soon after the issue is discovered.


## Remote Computing Access


The Lantronix SpiderDuo provides KVM-Over-IP. With keyboard, video, & mouse, plus power control, administrators can remotely trigger a power cycle of a remote computer or server and boot the device into safe mode.


This is huge, because while recovery from the Crowdstrike issue is a relatively simple deletion of a specific file, the first step of the Crowdstrike’s remediation procedure is to hold down the power button on the device to force a reboot. Easy to do if it’s one device, or maybe a few sitting near your desk, but if you support devices scattered across the country, without SpiderDuo, you are looking at a long road trip, or lots of painful calls walking remote users through the process.


In addition to remote operation of the computer, with Virtual Media on SpiderDuo, patches and other files can quickly be copied to remote devices. If you are facing a whole deployment of updates, going physically device to device with a thumb drive only slows your recovery. Being able to access computers as if you were right there would let you more rapidly recover from issues. It also stands to note that with the SpiderDuo, you can also access devices directly onsite too (the local user) using the secondary monitor and keyboard/mouse connections.


## What about when it’s a network issue?


From the moment you become aware that your network might have been hacked or is down due to a non-malicious mistake, a clock starts ticking. You want to minimize the impact by locking down impacted or potentially impacted network functions.


The Lantronix AI-driven out-of-band platform connects to network infrastructure devices directly over a console connection, so independently of the network itself. With continuous monitoring, rapid triage is possible to root-cause the issue and start working the problem. Lantronix LM-Series console servers can locally store “safe mode” configurations for network devices, limiting functionality to effectively quarantine sections of the network. Admins can push a config to one device, or with the same effort, thousands deployed across the network. Think of it as a panic button to help restore order.


As the scope of the issue becomes more clear, additional configurations can be pushed over the network or by using an out-of-band link like a cellular modem to bring unaffected services and locations back up across the network, ensuring the fastest return to normal possible. Remember, network resiliency is defined not just by how unlikely your network is to get hacked, but maybe even more importantly, by how quickly you can recover from the unexpected.


During the frenzy of a network outage, especially one that’s going to put your company on the evening news, it’s not only all-hands-on-deck, but often it’s just FIX THE ISSUE at any cost. The first thing that often goes out the window are standard security policies as admins use break-glass passwords and all the usual monitoring and compliance measures are down or ignored. With Lantronix AI-driven out-of-band, all monitoring and access controls continue to function as normal, ensuring an audit trail of who did what, when, and with what effect to the network.


## So, my wife is still asking… could Lantronix have solved this issue?


Most likely in some cases. But it’s a big, complicated world, and the one thing I’m certain of is that I’d rather have Lantronix in my toolbox than having to jump in the car with a laptop to strike out into the unknown.
