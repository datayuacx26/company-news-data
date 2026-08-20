---
schema_version: "1.0.0"
document_id: "9047cfe6a173a26be237f1232128089c583c0b519051b2a4412da7167f14d771"
company_key: "intrusion-inc-common-stock"
company: "Intrusion Inc."
source_id: "intrusion-inc-common-stock-rss-206824b90ed5"
canonical_url: "https://intrusion.com/blog/the-forgotten-defense-against-ransomware/"
published_at: "2025-04-15T21:44:03+00:00"
first_seen_at: "2026-07-28T22:10:21.872433+00:00"
fetched_at: "2026-07-28T22:25:52.186224+00:00"
content_hash: "sha256:2befad01c2cd7a6ccf852af2a6c0388d36be24b65366953509ceedaa1a2941af"
---

# The Forgotten Defense Against Ransomware

Ransomware stands out as one of the most devastating forms of attack capable of crippling organizations, causing significant financial damage and lost time. Traditional security measures alone are often insufficient to counter these evolving threats. Today, we're going to explore how leveraging threat reputation, an approach that preemptively assesses and deters threats based on historical behavior and attributes, can provide a robust deterrent against ransomware.


#### Ransomware is a business


The fact that cybersecurity researchers are mapping ransomware actors on a Gartner like magic quadrant based on their vision and ability to execute shows the maturity of that business model.


Ransomware-as-a-service is a business model where ransomware developers lease their malicious software and access to other cyber criminals who then use it to conduct an attacks and extort ransom.


It's not about revenge or intimidation, ideology, or showcasing hacker skills. The primary motivation behind the attacks is financial gain, and the victims are bystanders that happen to let their guard down.


Something to remember—the threat actors are also engineers and programmers, and they follow the same continuous integration and continuous delivery principles in their development. But they're faster and they're not constrained the way we are. In simple terms, the threat actors are highly skilled, agile, and unrestricted, making them formidable opponents.


#### Every business is a target for ransomware


Most ransomware seeks victims of opportunity rather than targeted attacks. So these cyber criminals cast a very wide net, exploiting common technical and social weaknesses. Their primary goal is to infect as many companies and systems as possible to maximize their profits.


They're going to go after the lowest hanging fruit, and they're going to go after the most lucrative client that they can infiltrate the quickest with the largest payback.


So unlike targeted attacks, which require extensive planning and opportunities, ransomware attackers are just trying to exploit any accessible weakness, making them a threat to all organizations of all sizes. So small to medium sized businesses are just at risk, if not more than large companies.


#### The rise of the unknown attack vector


A few years ago, the majority of ransomware attacks originated from phishing emails. But now ubiquitous security training and email filtering solutions helped drive down that vector.


The use of generative AI to craft more advanced and convincing phishing emails may seem like a resurgence, but so far it hasn't had a significant impact.


**Nearly half of reported ransomware attacks have unknown attack vectors.** Meaning the security researchers or analysts investigating the attack are unable to determine how the attackers initially gained access to the system or network.


This could be due to things like:


- Lack of visibility or logging capabilities
- Unusual or novel exploitation techniques
- Inadequate security measures or misconfigurations
- Insufficient information or evidence


This is troubling news and highlights both the limitations of today's security solutions and the capabilities of organizations to clearly identify the threat.


#### The ransomware lifecycle


Often we see that it's not just one actor that takes from a compromise to ransomware, but rather the work of a complex arrangement of suppliers and consumers that serve different roles in this enterprise.


The left two stages, the reconnaissance and initial access stage, are typically performed by initial access brokers. These are malicious actors who specialize in gaining access to networks.


Their tools involve the infiltration of a network using phishing, brute forcing, credential stuffing, zero day vulnerabilities, and social engineering. The use of AI and automated tools further allow them to cast a wider net spanning large audiences for that big catch.


Once the initial access has been obtained, control is often handed over to a ransomware group. They specialize in the lateral spread of that infection, surveying the target and deploying ransomware.


At this point, they assess who their victim is, what they're worth, whether they have backups or not, which all help set that ransom demand value.


The good news is that many of these activities rely on network communications, which gives us an opportunity to intercept this behavior and prevent the spread of ransomware.


#### Why static block lists don't work


Most threat intelligence is reactive. That is an attack is detected maybe because it's sandboxed or discovered by a researcher. And then that knowledge is propagated to endpoints and firewalls via block lists.


This is effective for a long running, expansive malware campaigns that reuse the same infrastructure.[The average lifespan of a malware command and control server is less than one hour.](https://gridinsoft.com/command-and-control) Attackers also use unique signatures to attack each targets and tactics like fast flux, which constantly change the attacker's infrastructure. This creates challenges when trying to react because it becomes a game of whack-a-mole attempting to block this ever changing list of IP connections.


To combat this, we need to focus on a more proactive, preemptive solution.


#### Why baselining a network isn't effective


Can't you just determine what 'normal' traffic looks like on a network and flag everything else as suspicious?


It's a great idea, in theory, but the problem is that a typical organization's traffic is incredibly diverse. Millions of domains and IPs are requested every month for everything from web browsing to APIs and vendor communications.


To illustrate this, we logged network traffic (DNS requests and IP communications) in a medium-sized organization over the course of 3 months and took note of the overlap from month to month.


On average, there were about 800,000 unique domain requests each month, but only 42% of these overlapped from month to month. And over a 3 month period, there was only a 30% overlap.


We found the same to be true with IPs. We looked at unique destination server IPs and outbound TCP communications. Around 1-2 million distinct server IPs were observed per month, but only a 25% overlap from month to month.


In summary: the internet is a moving target.


#### It's not as simple as "good" or "bad"


The image below illustrates the scale of reputation from bad to good. When you see sites like those on the right—Google, Facebook, and Twitter—in your network traffic, you know exactly what they are and probably why they were requested.


In contrast, on the left, there are known malicious sites that have a history of suspicious behavior that we learned from threat intelligence feeds and security research. This is kind of a simplistic example, but when you see sites that have characters meant to deceive a user—like throwing zeros in Google and threes in Facebook and dashes in Twitter— you know that that site is probably up to no good. These are known as look-alike-sites or homoglyphs.


These sites are clearly bad news.


But what about all the stuff in the middle? The vast majority of domains we see are neither the known good or the known bad, but hundreds of millions of sites in this gray area in between that really lack any threat intelligence, good or bad. These may be newly registered domains or even sites which have existed for a long time, but with no content and no legitimate purpose.


Determining what action to take is a less of a binary good versus bad choice and more of a calculation in risk tolerance.


#### The importance of IP and domain reputation


You need a lot of information and intelligence to make real-time decisions on whether the incoming and outgoing traffic is good or bad.


You can protect your network from these risky and suspicious domains and IPs with Intrusion. Backed by 30 years of internet reputation, threat intelligence, and flow analysis, you can be confident in knowing your network is protected from call-homes and other attempted outbound malicious communications.


IP and domain reputation can often be overlooked or considered too basic to protect from advanced attacks, but we've found the opposite to be true. IP and domain reputation is a proactive approach and can help your business identify and anticipate potentials threats that no other solutions notice.


You can dismantle attempted ransomware attacks with Intrusion.[Book a meeting today and let us show you](https://intrusion.com/book-a-meeting/) .


### Resources that might interest you.


[Blog](https://intrusion.com/resource-center/category/blog/)


### [Top 10 Cloud Security Threats](https://intrusion.com/blog/top-cloud-security-threats/)


[Blog](https://intrusion.com/resource-center/category/blog/)


### [Why Historical IP Reputation Matters More Than Ever for Cyber Attack Prevention](https://intrusion.com/blog/why-historical-ip-reputation-matters-more-than-ever-for-cyber-attack-prevention/)


[Blog](https://intrusion.com/resource-center/category/blog/)


### [The Practical Guide to OT Security: Protecting Industrial Networks from Cyber Threats](https://intrusion.com/blog/the-practical-guide-to-ot-security-protecting-industrial-networks-from-cyber-threats/)
