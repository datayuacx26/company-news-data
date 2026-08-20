---
schema_version: "1.0.0"
document_id: "721766a17d88b764f6010ad1855a0e92c4f34a1c499a1a45c219d001d488eacc"
company_key: "a10-networks-inc-common-stock"
company: "A10 Networks Inc."
source_id: "a10-networks-inc-common-stock-rss-284845abe605"
canonical_url: "https://www.a10networks.com/blog/5-most-famous-ddos-attacks/"
published_at: "2026-05-12T18:00:00+00:00"
first_seen_at: "2026-07-23T23:21:56.777318+00:00"
fetched_at: "2026-07-28T22:13:25.865061+00:00"
content_hash: "sha256:e45459d071ff0b65b4cfa590d2b8af7922644656f0fb74869f6aa43ad5afc10d"
---

# The Most Famous DDoS Attacks in History

Distributed denial of service (DDoS) attacks are an everyday occurrence. Traditional network-layer DDoS attacks can still threaten the infrastructure that provides network and service availability to all its tenants, subscribers, and customers, but due to large advances in the ability of defenses at Layer 3/4 to absorb network floods, DDoS attacks have been “shifting upwards,” targeting Layer 7 instead.


With more public-facing apps, APIs, and microservices, Layer 7 DDoS attacks have become an easy-to-execute, cost-effective, and difficult-to-detect alternative to the traditional network-layer DDoS attacks. Whether a small non-profit or a huge multinational conglomerate, the online services of the organization—email, websites, anything that faces the internet—can be slowed or completely stopped by a[DDoS attack](https://www.a10networks.com/glossary/what-is-a-ddos-attack/) .


Despite the longevity of DDoS attacks, DDoS is still the number-one threat incident because of its efficacy and relative ease of execution. A successful DDoS attack can seriously damage brand reputation and cost hundreds of thousands or even millions of dollars in revenue. Moreover, DDoS attacks are sometimes used to distract cybersecurity operations while other criminal activity, such as data theft or network infiltration, is underway. With the introduction of L7 DDoS attacks, comes a revitalized relevancy of DDoS.


The number of active DDoS weapons has hovered around 15.1 million for the past several years. Further insights gained via[A10 Defend Threat Control](https://www.a10networks.com/products/a10-defend-threat-control/) can be viewed in the[2025 Global DDoS Weapons Report](https://www.a10networks.com/resources/reports/ddos-weapons-report/) .


## Key Takeaways


- Traditional DDoS attacks are now hyper-volumetric, with the largest DDoS attack on record peaking at 7.3 Tbps (Cloudflare, 2025), ultimately sending ~37 terabytes of data toward a single target.
- The 2016 Mirai botnet was the first to weaponize IoT devices at scale, marking a turning point in DDoS attack methodology.
- Memcached amplification (used in the GitHub attack) can amplify traffic up to 51,200x, making it one of the most dangerous DDoS vectors ever recorded.
- AI-enhanced L7 DDoS attacks have risen in popularity, with an increased intent to target APIs. Such attacks are inherently smaller in size, and much more difficult to detect because of the way malicious behavior can be masked.


## DDoS Attacks Getting Bigger, More Frequent


During the past few years, we have seen a surge in notable DDoS attacks causing significant disruptions in various industries and costing organizations millions of dollars. Organization leaders don’t want to be told they are under a DDoS attack. They want to be prepared and stay ahead of potential threats to minimize system downtime and safeguard brand reputation.


One of the biggest factors in 2020 DDoS attacks was the COVID-19 lockdown, which drove a rapid shift to online for everything from education and healthcare to consumer shopping and office work, giving hackers more targets than ever before. Because of the haste of this transition, many of these businesses and workers turned out to be significantly under protected from attacks due to the difficulty of maintaining cybersecurity best practices in an emergency scenario.


In 2021, the scale of these attacks hit record highs. In November 2021, Microsoft mitigated a DDoS attack targeting an Azure customer with a throughput of 3.45 Tbps and a packet rate of 340 million PPS – believed to be the[largest DDoS attack ever recorded](https://azure.microsoft.com/en-us/blog/azure-ddos-protection-2021-q3-and-q4-ddos-attack-trends/) . In 2021, we also saw the[increased use of DDoS to demand ransom payments](https://www.datacenterknowledge.com/security/denial-service-attacks-expected-get-bigger-nastier) for stopping the attacks — or not launching them in the first place.


Beginning in 2022, Ukraine has been bombarded both physically and digitally, with DDoS attacks paving the way for other data breaches, critical infrastructure downtime, or espionage activity.


In late 2023, a new DDoS attack with record levels was executed. This attack method –[HTTP/2 Rapid Reset Layer](https://www.a10networks.com/blog/protecting-systems-from-the-new-http-2-rapid-reset-vulnerability/) – was a new method of targeting servers that could bypass the traditional methods of[DDoS protection](https://www.a10networks.com/products/a10-defend/) , such as rate limiting or basic blocklists. This was a reminder that the DDoS defense must continue to evolve, and more comprehensive protection is required for DDoS vendors to effectively defend against modern, sophisticated DDoS attacks.


Though not exclusive to 2024, we’ve seen a continuous rise in carpet-bombing attacks. This could be attributed to two reasons. One, thwarting the source of the attack is difficult, and two, identifying intended targets can be difficult. Carpet-bombing attacks spread across a wide range of IP addresses versus certain targets. This means while the overall volume of malicious traffic generated is still the same, and still dangerous, it is more difficult for existing DDoS defenses to detect because typical threshold-based checks won’t flag the traffic. Carpet-bombing attacks also may have a complex network of bots, including the bots themselves (foot soldiers carrying out commands), command and control center (the brains and source of evil), and proxy bots (the middlemen who take initiatives created by C&C). While one can identify the bots executing the attacks, that won’t necessarily help in stopping the overall attack. You must find a way to trace it back to the middlemen, and then ultimately back to the Command & Control servers.


In 2024, one of A10’s[large service provider customers in Eastern Europe](https://www.a10networks.com/blog/carpet-bombing-attacks-highlight-the-need-for-intelligent-and-automated-ddos-protection/) was faced with such an attack and reached out to A10 to learn more about the strange behavior its DDoS equipment was seeing. It wanted to receive further intelligence to dig deeper into adjustments that could be made on their end.


In 2025,[Cloudflare successfully mitigated a 7.3 Tbps DDoS attack](https://blog.cloudflare.com/defending-the-internet-how-cloudflare-blocked-a-monumental-7-3-tbps-ddos/) , which delivered 37.4 TB of traffic in just 45 seconds. While this is an impressive defensive feat, it also explains why attackers are increasingly shifting their focus from Layer 3/4 DDoS attacks to application-layer DDoS attacks. Traditional flood-based attacks have become less effective as network-layer defenses continue to advance. Executing such large attacks requires significant resources, and it might not even be effective, as seen with the Cloudflare attack. In contrast, Layer 7 attacks are more effective, harder to detect, and require fewer resources. In Q1 2025, there was a 358 percent year-over-year increase in Layer 7 DDoS attacks.


Layer 7 DDoS attacks present a larger challenge.


- L7 traffic requires full decryption to inspect, which inherently, is resource-intensive.
- Fringe methods of looking into packets without decrypting, such as TCP progression tracking, may introduce large amounts of false positives, and accuracy is unreliable at best.
- Low-and-slow tactics, such as slow HTTP or attacks that keep a connection open, can consume resources without triggering traditional DDoS prevention.
- Malicious requests often mimic legitimate user behavior, making it difficult to distinguish normal traffic spikes from coordinated DDoS attacks.
- Like network-layer DDoS attacks, Layer 7 attacks can span multiple vectors and act as a smokescreen, distracting from other malicious activities such as API abuse or OWASP Top 10 application attacks.


## The Most Famous DDoS Attacks (for Now)


To provide insight into what these attacks are like “in the wild,” we’re going to look at some of the most notable DDoS attacks to date. Our choices include some DDoS attacks that are famous for their sheer scale, while others are because of their impact and consequences.


### 1. A New Age of DDoS: Hyper-volumetric DDoS Attacks, 2025


In recent years, Google, Cloudflare, and others, have begun experiencing DDoS attacks of record-breaking size, now known as “hyper-volumetric DDoS attacks.” A hyper-volumetric DDoS attack is a gargantuan volumetric DDoS attack. Its goal is still to introduce downtime, and its method is to flood targets using unprecedented levels of traffic.


#### *Why Hyper-volumetric DDoS Attacks Matter*


The scale of hyper-volumetric DDoS attacks is staggering, but volumetric attacks are starting to plateau. The number of resources needed to generate multi-terabit attacks is large, and despite their size, such attacks have not proven to be consistently effective. It is only a matter of time before attackers begin finding alternative methods of executing DDoS attacks efficiently, and that will likely come in the form of L7 DDoS attacks. Be on the lookout for that in our 2026 update.


### 2. Novel DDoS Attack: HTTP/s Rapid Reset Hits Multiple Targets, 2023


In Q3 of 2023, AWS, Google, and Cloudflare all experienced DDoS attacks of record-breaking size from botnets that were significantly smaller than what had previously been seen. This was concerning and pointed to new methods being used. Further investigation revealed that the attack exploited a feature in HTTP/2 that allows for rapid request cancellation using the RST_STREAM frame, which can eventually overwhelm servers by continuously opening and closing streams, leading to resource exhaustion.


#### *Why the HTTP/s Rapid Reset Layer Attack Matters*


It is a reminder that DDoS innovation continues. Interestingly, this style of[DDoS attack](https://www.a10networks.com/glossary/what-is-a-ddos-attack/) is better suited to be stopped with ADC reconfigurations because this[DDoS attack](https://www.a10networks.com/glossary/what-is-a-ddos-attack/) doesn’t rely on overwhelming servers with the number of requests (or packets of data being sent). Instead, it overwhelms by leveraging the number of headers within the packets of data that are being sent. DDoS-specific solutions are not usually configured to examine details of the packet. They are configured to scrutinize other fields that are more indicative of incoming DDoS attacks. An ADC, on the other hand, focuses on looking at the requests themselves, as they need to establish sessions between the clients and servers, so they are more naturally suited to counter this style of DDoS attack. HTTP/2 rapid reset DDoS attacks are a reminder that DDoS is becoming more advanced.


### 3. The Google Attack, 2020


On October 16, 2020,[Google’s Threat Analysis Group (TAG) posted a blog](https://blog.google/threat-analysis-group/how-were-tackling-evolving-online-threats) update concerning how the threats and threat actors are changing their tactics due to the 2020 U.S. election. At the end of the post, the company snuck in a note:


in 2020, our Security Reliability Engineering team measured a record-breaking UDP amplification attack sourced out of several Chinese ISPs (ASNs 4134, 4837, 58453, and 9394), which remains the largest bandwidth attack of which we are aware.


Mounted from three Chinese ISPs, the attack on thousands of Google’s IP addresses lasted for six months and peaked at a breath-taking 2.5Tbps! Damian Menscher, a Security Reliability Engineer at Google,[wrote](https://cloud.google.com/blog/products/identity-security/identifying-and-protecting-against-the-largest-ddos-attacks) :


The attacker used several networks to spoof 167 Mpps (millions of packets per second) to 180,000 exposed CLDAP, DNS, and SMTP servers, which would then send large responses to us. This **demonstrates the volumes a well-resourced attacker can achieve:** This was four times larger than the record-breaking 623 Gbps attack from the Mirai botnet a year earlier.


#### *Why the Google Attack Matters*


The Google attack was a flood attack that sustained itself across a period of six months, rather than a sudden spike in traffic. Additionally, it proved the continued efficacy of amplification attacks, highlighting the danger of possessing unsecure, misconfigured services.


### 4. The AWS DDoS Attack in 2020


[Amazon Web Services](https://aws-shield-tlr.s3.amazonaws.com/2020-Q1_AWS_Shield_TLR.pdf) , the 800-pound gorilla of everything cloud computing, was hit by a gigantic DDoS attack in February 2020. This was the most extreme recent DDoS attack ever and it targeted an unidentified AWS customer using a technique called Connectionless Lightweight Directory Access Protocol (CLDAP) reflection. This technique relies on vulnerable third-party CLDAP servers and amplifies the amount of data sent to the victim’s IP address by 56 to 70 times. The attack lasted for three days and peaked at an astounding 2.3 terabytes per second.


#### *Why the AWS Attack Matters*


While the[disruption caused by the AWS DDoS Attack](https://www.a10networks.com/blog/aws-hit-by-largest-reported-ddos-attack-of-2-3-tbps/) was far less severe than it could have been, the sheer scale of the attack and the implications for AWS hosting customers potentially losing revenue and suffering brand damage are significant.


### 5. The Mirai Krebs and OVH DDoS Attacks in 2016


On September 20, 2016, the blog of cybersecurity expert[Brian Krebs was assaulted by a DDoS attack](https://www.a10networks.com/blog/inside-the-mirai-malware-that-powers-iot-botnets/) in[excess of 620 Gbps](https://krebsonsecurity.com/2016/09/krebsonsecurity-hit-with-record-ddos/) . Krebs’ site had been attacked before. Krebs had recorded 269 DDoS attacks since July 2012, but this attack was almost three times bigger than anything his site or the internet had seen before.


The source of the attack was the Mirai botnet, which, at its peak later that year, consisted of more than 600,000 compromised IoT devices such as IP cameras, home routers, and video players. The Mirai botnet had been discovered in August that same year but the attack on Krebs’ blog was its first big outing.


The next Mirai botnet attack on September 19 targeted one of the largest European hosting providers, OVH, which hosts roughly 18 million applications for over one million clients. This attack was on a single undisclosed OVH customer and was driven by an estimated[145,000 bots, generating a traffic load](https://krebsonsecurity.com/2016/09/the-democratization-of-censorship/) of up to[1.1 terabits per second](https://arstechnica.com/information-technology/2016/09/botnet-of-145k-cameras-reportedly-deliver-internets-biggest-ddos-ever/) . It lasted about seven days. But OVH was not to be the last Mirai botnet victim in 2016.


#### *Why the Mirai Krebs and OVH Attacks Matter*


The Mirai botnet was a significant step up in how powerful a DDoS attack could be. The size and sophistication of the Mirai network was unprecedented as was the scale of the attacks and their focus.


### 6. The Mirai Dyn DDoS Attack in 2016


Before we discuss the third notable Mirai botnet DDoS attack of 2016, there’s one related event that should be mentioned. On September 30, someone claiming to be the author of the Mirai software released the source code on various hacker forums and the Mirai DDoS platform has been replicated and mutated scores of times since.


**Figure 2. A map of internet outages in Europe and North America caused by the Dyn cyberattack October 2, 2016 / Source: DownDetector (CC BY-SA)**


On October 21, 2016, Dyn, a major domain name service (DNS) provider, was assaulted by a one terabit per second traffic flood that then became the new record for a DDoS attack. There’s some evidence that the[DDoS attack may have actually achieved a rate of 1.5 terabits per second](https://arstechnica.com/information-technology/2016/09/botnet-of-145k-cameras-reportedly-deliver-internets-biggest-ddos-ever/) . The traffic tsunami knocked Dyn’s services offline rendering a number of high-profile websites including GitHub, HBO, Twitter, Reddit, PayPal, Netflix, and Airbnb, inaccessible. Kyle York, Dyn’s chief strategy officer, reported, “We observed[10s of millions of discrete IP addresses associated with the Mirai botnet](https://www.zdnet.com/home-and-office/networking/the-dyn-report-what-we-know-so-far-about-the-worlds-biggest-ddos-attack) that were part of the attack.”


#### *Why the Mirai Dyn Attack Matters*


Mirai supports complex, multi-vector attacks that make mitigation difficult. Even though the Mirai botnet was responsible for the biggest assaults up to that time, the most notable thing about the 2016 Mirai attacks was the release of the Mirai source code enabling anyone with modest information technology skills to create a botnet and mount a distributed denial of service attack without much effort.


### 7. The GitHub Attack in 2018


On Feb. 28, 2018, GitHub, a platform for software developers, was hit with a[DDoS attack that clocked in at 1.35 terabits per second](https://www.wired.com/story/github-ddos-memcached/) and lasted for roughly 20 minutes.[According to GitHub](https://thehackernews.com/2018/03/biggest-ddos-attack-github.html) , the traffic was traced back to “[over a thousand different autonomous systems (ASNs) across tens of thousands of unique endpoints](https://www.infosecurity-magazine.com/news/github-35tbps-memcached-ddos/) .”


The following chart shows just how much of a difference there was between normal traffic levels and those of the DDoS attack.


**Figure 3. Chart of the February 2018 DDoS attack on GitHub. Source: Wired**


Even though GitHub was well prepared for a DDoS attack, their defenses were overwhelmed. They simply had no way of knowing that an attack of this scale would be launched. As[GitHub explained in the company’s incident report](https://github.blog/2018-03-01-ddos-incident-report/) : “Over the past year, we have deployed additional transit to our facilities. We’ve more than doubled our transit capacity during that time, which has allowed us to withstand certain volumetric attacks without impact to users … Even still, attacks like this sometimes require the help of partners with larger transit networks to provide blocking and filtering.”


#### *Why the GitHub Attack Matters*


The GitHub DDoS attack was notable for its scale and the fact that the attack was staged by exploiting a standard command of Memcached, a database caching system for speeding up websites and networks. The Memcached DDoS attack technique is particularly effective as it provides an amplification factor – the ratio of the attacker’s request size to the amount of[DDoS attack traffic generated – of up to a staggering 51,200 times.](https://blog.cloudflare.com/memcrashed-major-amplification-attacks-from-port-11211/)


### DDoS Defenses Enter the AI Era


In what’s proved to be another year of record-breaking attacks, service providers defended against multiple DDoS attacks that topped 2.3 Tbps and 2.5 Tbps. Read this IDC report to learn how AI/ML and automation are keys to a rapid-response DDoS attack protection that drives business resilience.


[Learn About AI/ML and Automation in DDoS](https://www.a10networks.com/resources/reports/idc-technology-spotlight-ddos-defenses-enter-the-ai-era/)


## Other Notable Distributed Denial of Service Attacks


### 8. A European Gambling Company, 2021


In February, Akami announced that they had dealt with “[three of the six biggest volumetric DDoS attacks](https://blogs.akamai.com/2021/03/2021-volumetric-ddos-attacks-rising-fast.html) ” the company has ever recorded. The DDoS attacks were attempts at extortion. The hackers launch a DDoS attack the target can’t help but notice and then demand payment not to do it again and at an even greater scale. In this case the threat attack weighed in at 800Gbps.


#### *Why the Gambling Company Attack Matters*


This attack was notable not just for its scale but also for its novelty. The attackers used a previously unseen DDoS attack vector that was based on a networking protocol known as protocol 33, or Datagram Congestion Control Protocol (DCCP). This attack was volumetric and by abusing protocol 33, the exploit was designed to bypass defenses focused on traditional Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) traffic flows.


### 9. Occupy Central, Hong Kong DDoS Attack in 2014


The multi-day[PopVote DDoS attack](https://www.forbes.com/sites/parmyolson/2014/11/20/the-largest-cyber-attack-in-history-has-been-hitting-hong-kong-sites/?sh=3c8865e438f6) was carried out in 2014 and targeted the Hong Kong-based grassroots movement known as[Occupy Central](https://en.wikipedia.org/wiki/2014_Hong_Kong_protests) , which was campaigning for a more democratic voting system.


In response to their activities, attackers sent large amounts of traffic to three of Occupy Central’s web hosting services, as well as two independent sites, PopVote, an online mock election site, and Apple Daily, a news site, neither of which were owned by Occupy Central but openly supported its cause. Presumably, those responsible were reacting to Occupy Central’s pro-democracy message.


The attack barraged the Occupy Central servers with packets disguised as legitimate traffic. It was executed using not one, but five botnets and resulted in peak traffic levels of 500 gigabits per second.


#### *Why the Occupy Central Attack Matters*


Although, it was reported that the[attackers were probably connected to the Chinese government](https://www.vice.com/en/article/539wnz/inside-the-unending-cyber-siege-of-hong-kong) , there has never been conclusive proof and, perversely, the attack could have been intended to make the Chinese government look bad. The attack may have also provided cover for hackers who managed to extract Occupy Central staff details from a database to mount an extensive subsequent phishing campaign.


### 10. The CloudFlare DDoS Attack in 2014


In 2014, CloudFlare, a cybersecurity provider and content delivery network, was slammed by a DDoS attack estimated at approximately 400 gigabits per second of traffic. The attack, directed at a single CloudFlare customer and targeted on servers in Europe, was launched using a vulnerability in the Network Time Protocol (NTP) protocol, which is used to ensure computer clocks are accurate. Even though the attack was directed at just one of CloudFlare’s customers, it was so powerful it significantly degraded CloudFlare’s own network.


#### *Why the CloudFlare Attack Matters*


This attack illustrates a technique where attackers use spoofed source addresses to send fake NTP server responses to the attack target’s servers. This type of attack is known as a “reflection attack,” since the attacker is able to “bounce” bogus requests off of the NTP server, while hiding their own address. Due to a weakness in the NTP protocol,[the amplification factor of the attack can be up to 206 times](https://www.cloudflare.com/learning/ddos/ntp-amplification-ddos-attack/) , making NTP servers a very effective DDoS tool. Shortly after the attack, the[U.S. Computer Emergency Readiness team explained NTP amplification attacks](https://www.cisa.gov/uscert/ncas/alerts/TA14-013A) are, “especially difficult to block” because “responses are legitimate data coming from valid servers.”


### 11. The Spamhaus DDoS Attack in 2013


In 2013, a huge[DDoS attack was launched against Spamhaus](https://www.computerworld.com/article/2495967/update--spamhaus-hit-by-biggest-ever-ddos-attacks.html) , a nonprofit threat intelligence provider. Although Spamhaus, as an anti-spam organization, is regularly attacked and had DDoS protection services already in place, this attack—a reflection attack estimated at 300 gigabits of traffic per second—was large enough to knock its website and part of its email services offline.


#### *Why the Spamhaus Attack Matters*


The cyberattack was traced to a member of a Dutch company named Cyberbunker, which had apparently targeted Spamhaus after it blacklisted the company for spamming. This illustrates that companies or rogue employees can mount DDoS attacks with immense brand damaging and serious legal consequences.


### 12. The Six Banks DDoS Attack in 2012


On March 12, 2012,[six U.S. banks were targeted by a wave of DDoS attacks](https://www.nytimes.com/2012/10/01/business/cyberattacks-on-6-american-banks-frustrate-customers.html) : Bank of America, JPMorgan Chase, U.S. Bank, Citigroup, Wells Fargo, and PNC Bank. The attacks were carried out by hundreds of hijacked servers from a botnet called Brobot with each attack generating over 60 gigabits of DDoS attack traffic per second.


At the time, these attacks were unique in their persistence. Rather than trying to execute one attack and then backing down, the perpetrators barraged their targets with a multitude of attack methods in order to find one that worked. So, even if a bank was equipped to deal with a few types of DDoS attacks, they were helpless against other types of attack.


#### *Why the Six Banks Attack Matters*


The most remarkable aspect of the bank attacks in 2012 was that the attacks were, allegedly,[carried out by the Izz ad-Din al-Qassam Brigades](https://en.wikipedia.org/wiki/Izz_ad-Din_al-Qassam_Brigades) , the military wing of the Palestinian Hamas organization. Moreover, the attacks had a huge impact on the affected banks in terms of revenue, mitigation expenses, customer service issues, and the banks’ branding and image.


## Holistic DDoS Attack Protection with A10 Defend


For multi-vector DDoS attacks that are targeting the network layer,[A10 Defend](https://www.a10networks.com/products/a10-defend/) employs advanced defense strategies that protect against all kinds of cyberattacks including new, novel DDoS attacks that could bring down online and in-house services.


For Layer 7 DDoS protection, ThreatX by A10 Networks provides L7 DDoS protection as part of a comprehensive web application protection platform. Visit the A10 Defend[DDoS protection solution page](https://www.a10networks.com/solutions/security/ddos-protection/) to learn more about the ThreatX L7 DDoS protection approach.


For additional insight, including the top reflector searches and DDoS research insights performed by attackers, download the complete A10 Networks report,[DDoS Attacks— Evolving Game of Threat Actors](https://www.a10networks.com/resources/reports/ddos-weapons-report) .


---


## FAQs


Cloudflare reported a 7.3 Tbps attack in 2025.


Mirai hijacked IoT devices to launch DDoS attacks in 2016, taking down Brian Krebs’ blog, OVH, and DNS provider Dyn, disrupting Netflix, Twitter, and PayPal.


Attackers exploited Memcached servers to amplify traffic 51,200x, hitting 1.35 Tbps. GitHub mitigated it in under 10 minutes.


Unsecured IoT devices, DDoS-as-a-service platforms, and AI-enhanced attack tools are driving rapid growth in frequency, ease of execution, and scale.


A multi-layered approach including traffic scrubbing, behavioral analytics, and rate limiting is essential. A10 Defend provides real-time, carrier-grade DDoS mitigation. ThreatX by A10 provides robust L7 DDoS protection.


### Share this post:


[Share on X (Twitter)](https://twitter.com/intent/tweet?text=The%20Most%20Famous%20DDoS%20Attacks%20in%20History&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2F5-most-famous-ddos-attacks%2F)[Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.a10networks.com%2Fblog%2F5-most-famous-ddos-attacks%2F)[Share on LinkedIn](https://www.linkedin.com/shareArticle?mini=1&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2F5-most-famous-ddos-attacks%2F&title=The%20Most%20Famous%20DDoS%20Attacks%20in%20History&source=https%3A%2F%2Fwww.a10networks.com)Share on Email


Categories:


[Network Security](https://www.a10networks.com/blog/category/network-security/)


---


** Previous Post


[Post-quantum Cryptography Comes to A10 SSL/TLS Data Plane](https://www.a10networks.com/blog/post-quantum-cryptography-comes-to-a10-ssl-tls-data-plane/)


Next Post **


[5 Key Takeaways from Application and API Security Trends Report — And What They Mean for ThreatX](https://www.a10networks.com/blog/5-key-takeaways-from-application-and-api-security-trends-report-and-what-they-mean-for-threatx/)


---


[A10 Staff](https://www.a10networks.com/blog/author/ccunningham)


| May 12, 2026
