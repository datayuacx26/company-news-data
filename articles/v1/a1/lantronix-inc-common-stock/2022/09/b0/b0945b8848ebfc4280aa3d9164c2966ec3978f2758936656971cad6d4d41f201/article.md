---
schema_version: "1.0.0"
document_id: "b0945b8848ebfc4280aa3d9164c2966ec3978f2758936656971cad6d4d41f201"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/feature-spotlight-virtual-ports/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:63a925735ae2692eee6b0a313c1dbad9d444d9a25c7480ec11e45a087fceedf9"
---

# Feature Spotlight: Virtual Ports

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### Feature Spotlight: Virtual Ports


Of the many profound and metaphysical questions plaguing Philosophy majors today, none may be so important as **why are hotdogs sold in packs of 10 and hotdog buns in packs of 8** , followed closely by **why does my Local Manager only have 56 serial ports when I clearly have *57* routers?** Lucky for you, our founder and technological visionary, Phinneas Hubert Lantronix, already considered these questions all the way back in 2003, when the first Lantronix Envóy (the precursor to today’s LM83X and LM80 advanced console servers) rolled off the assembly line with a paltry four serial ports. Phinneas envisioned a world where datacenters had more than four routers and switches, and even a world where routers and switches didn’t have RS-232 serial ports at all. What would we do then? The answer I’m deftly segueing into is, of course, **virtual ports** .


Thanks to Phinneas’ vision, we can confidently say *even if it doesn’t have a serial port, we can manage it.*


In today’s Feature Spotlight, we’re taking a look at Virtual Ports!


# What is a Virtual Port?


A virtual port is just like a standard port on a Local Manager except that instead of being directly connected via an RS-232 serial cable, we connect **over the network** to the end device using a variety of protocols like ssh-vty, ssh, telnet (RFC-2217), and even a USB to serial adapter. All you have to do is tell the Local Manager where to go (IP address, TCP Port), which protocol to use (SSH / Telnet), and how to log in (username / password), and it will start maintaining that connection as if it were plugged in directly. The virtual port connection becomes transparent to the user, and they can manage the device without worrying about how they’re getting there.


An example of a Local Manager connecting over the LAN to devices using virtual ports (blue) and console connections (orange).


## Why would I use a Virtual Port?


Aside from simply running out of physical serial ports on a Local Manager (why do you have so many network devices?), there are several reasons why you might want to use a virtual port.


**Distance** – Did you know there is only so far you can run a standard shielded Cat5 cable? After a certain point, the signal degrades so much that communication becomes impossible. While there are other solutions like powered RS-422, sometimes it’s just not practical or possible to run a cable to the end device.


**Existing Connection** – What if the end device’s console port is already in use or connected to an existing console server? A physical Lantronix Local Manager (or a VM Local Manager which is *all* virtual ports) can integrate with existing console servers and piggyback on those serial connections, basically adding automation, monitoring, and security to what you already have.


Virtual Local Managers were specifically designed to integrate with existing solutions


**No Serial Port** – Times are changing, and we’re already seeing manufacturers drop RS-232 serial ports completely in favor of USB. But if the only connection a device provides is TCP, then we will gladly use that instead.


## Are there any drawbacks?


Since the dawn of Lantronix, we’ve tried to limit our reliance on the network by connecting directly to devices via an out-of-band serial connection. Even when Local Managers are connected to the in-band network, we prefer IP addresses over hostnames and private, dedicated Ethernet connections over routing traffic through the LAN. Our solution doesn’t depend on tools like SNMP, ping, or syslog simply because **you can’t reliably manage the network *over the network*** .


Ultimately, it comes down to questions of fault tolerance and acceptable risk.


For example, if the Local Manager is in Austin and it has a virtual port configured to a device in El Paso (a mere 576-mile, 9-hour drive away), then you have to think about the dozens, possibly hundreds of individual pieces of network equipment that connection is going through. Any failure along that path would cause the virtual port to stop working; management and visibility functions would be lost.


On the other hand, if the Local Manager and the end device are in the same rack and plugged into the same switch, then that’s less risky. You still have the same problem if the switch goes down, but most likely, we’re managing that switch anyway, so you can get it back up and running quickly and restore that virtual port connection.


Virtual ports may come with reliance on the network, but the risk is manageable.


## Are there any limitations?


Virtual ports rely on the network being up, which includes every network device between the Local Manager and the end device. The end device must also be in a state where it can pass traffic over the network (i.e., not in ROMMON mode). When these requirements are not met, several features become unavailable.


- Out-of-band connection (LAN independence)
- Bare metal restore
- ROMMON recovery
- Password/configuration recovery where boot loader configuration is required
- POST data collection
- Automatic Rollback
- xmodem and ymodem file transfers


Other than that, the same advanced drivers that work on physical ports still work on virtual ports. We can pull and back up configs, manage access, and monitor the device.


## How do I get started?


If you’re ready to get started with virtual ports, check out our documentation here: Virtual Ports.


A few notes:


- A license is required for virtual ports. If you’d like a test license so you can evaluate the feature, please let us know.
- The Lantronix LM80/LM83X supports a maximum of 16 virtual ports.
- The Virtual Local Manager supports a maximum of 48 virtual ports.
- Virtual ports will appear on Slot 5 of an LM80/LM83X


## Like regular ports, only not.


Virtual ports are great because you can connect to anything that is reachable over the network and put the Local Manager’s automation, monitoring, and security to work even at a distance. While we tend to focus on network- *independent* management, our only real goal is to make sure you have all the tools you need to manage your network efficiently and securely.


Be on the lookout for our next feature: **Quantum Ports** . Scheduled for release on 4/1/2025, quantum ports won’t require a serial or network connection, but will instead rely on quantum entanglement to back up configs and operating system images. Of course, simply querying the end device changes its state, but our engineers are hard at work trying to figure out a way around that.


In the meantime, if you’d like to talk more about Virtual Ports,[drop us a note online](http://lantronix.com/to-buy/contact-me/) ! Already a customer? Our support team is standing by 24/7/365 at[\[email protected\]](https://www.lantronix.com/cdn-cgi/l/email-protection) and 888.663.6869.
