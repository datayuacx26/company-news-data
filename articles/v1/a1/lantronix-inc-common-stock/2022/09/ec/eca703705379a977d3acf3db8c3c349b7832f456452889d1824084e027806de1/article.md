---
schema_version: "1.0.0"
document_id: "eca703705379a977d3acf3db8c3c349b7832f456452889d1824084e027806de1"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/feature-spotlight-surgical-rollback/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:5cbdbbc6480736f43002b92dee435cc5a96bebab28b0f4e2470a1432a01c648a"
---

# Feature Spotlight: Surgical Rollback

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### Feature Spotlight: Surgical Rollback


I remember exactly where I was the day I first learned about Surgical Rollback. I was just out of college, recently promoted to Network Analyst, and trying above all else to simply not break the Texas Higher Education Network. A request came in over email. *Daniel, go to this router and remove this IP from access-list 301.* Sounded simple, right? I could do that. As a recent graduate with a degree in English, I was more than prepared for this moment. So I logged into the router, issued the *no access-list* command with the IP address *clearly* specified, and then… disaster. The entire access-list was gone. As my fellow router jockeys screamed in unison, I wondered to myself, could anything have prevented this? Besides a CCNA certification, probably not. But there was something that could help fix it: **Surgical Rollback** .


It was 18th century English poet and prophetic futurologist Alexander Pope who once said, *to err is human, but to take down the network is unforgiveable.* This is one of the reasons why backing up configurations is so important. In the dark times, backups were kept in a central repository through telnet-based programs like RANCID. If someone made a mistake, the backup configuration could be transferred back to the device, either by TFTP or pasting into the CLI. In the case of blowing away an ACL, the remediation is pretty simple: just paste in the access-list lines from the backup config.


But what if something worse happens?


What if you are trying to change the subnet mask on GigabitEthernet0/1 but instead of typing **ip address…** you type **shut** ?


As Alexander Pope would say: **you done goofed.**


Fortunately, you have Lantronix, and Lantronix has your back.


# A Safety Net


There are so many bad situations that can be mitigated by having some kind of advanced remote network management platform directly connected to the console port of your network device. The Lantronix Local Manager, for example, pulls the running config on a schedule *and* every time it detects a change during a terminal session. This means the last known-good config is always there, waiting to be redeployed if something goes wrong.


```text
[super@LantronixLM (port1/1)]# show directory  Type Version Name  ------- --------- ------------  Config   Running*   current running-config   previous running-config    Startup*   current startup-config   previous startup-config    Running-Undo*   candidate running-config-undo3405771268234537483.img  OS   current c2960-lanbasek9-mz.150-2.SE11.bin   previous c2960-lanbasek9-mz.122-44.SE6.bin   Post   current readPost
```


As useful as this is, it’s still just a net that catches you when you fall. You still have to do the manual work of climbing out of the net and making your way back up the rock wall. If you just took down the network, then every second counts. You don’t have time to wait for the out-of-band connection to spin up automatically so you can log in and push the last known-good config down.


Fortunately, you have Lantronix, and Lantronix not only provides a safety net, but a bungee cord too.


## A Bungee Cord


The access-list scenario is pretty tame compared to accidently shutting down an interface on a router, the very same interface providing network connectivity to the entire site, including the Local Manager you’re using to make the changes to the router. We call this the ol’ *taking down the network while we’re using it* scenario, and it’s something we’re built to handle with grace and aplomb .


Surgical Rollback works in tandem with a feature called Automatic Rollback. It’s actually AR that detects when a user disconnects uncleanly (e.g., when they *shut* an interface they shouldn’t have). If changes to the running config are detected, then Surgical Rollback is called in to undo them. The last thing we want to do is simply push down the previous config and hope for the best.


A, that’s just inefficient. And 2, as we know, Cisco configurations are cumulative. This comes into play with commands like **shutdown** , as there is typically no command in the previous config that can undo it ( **no shutdown** would be a default). If you shut an interface and push the previous config, it simply won’t fix it.


That’s where Surgical Rollback comes in. It analyzes the changes and issues the appropriate configuration lines to undo the previous work.


```text
AUS-CORE#conf t  Enter configuration commands, one per line. End with CNTL/Z.  AUS-CORE(config)#int GigabitEthernet0/1  AUS-CORE(config-if)#shut  AUS-CORE(config-if)#  *May 26 19:05:38.666: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down  *May 26 19:05:40.663: %LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down  *May 26 19:05:41.669: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down
```


Above is an example of issuing a shut on an interface. If the shut causes the network to drop out from under you, you won’t see the console messages—you’ll just be disconnected. You can simulate this in a lab by simply closing the Terminal (or your SSH application) window. That’s when the magic happens.


1. The Local Manager detects the abrupt disconnect
2. It logs the user out and logs back in using its functional / service account
3. It pulls the running config and compares it to the previous
4. If changes are found, it builds a snippet of config to undo the changes
5. The snippet is pushed down and merged with the running config


You can see the process using **terminal shadow** .


```text
AUS-CORE#more flash:/staging-running-config  interface GigabitEthernet0/1  no shutdown  end  ^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^Z^  AUS-CORE#copy flash:/staging-running-config running-config  Destination filename [running-config]?  128 bytes copied in 0.025 secs (5120 bytes/sec)  AUS-CORE#delete flash:/staging-running-config  Delete filename [staging-running-config]?  Delete flash:/staging-running-config? [confirm]  AUS-CORE#
```


By using the exact commands necessary to undo changes, we ensure nothing else is touched. Just like me at my first job, the Local Manager’s personal motto is **at a minimum, break nothing** .


When this network-down scenario happens in production, Automatic Rollback and Surgical Rollback will undo the changes and bring the network back up in less than 60 seconds, a full minute sooner than Pulse can detect the network outage and spin up an out-of-band connection.


Often, the Local Manager can restore access in the time it takes the user to realize what they’ve done, hang their head in shame, take a sip of coffee, and restart their SSH session.


Even if you fall, we pick you up and put you right back where you were before you slipped.


## Always Watching


The best thing about Surgical Rollback (and its sibling, Automatic Rollback) is that they’re always watching, always waiting. Sure, they may snicker at your typos, but they’ll restore configs while they’re doing it. The Local Manager’s only concern is keeping that device running the way *you* want it running. If you don’t tell it *I meant to shut that interface* , it will undo your change for you.


Before we go, let’s return to access-lists for a moment. It’s important to note that the Local Manager sees change by looking at the differences between two running configs, one pulled before your session and one pulled after. Running a *no access-list 301…* command would result in the loss of the entire 301 ACL. The Local Manager wouldn’t know specifically which command you ran to make that change, but it does know how to restore it.


```text
Disconnecting ...  Logging out of device...  Retrieving running-config from device ...  Failed to transfer 'running-config' via TFTP  Could not retrieve running-config.  Unable to retrieve configuration via network.  Attempting to retrieve configuration using the console...  Complete. running-config pulled.  running-config saved to archive as current.   Changes made in current terminal session:  -access-list 301 permit tcp host 10.10.10.1 any  -access-list 301 permit tcp host 11.11.11.11 any  -access-list 301 permit tcp host 12.12.12.12 any
```


That list of changes is what we use to turn around and put lines back into your router. We don’t push the entire config; we only undo what was changed.


These days, blowing away an ACL is no big deal. Simply exit the terminal session, answer R(ollback) to the prompt, and the Local Manager will take care of the rest. When it’s done, you can terminal in and try again.


Surgical Rollback makes a great demo , so if you want to see it in action,[shoot us a note](https://www.lantronix.com/about-us/contact/) and we’ll put something on the calendar.
