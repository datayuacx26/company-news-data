---
schema_version: "1.0.0"
document_id: "83da94a8e20de4d34c0e8abfdbf444cb7d5fc9d2f7d478c373308cb2f47a78de"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/cellular-out-of-band-4-things-to-consider/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:c8b363a6cef04c8895b166c00ebe4fc325df0ea4427f60e0b63f5ff51299027a"
---

# Cellular out-of-band: 4 things to consider

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### Cellular out-of-band: 4 things to consider


Lantronix has been a leader in cellular out-of-band management systems for over a decade. When it comes to running out-of-band over cellular, we’ve literally been there and done that — experiences that have led us to know the challenges and pitfalls of deploying and running cellular out-of-band. Here are four considerations to keep in mind if you are looking to deploy cellular or upgrade your existing solution.


## **Coverage**


First, you must think about coverage. At a basic level, is there cellular service at the site? What about when deploying across multiple sites? Will you need more than one provider? What about international sites? If you are going global, you’ll almost certainly have to work with different providers.


Even once you have the provider questions answered, you’ll need to determine what cellular options you have at your locations. The cellular industry is fraught with a mix of well-known marketing names, lesser-known standards and legacy technologies. 5G, 4G, LTE, EVDO, CDMA, HPSA+, GPRS, GSM… it’s helpful to have a partner like Lantronix that can help you cut through the noise to deploy the right cellular modems to be successful in all your locations.


Sometimes, coverage is available, but challenging. At Lantronix we’ve helped customers achieve viable signal strengths by deploying secondary high gain antennas, or by placing a directional YAGI antenna oriented to the heading of a local cell tower. We also offer a CAT M1 modem option. These were developed for IoT applications and M2M links where connectivity and reliability are more critical than high speeds. Think data centers and basement MDFs.


What about if you really have no connectivity options? No problem. Solutions include just about any option you might have for a secondary network connection: a v.92 phone line, a fiber link to a secondary fiber optic network, maybe an ISDN modem… If you are in the middle of an ocean or anywhere else on the planet, Lantronix can connect through an Iridium satellite link. It’s something we do all the time for oil & gas customers. For most, network locations just aren’t that exotic and cellular is a great option.


## **Connectivity**


Once you know there is coverage at your sites and know what types of modems you’ll need, the next consideration is how you’ll connect to the cellular network. Data connections aren’t the same as the connection you make for a call. First, you’ll need to know what kind of APNs (Access Point Names) you’ll be connecting to. An APN is an IP gateway between the mobile network and another computer network and can be delivered by the provider or set up as a private APN.


So now that your modem has IP there are more questions. Do you want the IP address to be dynamic (DHCP) or fixed? Should it be a public IP address or private? There are pros and cons for each. How you use cellular out-of-band might have an impact on what you choose. Will your cellular connection be always-on? Or will it come up when the primary network fails? Maybe you only want the link available when you trigger it to come up through something like an SMS message?


At Lantronix we’re fans of bringing up cellular as needed. It greatly reduces your chances of attack if the link doesn’t exist for most of the time. One method Lantronix recommends uses to avoid knee-jerk responses to network issues is called pulse. You can set up to three pulse servers that Lantronix will send an echo request to when the primary network fails. After no echo response for three attempts over about 90 seconds, Lantronix will open the out-of-band connection. When the primary network has been restored, Lantronix will tear down the cellular out-of-band connection after five minutes of successful echo requests.


When it comes to private or public IP addressing, it’s your choice. With a private IP address, you VPN back into your network or use a private APN. With a public address, you can connect with a local password, or use our preferred method where the Lantronix appliance will create a reverse SSH tunnel through the cellular network to a NAT-ed address for your Lantronix Control Center through your firewall to the Control Center itself. Users authenticate into the Control Center using your preferred AAA and have secure communications to the remote site through the RSSH tunnel. When they are done working, the tunnel is torn down and the cellular connection closed.


*Creating a Reverse SSH Tunnel from the Uplogix Appliance Back to the Uplogix Control Center Provides a Secure Link Between Sites Over the Cellular Network.*


## **Buying a Plan**


Most of us have had the pleasure (NOT!) of contacting a cellular provider. When it comes to buying a business cellular data plan, other providers will try to sell you the world, often looking to include high data volumes and speeds and maybe even an entirely separate network for out-of-band. This really isn’t all necessary. What you need is a data plan akin to what you’d buy for an iPad. It should include SMS, as it’s a useful way to initiate out-of-band or use in automated alerting. Data between sites can be shared and the small volume of network management traffic means that a plan of 10GB/month should support up to 100 sites managed by Lantronix.


With[Lantronix Connectivity Services](https://www.lantronix.com/connectivity-services/) we can provide secure out-of-band connections using our cellular platform to simplify connecting all your sites worldwide. Our easy-to-use cellular management platform provides built-in analytics to help you uncover SIM usage and key trends. Run the extensive diagnostic features to help troubleshoot any issues that may arise.


## **Most important of all: Reliability**


While an out-of-band platform like Lantronix provides value when the network is up and running, its key responsibility is to be there when you need it. When it hits the fan. Historically out-of-band solutions weren’t very reliable due to human factors:


- Someone unplugged a modem line to connect something else, say a fax machine. The network goes down and a remote IT admin tries to dial-in to the remote site only to encounter a friendly fax machine trying to receive.
- Maybe the console connection to a device was unplugged for an onsite admin to push a change from their laptop last month, but it didn’t get plugged back in, or plugged into the right port. Again, this kind of error could sit unknown until that link is needed.
- It’s not always someone with access to the networking closet that can impact reliability. Say someone in finance is auditing cellular bills and comes across a plan that doesn’t seem quite like the others. They inquire with the sales team, find no answers and promptly shut down the plan to save money, leaving a timebomb ticking until the next network outage when cellular out-of-band just doesn’t work…


These are all scenarios that we’ve run across at Lantronix and have built in safeguards to ensure your cellular out-of-band solution is there when you need it. One example is that Lantronix appliances can automatically test the out-of-band connection as often as you’d like, and then alert when there are issues so you can deal with it on a day when the network is up and running just fine. Same for console connections. Automated monitoring of managed devices ensures that if you lose a console connection, you’ll know it.


When it comes to troubleshooting cellular problems, Lantronix has your back. Cellular modems use cryptic commands to initiate as well as transfer all kinds of useful information about the modem and cellular network. The challenge is they aren’t easy to interpret and are easy to mess up. Lantronix has basic commands to send appropriate AT commands to the cellular modem and then parse out the return information into an easily human-readable format that makes it easier to troubleshoot issues. Specific values like modem type and carrier, as well as qualitative values like signal strength give you the information needed to ensure the best cellular out-of-band connection.


## **Cellular is great when it works**


Cellular makes a great out-of-band option, but like anything, has its own quirks. Partnering with a company like Lantronix that has been in the trenches of cellular networks for more than a decade and has developed a solution designed to make out-of-band management easier to deploy and more effective is a great first step. We can walk you through each of the considerations we’ve covered here and find the right solution for your network.
