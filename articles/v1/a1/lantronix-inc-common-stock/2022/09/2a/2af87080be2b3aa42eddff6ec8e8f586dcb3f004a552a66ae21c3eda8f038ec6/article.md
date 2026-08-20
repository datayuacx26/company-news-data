---
schema_version: "1.0.0"
document_id: "2af87080be2b3aa42eddff6ec8e8f586dcb3f004a552a66ae21c3eda8f038ec6"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/feature-spotlight-power-control/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:77b2569442080d729f6777d9f30c0d7f0aa965a5087ce985841b74a4cc8fd911"
---

# Feature Spotlight: Power Control

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### Feature Spotlight: Power Control


Do you think it was Thomas Edison or Nikola Tesla who first said **have you tried turning it off and back on again?** Did they know in ancient times that one of the most tried and true troubleshooting techniques of the 21st century would be to simply cut power and restore it? I like to think **yes, of course they did** . And now here we are, thousands of years later, talking about one of the most basic but also one of the most useful features of the Lantronix LM-Series Console Server, namely, the ability to automatically control a managed power controller. That means we can *leverage* managed PDUs to *synergize* a new *paradigm* in remote network management!


That’s right. No more rolling trucks to flip a switch. No more calling Earl in the middle of the night to run down to the datacenter to power cycle a router. No more getting out of your chair to walk into the lab.


Now you can do it all from the LM-Series.


By the power of Greyskull, you have the power.


# Smart Power


Let’s be clear. We’re not talking about simple power strips like you find on the shelf at Target. These things are *smart* , with embedded interfaces that know which outlets are on and how much voltage is being used. They often come with serial ports for direct, out-of-band management, but newer models also support TCP connections and have web interfaces.


We have Advanced Drivers available for several PDU brands, which gives the LM-Series the ability to log into the CLI, check the status of each outlet, see how much current they are drawing, and turn the outlets on and off as needed. And since we’re directly connected over the serial port, we can still control power at the site even if the network goes down.


## Show Your Routers Who’s Boss


You know the routers I’m talking about. They like to throw errors on their interfaces. Sometimes they pretend to be out of memory. And occasionally, usually when you need them the most, they’ll just stop talking on their console ports. They think the silent treatment will save them, since you’re in Austin and they’re down ‘Pulco. Little do they know that you’ve got a fully integrated power controller at the site, so giving that router a little kick in the pants is as easy as changing to the power controller port and issuing **off** and **on** commands.


```text
[super@LantronixLM (port1/8)]# off A1  Powering off port1/8 outlet(s) [A1]  [super@LantronixLM (port1/8)]# on A1  Powering on port1/8 outlet(s) [A1]
```


When you issue the **off** and **on** commands, the LM-Series takes care of logging into the PDU to issue the equivalent commands. So, no matter which brand of power controller you use, the interface on the Lantronix side will always be the same.


## Map it Out


Unless you set up the site yourself and have the world’s most perfect memory, you’re probably not going to remember which device is plugged into which outlet on the PDU. That’s where outlet mapping comes into play, and you’re prompted to do it immediately after initializing the power controller port.


Outlet mapping means telling the LM-Series that outlets A1 and A2 (the outlet names vary across manufacturers!) are associated with port 1/1. This one piece of knowledge not only allows you to confidently issue power commands, but it also opens the door to some awesome Lantronix LM-Series automation.


```text
[super@LantronixLM (port1/8)]# show outlets  No outlets are mapped to this device.  Outlet A1 goes to interface port1/1  Outlet A2 goes to interface port1/2
```


Because really… typing **power off** and **power on** is a lot of work.


Let’s see if we can simplify the human contribution. (Spoiler: we can)


## Automated Power Cycling


When a router or switch is giving you trouble and the command line is not responding, you flip the power switch on the back, wait a few seconds, and then flip it back on. Then what do you do? You watch the POST. You make sure that router came back up the way it was supposed to. Then, once you have a login prompt, you get back to work.


Wouldn’t it be a shame if all the LM-Series did was cut and reapply power ( *he asked, fully aware the LM-Series does much more than that* )?


Since we’re directly connected to the router, we can watch the output on the console port both before and after the power cycle. That means we look at the electrical signals on the port to let you know the status of CTS and DSR. Then we cycle the power. As POST begins, we capture that information and store it in our local port directory for later reference. If there are any problems booting, such as a loss of startup config, we’ll handle that by pushing down our last known-good version. Once the router has fully booted and we’re able to start monitoring, we’ll report success.


The interface looks like this:


```text
[super@LantronixLM (port1/1)]# power cycle  1/8: Powering off port1/8 outlet(s) [A1]  1/8: CTS was active.  1/1: Reading post  1/8: Powering on port1/8 outlet(s) [A1]  1/1: done Initializing Flash  1/1: Image decompressed  .1/1: Started  1/1: Post complete.  Serial link (CTS) is active.  getting device uptime  Hostname : AUS-CORE  Serial Number: FOC1432X2N5  Make : cisco  Model : WS-C2960G-24TC-L  OS Type : IOS  OS Version : 15.0(2)SE11  Uptime : 1 minute  System image file is "flash:/c2960-lanbasek9-mz.150-2.SE11.bin"  start time was : Tue May 25 15:58:55 UTC 2021  start time is now : Tue May 25 16:02:27 UTC 2021
```


As you can see, the LM-Series keeps tabs on everything. To you and I, the term *power cycle* means a flipping of a switch. To the LM-Series, it means carefully managing a power loss scenario and ensuring the device is running as expected.


## Keeping Communication Open


One of the best qualities of the Lantronix LM-Series is that it adjusts to your comfort level. We can sit quietly and watch, or we can alert, or we can take pre-approved actions. We can even take the last-resort action of power cycling a router that stops responding on its console port. If that sends chills down your spine, don’t fret—you have to *very* explicitly enable that feature.


Automatic Console Recovery is a good example of what the LM-Series can do when it controls both the console and the power. The status of one informs the other. That means we can watch the console port, see that CTS and DSR are still high (link light active), but the router just isn’t responding. We’ll cut power, reapply, watch the POST, and verify everything comes back to a nominal state.


If you have a router that locks up randomly in the middle of the night, Console Recovery could be just the feature you’re looking for.


## Automatic Password Recovery


I love this feature. Mostly because I never remember the exact sequence for performing a password recovery on a Cisco router. Or maybe it’s the config register that I get hung up on. Either way, I don’t have to worry about it anymore. If I find myself locked out of a router (or if the LM-Series is locked out too), then all I have to do is run the **recover configuration** command. The LM-Series knows what the passwords *should* be, in that they are defined by the last known-good startup config. All it needs to do is push down that config.


But… if it can’t log into the router, how can it push down the config?


Easy. Cut power. Send a break signal at exactly the right moment. Change a config register. Boot to empty config. Push down startup. Reload. Change config register back. Boot. Verify startup config. Resume monitoring.


```text
[super@LantronixLM (port1/2)]# recover configuration  This process can take about 5 minutes.  Attempting to revert startup configuration to current from 25 May 14:52  1/8: Powering off port1/8 outlet(s) [A2]  1/8: DSR was active.  1/8: CTS was active.  1/8: Powering on port1/8 outlet(s) [A2]  Attempting to break into ROMmon mode.  Break into ROMmon successful.  Reading post  Image decompressing  Image decompressed  Post complete.  Transferring file ...  Sent startup-config (452 bytes) at 594 B/s.  Issuing 'reload'  Reading post  Bootstrap posted  Image decompressing  Image decompressed  Started  Post complete.   Trying ping to check that network is working  pinging 10.10.10.1 was successful, round trip time was 4.0 ms  testing credentials.  Succeeded.
```


And what will *YOU* do while that is happening? Whatever you want. Go get some coffee. See if there are any donuts left in the kitchen. When you get back, that router will be running the correct configuration with all of its expected usernames/passwords back in place.


It really is just that simple, but it wouldn’t be possible without a managed power controller.


## Like a Light Switch


I recommend managed power to every customer for every site. Even if you have personnel at the site who can just run over to the lab or data closet and flip a switch, in a network-down situation, **every second counts** . There is no time to wait for Earl to finish his hand of solitaire. We need to get on the road to Network Good as quickly as possible.


You can do that with an LM-Series console server. Or it can do it for you.
