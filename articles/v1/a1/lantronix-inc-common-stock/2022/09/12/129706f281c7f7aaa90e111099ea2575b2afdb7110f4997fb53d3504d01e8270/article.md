---
schema_version: "1.0.0"
document_id: "129706f281c7f7aaa90e111099ea2575b2afdb7110f4997fb53d3504d01e8270"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/there-when-you-need-it-testing-your-out-of-band-connection/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:c1b54dbf24feb210bc81de43906d9c522f97556139de7e7e0ea6d0c22f3c5090"
---

# There When You Need It: Testing Your Out-of-Band Connection

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### There When You Need It: Testing Your Out-of-Band Connection


It always happens the same way. You spend the time deploying your out-of-band solution, you configure all the settings, and you test it once to make sure it works. Then you forget about it. And why not? You have a redundant network with five 9s reliability. You’re golden. But then one day, when you least expect it, the network does go down. You try to dial in or a Lantronix LM-Series Console Server tries to spin up the out-of-band connection, and it just doesn’t work. The LM is still there, monitoring and taking action, but it’s isolated.


Later, after you begrudgingly drive out to the site and fix the network, you’ll find that someone tripped over the phone line and disconnected it from the LM; or a small child removed the antennas from the cellular modem to use as drumsticks; or the SIM card was deactivated by your Service Provider. Whatever the reason, the out-of-band connection wasn’t there when you needed it, and that’s just not acceptable in the world of network management.


So what can we do? Dial into the LM every day? Turn on PPP every day?


Yes. That’s exactly what we can do. And by “”we,”” I mean the LM-Series console server, and I mean automatically on a set schedule.


# What should I test?


Every out-of-band solution has multiple components. The more of those components we can test, the more we’ll be able to rely on the solution working when we need it.


Consider a typical LTE out-of-band solution and the questions we need to ask:


- Is the physical hardware functioning?
- Is a SIM card inserted?
- Is the SIM card activated and provisioned correctly?
- Do we have good signal quality?
- Is the correct APN configured?
- Can we establish the data connection and get an IP address?
- Can we establish the VPN connection? (if configured)
- Can we send a heartbeat to the Control Center?


Rather than testing each component individually (and we could, if you really wanted to), Lantronix recommends an end-to-end test of the out-of-band solution. After all, the signal quality doesn’t matter if the SIM card isn’t inserted, and the VPN connection status doesn’t matter if we can’t establish the initial data connection and get an IP address.


Here are some options for testing your out-of-band connection.


## Out-of-Band Heartbeat Test


Who it’s for: customers using mobile-originated (dial-out) out-of-band that connects the LM back to the corporate network or directly to the Lantronix Control Center (e.g, via DMZ or port forwarding)


What it does: spins up the out-of-band connection, sends a single heartbeat to the Control Center, and records the results


Testing out-of-band heartbeat is as easy as scheduling the outband cycle heartbeat command to run on a regular interval. Because of its low bandwidth usage, you should be able to run this job every day, though we have found most customers prefer to run it once a week. If any component of the out-of-band connection fails, an alarm will be generated. You can then troubleshoot the problem and fix any issues while the in-band network is still in good working order.


The outband cycle heartbeat job can be scheduled from the LM’s CLI with the config schedule command from the modem resource or from the Control Center using the Schedule button on the LM Summary Page.


## Out-of-Band Duration Test


Who it’s for: customers using mobile-originated (dial-out) that connects the LM to a public IP address with no access back to the corporate network or Control Center


What it does: spins up the out-of-band connection, gets an IP address, and records the results


If the Control Center is not reachable while out-of-band, the next best option is to schedule a repeating outband cycle duration job. This does everything the heartbeat job does with the exception of sending a packet to the Control Center. It also keeps the connection up for 1 minute (by default). It has the same low bandwidth usage and will alarm if any component of the out-of-band connection fails.


The outband cycle duration job can be scheduled from the LM’s CLI with the config schedule command from the modem resource or from the Control Center under Schedule on the LM Summary Page.


```text
[admin@LantronixLM (modem)]# config schedule pppCycleHeartbeat -d 86400  Validate scheduled job(pppCycleHeartbeat)? (This will execute the job now.) (y/n): y  Job 11 was scheduled.
```


## Modem Monitors


Who it’s for: customers using mobile-terminated (dial-in) out-of-band, customers who want to query the modem directly to make sure it is working


What it does: sends commands directly to the modem and takes action based on the responses


Modem monitors are a great way to ensure the physical hardware is working. The actual commands sent to the modem will differ based on the technology, but if any of the values are out of line (e.g., NO DIALTONE), an alarm will be generated. At the most basic level, just being able to communicate with the modem is itself a test. If that fails, an ominous UNABLE TO COMMUNICATE WITH DEVICE alarm will be generated.


For v.92 modems, you can run the config monitor modem modemBasic command from the CLI on the modem resource. This schedules our default monitor and ruleset, which will check for dialtone and voltage.


For cellular modems, you can run the config monitor modem command from the CLI on the modem resource. While we don’t have a cellularBasic ruleset (outband cycle is a better test), the modem monitor will at least test communication with the modem. Customers using SMS-Initiated PPP should already have an SMS monitor scheduled, and this will be sufficient for testing.


```text
[admin@LantronixLM (modem)]# config monitor modem modemBasic  Validate scheduled monitor(modem)? (This will execute the job now.) (y/n): y  Job was scheduled 8: [Interval: 00:00:30 Mask: * * * * *] rulesMonitor modem none modemBasic 30
```


## Rest Assured


When someone unplugs the phone line, we need to know about it. When a SIM card gets deactivated, we want to get that fixed before it becomes a bigger problem. Regular out-of-band testing makes sure there are no surprises, and the Lantronix LM-Series is more than happy to do just that 24/7/365.


A Lantronix Deployment Specialist should have helped you set up regular out-of-band testing during your initial setup, but if you’re noticing some testing gaps in your deployment, please feel free to reach out to our LEVEL Technical Support department. We’ll look at your setup and recommend some options to keep everything running smoothly.
