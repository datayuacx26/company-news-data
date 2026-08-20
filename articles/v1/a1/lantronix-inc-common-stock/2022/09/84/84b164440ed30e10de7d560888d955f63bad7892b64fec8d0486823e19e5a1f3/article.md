---
schema_version: "1.0.0"
document_id: "84b164440ed30e10de7d560888d955f63bad7892b64fec8d0486823e19e5a1f3"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/using-port-forwarding-to-route-the-unroutable/"
published_at: "2022-09-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:70cd53b42c4648023ff34b2482e20f7ccf92528dc4b6350aeefa0c189e712b2e"
---

# Using Port Forwarding to Route the Unroutable

September 10, 2022


- [Former Uplogix Blog Archive](https://www.lantronix.com/blog/category/former-uplogix-blog-archive/)


### Using Port Forwarding to Route the Unroutable


On November 25, 2019, the regional Internet registry for Europe announced that it had made its “final /22 IPv4 allocation from the last remaining addresses in our available pool” and that “\[w\]e have now run out of IPv4 addresses.” This wasn’t surprising; smart people had been working on the problem for years, which is how we got IPv6. But, there was a while when the answer to IPv4 exhaustion was to simply create private, unroutable networks and place them behind a router doing Network Address Translation (NAT). This is essentially how most home networks work, but in an enterprise environment, NAT can end up stranding important services behind the router. Exposing those services can be cumbersome and insecure, which is why Lantronix came up with a better way.


We call it Port Forwarding, and it’s built into every out-of-band management Local Manager.


## Prime Position


The Lantronix Local Manager (LM) always seems to be in the right place at the right time. In this case, it’s sitting in the private network with your inaccessible web server (though it could be an HP ILO or any other network-based service). We don’t allow outbound SSH connections from the LM for security reasons, so our only option is to use a reverse tunnel through your inbound SSH connection. This works regardless of whether the connection is in-band, out-of-band, or through our Reverse SSH feature. However you connect, we can take a remote port and forward it securely to a local port on your workstation.


In the diagram above, the Local Manager is located in a private network with an HP ILO device that we want to access via Port Forwarding. A NAT prevents us from accessing the HP ILO directly, so we’re going to forward the connection through your SSH connection. The target server can be on the same management network as the LM (option A) and use a virtual port to connect, or it can be connected directly to a Dedicated Ethernet port (option B). This is only a configuration choice, as the Port Forwarding feature itself works the same with either option.


## Making the Connection


You can read the[full documentation for Port Forwarding here](https://uplogix.com/docs/6.1/control-center-user-guide/applet/ssh-port-forwarding) , but it’s as simple as telling the Local Manager which IP and port to forward. Then, when you load the Terminal Applet and click Terminal > Forward, you’ll see the forward listed next to the associated port. Clicking the button will automatically launch another browser window.


In the above example, the target server would be on the same management LAN as the Local Manager. The management IP address of the device on port 2/3 has been set to 172.30.4.225, and under config protocols forward, the port has been configured to forward to the management IP (172.30.4.225) on port 80. Once the button with the globe icon is clicked, the local port turns green and the number goes from random to 80.


Depending on your environment and what you’re trying to connect to, we may need to adjust the settings, but Lantronix Support is standing by 24/7 to do just that.


## Why Not Just Forward Ports on the NAT?


Good question, savvy network person. It’s true, you can get around the limitations of NAT by exposing ports and doing forwards and modifying your firewall and taking on all that extra work. People have been doing it that way for years, and who are we to stand in the way of tradition?


Oh, that’s right. We’re Lantronix. We take you beyond tradition(al out-of-band).


Here are some reasons you may want to use port forwarding:


- Access network services that are hidden behind a NAT or firewall, or otherwise cut off from the network.
- Front-end web interfaces or even SSH connections by forcing all traffic through the Local Manager, which requires users authenticate based on your security policies.
- Use third-party software that requires a network connection by forwarding the port and pointing the software at localhost.
- Secure servers by limiting network access to the Local Manager (as in, only the LM can connect to it).


## Connect to Everything


Port Forwarding works with both network services and the actual console ports on your managed devices. If the target server isn’t directly connected to the Local Manager somehow, we can use virtual ports to bridge that gap. Having the LM at the site is like putting me in front of a buffet; if I can get my hands on it, I’m going to eat it. Likewise, if the Local Manager can connect locally inside that private network, it can forward that connection back to you. We can get you access to pretty much anything, and we can do it securely regardless of whether the network is up or down.


For more examples of how Port Forwarding can be used, check out the Configuration Guides in our documentation library, specifically:


- [C-Com Antenna](https://uplogix.com/docs/6.1/local-manager-user-guide/configuration-guides/c-com-antenna)
- [Checkpoint Firewall](https://uplogix.com/docs/6.1/local-manager-user-guide/configuration-guides/checkpoint-firewall)
- [Intellian Antenna Controller Unit](https://uplogix.com/docs/6.1/local-manager-user-guide/configuration-guides/intellian-antenna-controller-unit)
- [KG Encryptor](https://uplogix.com/docs/6.1/local-manager-user-guide/configuration-guides/kg-encryptor)


For a deeper dive into Port Forwarding,[drop us a note](https://www.lantronix.com/to-buy/contact-me/) and we’ll put something on the calendar. Until then: hoard your IPv4 addresses; they will be worth their weight in gold in 2031, mark my words!
