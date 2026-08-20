---
schema_version: "1.0.0"
document_id: "950fb13e508bb37a6d87aac030287c83608aee8f7878129d53f3f18d9d54b5bf"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/set-it-and-forget-it-why-the-best-out-of-band-solution-is-the-one-you-dont-have-to-think-about/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:190cb9fb0f64c1d7c0134b061e8b474b1594cd11b2f3048291db0c3c1ef78c39"
---

# Set it and Forget It: Why the Best Out-of-Band Solution is the One You Don’t Have to Think About

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### Set it and Forget It: Why the Best Out-of-Band Solution is the One You Don’t Have to Think About


“


Twenty years ago, out-of-band solutions were just starting to move past the era of plugging a phone line into a Cisco 2811 with octopus cables and hoping it answered when you called it. Today, we not only have more options for out-of-band channels, including cellular, satellite, and secondary Ethernet connections, we also have the ability to manage those channels automatically, bringing them up and down as needed to provide an always-there connection for the user. The end result is a single pane of glass from which the user can click *SSH* and be connected to their Local Manager, regardless of the state of the network. In this post, we’ll look at how Uplogix accomplishes this through monitoring, automated actions, and reporting.


# Checking the Network’s Pulse


From the early days of the Uplogix Local Manager, it became clear that we needed some way to know when the in-band connection went down. As a monitoring platform, we have access to a lot of information that can help us detect an outage, from the line protocol state of an interface on a managed router to the electrical signals on our own management Ethernet port. By triangulating all of this data, we can assess the network’s state and take action if necessary.


As network techs, we all liked that idea, but we had to admit it was a lot of work figuring out the “network down” criteria for each site. We needed a simpler solution, one that could be deployed for 95% of sites and work just fine.


The answer was **Pulse** . When configured, the Local Manager sends an echo packet to port 7 of a designated target. If that target echoes back, we consider the network up. If too many intervals go by without a response, we consider the network down. To ensure we’re always testing the in-band connection, the Local Manager uses a static route to keep that packet always going out the management Ethernet interface rather than send it out the newly established out-of-band connection where it might succeed (and start to flap).


```text
[admin@UplogixLM]# show system pulse  Use Pulse: true  Pulse Server IP 1: 192.168.1.254  Pulse Server Port 1: 7  Enable Out-of-Band on Pulse Failure: yes  Remain Out-of-Band after Pulse Success: no  Maximum Time Out-of-Band: 0 min
```


## Recalculating Route


Once we are able to determine a network outage, the next step is to spin up the out-of-band connection automatically so that we can reestablish communication with the Control Center and make ourselves available via SSH, which includes reporting back our new IP address. This functionality is limited to mobile-originated out-of-band, that is, when the Local Manager establishes the connection rather than a user dialing from a modem (e.g., POTS or Iridium). While there are many considerations when choosing and configuring your out-of-band connection, once it is in place, the switch between in-band and out-of-band should be transparent to a user.


Now that the out-of-band connection is established, the Local Manager’s routing table is updated to send all traffic through the new path, with the exception of Pulse, which is still tied to the management Ethernet interface via a static route. Whether through a VPN, private APN, or simply an open port on a DMZ router, the Local Manager then resumes communication with the Control Center. In cases where the Local Manager gets an IP in a private network where its connectivity is NATed, it can automatically establish a Reverse SSH tunnel to the Control Center, thereby allowing users to proxy through the Control Center and back down the SSH tunnel.


The primary goal is to give the user the same kind of access they had when the network was up. However, establishing the out-of-band connection is only half of the solution. After all, what’s the point of going out-of-band if you don’t know where on the network the Local Manager ended up?


## Forwarding Address


During normal operation, the Local Manager is heartbeating with the Control Center every thirty seconds. This allows the Local Manager to synchronize users, back up config files, and report alarms, but mostly it’s just trading system information and keeping the Local Manager Summary page up to date.


The first line of the Summary page shows you the most important information: status of the LM (using a color-coded Uplogix logo), the hostname, and its current IP address. From the example above, you can see at-a-glance that UplogixLM is in-band and heartbeating normally.


When operating out-of-band, the color of the Uplogix logo will change to orange and the IP will update to reflect the new out-of-band connection, whether that’s a public IP or a private, VPN-assigned internal address.


Although the new IP is displayed to the user, there is nothing they need to do to use it. Clicking on the SSH button works exactly the same when out-of-band as it does when in-band. The applet or terminal application is smart enough to know that it should use the IP that the Local Manager is reporting *when the button is clicked* . The same would be true if the Local Manager has established a Reverse SSH tunnel. Ultimately, **the connection path is transparent to the user** . Despite all the complicated things happening in the background, we found that our users just wanted to be able to click a button and establish the SSH connection. How we provide that SSH connection is up to the Local Manager to… um… *manage* .


## That’s the Way I Like It


Twenty years ago, a network outage meant consulting a list of terminal servers, finding the device you wanted to manage, and opening a telnet connection to an IP address on a specific port. Sometimes the terminal server wouldn’t answer. Sometimes a coworker had changed the VTY password without telling anyone. And sometimes it just plain didn’t work. Having to troubleshoot your terminal server is never fun, but during a network outage? Forget about it. You might as well roll trucks.


Today, our terminal servers have evolved into network management platforms. They can detect a network outage, establish an out-of-band connection, and report back their new address, all before you even get the first email alerting you to a problem. And when you go to use the Local Manager, all you have to do is click the SSH button; we’ll take care of the rest.


When we say **Uplogix** **takes you beyond out-of-band** , we don’t just mean technologies like 5G cellular and Iridium satellite modems. We want to change the way you think about out-of-band, or more accurately, we want to make it so you don’t have to think about it.


It’s just there when you need it.


—


Want to talk more about this topic? Have a question for the mail bag? Shoot me a note at[\[email protected\]](https://www.lantronix.com/cdn-cgi/l/email-protection) and let’s discuss!


“
