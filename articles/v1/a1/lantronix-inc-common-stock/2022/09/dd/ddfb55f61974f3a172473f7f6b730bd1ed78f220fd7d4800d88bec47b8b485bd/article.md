---
schema_version: "1.0.0"
document_id: "ddfb55f61974f3a172473f7f6b730bd1ed78f220fd7d4800d88bec47b8b485bd"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/if-it-has-a-serial-port-we-can-manage-it/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:8cb7158035479dcaee090189be7717c88ddb676fe9cdc32b2fe77629aed36d31"
---

# If It Has a Serial Port, We Can Manage It

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### If It Has a Serial Port, We Can Manage It


One of the most common questions we get from new and existing customers is whether the Lantronix Local Manager can support an *XYZ SynergyDynamics* *Router Model 390000* . Our reply is always the same: **if a device has a serial port** that you can connect to with a laptop, then **we support it** . This is possible thanks to a trio of drivers that determine the level of automation and control we can exert on a managed device. Getting you connected, whether that’s in a data closet down the hall or a datacenter on the other side of the world, is just the beginning. In today’s post, we’ll be looking at the roles each of our drivers play.


Oh, and if you are using the 490000 model of the *XYZ SynergyDynamics Router* , then it doesn’t even have a serial port for us to connect to—just a web interface running on port 80. Well, if that router is isolated behind a NAT at a remote location, we can get you access to it through Port Forwarding and reverse tunneling… but that’s a topic for another day.


For now, let’s meet our drivers.


# Advanced Drivers


If (like me) your version of **knowing how to do a password recovery on a Cisco router** involves a first step of Googling *cisco router password recovery* , then our advanced drivers are the killer feature you’ve been looking for. Lantronix Device Integration Specialists have worked tirelessly to take all the guesswork out of managing a suite of network devices, including Cisco routers and switches, Junipers, SeaTels, and more. Enabling advanced drivers is as easy as configuring the Local Manager with a username and password. After that, a whole host of features become available.


For example, enabling our Cisco Advanced Driver turns on:


- Automatic configuration backup for both startup and running configurations
- Automatic vlan.dat backup (where applicable)
- Automatic operating system backup
- Automatic recovery in the event of configuration loss or corruption
- Automatic ROMmon recovery
- Password Recovery (requires managed power)
- Configuration Rollback
- Chassis monitoring
- Log monitoring
- Alarm notifications
- Event notifications
- Terminal access
- On-demand servers: FTP, TFTP, SCP
- Serial transport protocols: xmodem, ymodem, zmodem (where supported)
- Port locking
- Terminal shadowing
- and more…


This list is what we mean when we say *Out-of-Band Everywhere* . This isn’t just access; this is monitoring and management beyond anything you’ve seen before. Advanced drivers are a jumping off point to full automation of your network.


But what about your *XYZ Router* ? The one we don’t have an advanced driver for?


Glad. You. Asked.


## Native Mode


We work hard to make sure we have advanced drivers for all of the most popular network devices out there in the world today, but it’s a never-ending game of catch-up as new gear is released. If the device you want to manage is too new (or too old) but has a serial port, then the native driver is the one you’re looking for. It may not be as exciting as the advanced driver, but it does give you secure, reliable access to your managed devices.


Native drivers are a great option when you’re looking for other things you can connect to your Local Manager. We may not be able to pull the running config (if it even *has* a running config), but having it connected gives us both access and insight. For example, link lights are driven by electrical signals coming over the cable from the end device. If that device loses power, the link light goes out. That’s something the Local Manager can detect, alarm on, and send email notification about.


As shown in the screenshot above, the **show serial** command contains useful information about the serial link between the Local Manager and connected device. DSR and CTS are used as a software-based link light, while RX and TX show you how many bytes are coming across the link. Lots of TX but no RX? The device may no longer be connected, or the baud rate is wrong. So even if we’re not doing any management, we can still get a ton of value out of our native driver.


If advanced versus native sounds too “night-versus-day” to you, then we’ve also got something in the middle that might work better.


## Enhanced Driver


It would be a shame to connect your *XYZ 390000* and not take advantage of the unique position of having a computer connected to your device at all times. Luckily, if you want to add some automation and we don’t have a specific advanced driver, we can use what we call the enhanced driver. After all, what is a driver beyond knowing how to log in, recognize a prompt, and run a command? If you know those things and which commands you want to run, we can start doing some really interesting work.


If the Advanced Driver’s requirement is *tell us what it is and how to authenticate* , then the Enhanced Driver’s is *tell us how to log in/out and what the prompt looks like* . The goal of the Enhanced Driver is to get the system to the point where it can run arbitrary commands, even commands that back up the configuration or check the voltage on an outlet or check a certificate’s expiration date. You could essentially build an advanced driver using the Enhanced Driver and a suite of rules and rulesets. We’ve configured Enhanced Drivers for many devices over the years, so check out our *Configuration Guides* section of the Local Manager User Guide for examples.


## Finish Line


We would love to make monitoring, backup, and automation a breeze for every make and model of every system out there, but in the meantime, we can still leverage the Local Manager’s unique position and build custom automation for devices we don’t have advanced drivers for yet. And since you’re using an Lantronix product to do it, you’ll have full access to our Technical Support and Professional Services teams who will help you every step of the way.


We’ve said it a thousand times, and we’ll say it a million more: getting you access to remote equipment is just the beginning.
