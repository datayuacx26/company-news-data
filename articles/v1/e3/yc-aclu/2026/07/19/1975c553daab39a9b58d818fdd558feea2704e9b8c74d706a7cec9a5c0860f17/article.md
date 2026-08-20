---
schema_version: "1.0.0"
document_id: "1975c553daab39a9b58d818fdd558feea2704e9b8c74d706a7cec9a5c0860f17"
company_key: "yc-aclu"
company: "ACLU"
source_id: "yc-aclu-rss-353a9e6c4c2a"
canonical_url: "https://www.aclu.org/news/privacy-technology/counter-drone-zero-days"
published_at: "2026-07-31T13:23:31+00:00"
first_seen_at: "2026-07-31T16:52:51.411085+00:00"
fetched_at: "2026-07-31T16:52:54.042532+00:00"
content_hash: "sha256:2e43c848acae09bf67ab17dd84b3b5870e73efc7ebdc063c3639f853602527ea"
---

# Government Playing a Dangerous Game With Drone Cybersecurity

[Back to News & Commentary](https://www.aclu.org/news/)


# Government Playing a Dangerous Game With Drone Cybersecurity


Return of the zombie idea that software vulnerabilities can be reserved for good actors


Jay Stanley ,
ACLU Speech, Privacy, and Technology Project


**Share This Page**


July 31, 2026


[Subscribe to the Free Future Newsletter](https://action.aclu.org/signup/free-future-newsletter)
[Or get immediate notifications of new posts via Substack](https://ourfreefuture.substack.com/subscribe)
*[Free Future home](http://www.aclu.org/freefuture)*


Earlier this month I[wrote](https://www.aclu.org/news/privacy-technology/government-drone-seizures) about how police have been seizing hundreds of drones around the country for alleged violations of no-fly rules. The counter-drone business is hopping today, with law enforcement around the country[highly](https://www.aclu.org/news/privacy-technology/drones-for-people-not-just-police-and-corporations)[focused](https://www.aclu.org/news/privacy-technology/drone-restrictions-and-journalism) on gaining the ability to deal with drones flying places they’re not supposed to be, as well as the so-far-nonexistent but probably eventually[inevitable](https://little-flying-robots.ghost.io/targeted-drone-attacks-are-killing-more-civilians-than-ever-before-i-made-a-map-about-it/) prospect of a violent attack via drone. Often called counter-UAS (for “uncrewed aerial system,” a[bureaucratic](https://www.aclu.org/blog/technology-and-liberty-national-security/should-we-call-them-drones-or-uavs) term for drone), the field is dedicated to the difficult problem of how to ground, capture, destroy, or otherwise “mitigate” a rogue drone. Within the counter-UAS field lurks a significant public policy issue that has not been discussed enough: the hacking of drones.


It’s not easy to defend against drones. Techniques for mitigating them include shooting at them with projectiles, lasers, or microwaves, jamming GPS or other radio signals, and sending up defensive drones with big nets. US law enforcement officers don’t operate on a battlefield, however (despite how many of them dress and sometimes[behave](https://en.wikipedia.org/wiki/Warrior_policing) ). In a civilian environment, massive interference with the radio spectrum, shooting[dangerous lasers](https://apnews.com/article/faa-el-paso-texas-air-space-closed-1f774bdfd46f5986ff0e7003df709caa) into our crowded skies, and aerial shoot-outs that send wreckage crashing down on civilians’ heads just won’t do.


There is another counter-drone technique that supposedly avoids all these problems: hacking into a drone to take it over and steer it safely away. But that raises one of the longest-running issues in cybersecurity: the creation or hoarding of software vulnerabilities that can be used by the authorities to break into software, but which also leave the door open for malevolent hackers to do so as well.


Those vulnerabilities can take the form of software bugs that make devices vulnerable to takeover or other hacking, known in the cybersecurity field as “zero days.” Also possible but currently unlikely (because of the amount of coordination and secrecy that would be required) is that manufacturers are building intentional backdoors into their devices for officials to use. Either way, exploitation of vulnerabilities in drones implicates a serious problem that has arisen in many different contexts: there is no way you can have a vulnerability in a technology that can be used only for good purposes.


This has been central to perennial debates over government encryption backdoors, in which law enforcement officials push for laws to require the writers of encrypted communications software to introduce intentional vulnerabilities so that the police can eavesdrop. It also arises in the conflict between the NSA’s offensive and defensive missions (“we’ve discovered a way to break into a Windows computer; do we keep it to ourself so we can spy on our enemies, or do we tell Microsoft so they can fix the bug and protect Americans?”). At the ACLU we’ve called for prioritizing public security over secret private power.


Here the same issue is repeating itself: if the government knows of a bug that allows an attacker to hijack a drone, but doesn’t inform the manufacturer, it’s a betrayal of the public interest that leaves every drone that uses that software vulnerable to takeover by malevolent actors.


It doesn’t help that consumer drones[generally](https://owasp.org/www-project-top-10-drone-security-risks/) have[terrible](https://cheatsheetseries.owasp.org/cheatsheets/Drone_Security_Cheat_Sheet.html)[security](https://dl.acm.org/doi/10.1007/s00521-023-08857-7) . Many protocols that govern communications between ground controllers and drones are weak — many still transmit data over unencrypted channels, and lack strong authentication measures that would ensure the drone only accepts commands from the controller being operated by its owner and not from anyone else.


It’s surprising to me that, amidst the enormous amount of hype about the threat of drone terrorism and the like, drone protocols remain so weak. I can’t help wondering why we haven’t seen a stronger push for security reforms — and whether it’s because too many shortsighted people in authority would rather drones *stay* vulnerable.


**An approach that is already being tapped** The hacking of drones is very real, however, and has[happened](https://bisresearch.com/insights/why-drone-cybersecurity-matters-and-its-advancements-in-2025) in both defense and commercial contexts, including to smuggle contraband into prisons via drone. And hacking is very much in play for counter-UAS. In reporting on efforts to combat drone flights near World Cup soccer games, the LA Times[reported](https://www.latimes.com/california/story/2026-06-18/inside-fbis-battle-to-keep-drones-out-of-world-cup-sites-amid-height-terrorism-fears) that


> More than once, drones have gotten close enough to the massive stadium in Inglewood to be taken over and brought to the ground by the FBI. That ability, according to a white paper by one of the manufacturers, is possible through technology that allows an agent to communicate with the drone and substitute themselves as the operator, take it over and ground it.


It’s not clear what white paper the Times is referring to, but some companies are certainly marketing their ability to hijack drones. A company called D-Fend Solutions sells a product that it[says](https://d-fendsolutions.com/enforceair/) provides advanced radio frequency (RF) “cyber takeover technology.”


> The system detects, locates and identifies rogue drones in protected airspace, then neutralizes threats by executing RF cyber-takeovers — whether as a standalone, multilayer, or integrated system — for safe landings and controlled outcomes.


D-Fend (which[just reached](https://dronelife.com/2026/06/01/motorola-solutions-makes-1-5-billion-bet-on-counter-drone-technology/) an agreement to be acquired by police technology vendor Motorola Solutions) offers little detail about how it executes these “cyber takeovers,” but it seems clear that whether the vulnerabilities are exotic zero days or run-of-the-mill protocol flaws, the company’s business model is to stay ahead of efforts to secure drones by identifying vulnerabilities and monetizing them on behalf of security agencies rather than fixing them so that all drones can be more secure against hijacking. A company[blog post](https://d-fendsolutions.com/blog/how-to-control-a-drone/) on “How to Control a Drone” warns that threat actors are “evolving rapidly, using increasingly complex tactics” to “bypass detection and mitigation.”


By “bypass mitigation,” the company means “protect their drones against takeover by others.” But again, if threat actors can’t protect their drones against takeover by others, neither can anyone else. Still, the company[writes](https://d-fendsolutions.com/blog/how-to-control-a-drone/) ,


> To stay ahead, counter-drone technology must… execute protocol-based takeovers that can adapt to diverse manufacturer-specific protocols.
>
>
> With modern drone threats, control is the central vulnerability, and understanding the concept of control allows defenders to respond with precision. The best[C-UAS](https://d-fendsolutions.com/cuas-mitigation/) solutions are focused on giving control back to the defenders.


The myth that vulnerabilities can be reserved for defenders and won’t be seized upon by bad actors appears to be a zombie idea — one that never dies.


In 2023 I represented the ACLU on an FAA Aviation Rulemaking Committee that made recommendations to the agency on counter-UAS, and I raised this issue. I[noted](https://www.faa.gov/sites/faa.gov/files/UAS-Detection-Mitigation-Systems-ARC_Final-Report_02052024.pdf#page=142) that the committee’s[final report](https://www.faa.gov/sites/faa.gov/files/UAS-Detection-Mitigation-Systems-ARC_Final-Report_02052024.pdf) “incorporates the concept of mitigating drones through ‘takeover’ of a targeted aircraft” and recommended that


> the FAA work to require that discovery of any security vulnerabilities in drones be immediately shared with drone manufacturers so they can be patched, and not kept secret for C-UAS purposes.


We also recommended “a ban on the creation of backdoors in drone security systems.”


But as far as I can tell this misguided approach to drone security seems to be continuing along its merry way largely unquestioned. Hopefully the lack of wisdom in that strategy won’t be revealed in tragic ways.


## Related Issues


---


- [Privacy & Technology](https://www.aclu.org/issues/privacy-technology)
- [Domestic Drones](https://www.aclu.org/issues/privacy-technology/surveillance-technologies/domestic-drones)
- [Cybersecurity](https://www.aclu.org/issues/privacy-technology/internet-privacy/cybersecurity)
- [Surveillance Technologies](https://www.aclu.org/issues/privacy-technology/surveillance-technologies)
- [Internet Privacy](https://www.aclu.org/issues/privacy-technology/internet-privacy)


## Related Content


---


-


Press Release


Jul 2026


Privacy & Technology


### Civil Liberties Groups File Amicus Brief Opposing NYPD’s Illegal, Secret Seizure and Storage of DNA


NEW YORK – Today, the American Civil Liberties Union (ACLU), the New York Civil Liberties Union (NYCLU), and the Cato Institute filed an amicus brief with the Second Circuit in Leslie v. City of New York, in support of The Legal Aid Society’s class action challenge to New York City’s “Suspect DNA Index.” The brief argues that the Suspect Index is a massive and unlawful DNA database. It also emphasizes that DNA contains an individual’s highly sensitive and private information, and that DNA that people inevitably leave on items they discard—from a used cup to a cigarette butt to a tissue—cannot be seized, searched, and stored without a warrant. The Suspect Index contains DNA samples from more than 34,000 people, including kids and people who have never been charged with a crime. Many of the samples are gathered through the NYPD’s practice of collecting DNA by arresting individuals for purported crimes, holding them for many hours, bringing them into an interrogation room specifically cleaned and set up to collect their DNA, pressuring them to drink or touch something, and concluding the interview as soon as that’s done. The NYPD then leads the individuals out in handcuffs (such that they cannot take whatever they touched or consumed even if they wanted to) and then goes back in to obtain the item specifically to extract the unavoidably shed DNA. “For more than a decade, the NYPD has been operating a rogue DNA database not authorized by law,” said Beth Haroules, director of disability justice litigation at the NYCLU. “No New Yorker should have their DNA taken illegally and stored in a secret database. This destructive practice disproportionately targets Black and brown New Yorkers and violates our Fourth Amendment rights. No court should allow this intrusion on our privacy.” “The government cannot treat our DNA like it treats our garbage, and seize, search, and store it without a warrant,” said Vera Eidelman, senior staff attorney at the ACLU’s Speech, Privacy, and Technology Project. “Usually we rely on hypotheticals to highlight the dystopian possibilities of this kind of privacy invasion, but no hypothetical could be more troubling than New York City’s actual practice: amassing thousands of people’s DNA without their knowledge, their consent, or any court-ordered or legislative guardrails. If the Second Circuit does not intervene, all I can say is: New Yorkers, hold on to your trash.” The brief argues that the Suspect Index violates state law, which establishes a separate state databank as the only DNA index that can exist in New York and sets strict limits on whose DNA can be collected, when, for what purpose, and when it has to be deleted—all of which the Suspect Index ignores. The Index also includes samples from people who have been expressly excluded as suspects in a crime, arrestees who have never been convicted of a crime, and exonerated or acquitted individuals. “The secret, dragnet collection of DNA for inclusion on a 'Suspect Index' sounds dystopian. Yet it's an affront to privacy and the presumption of innocence tens of thousands of New Yorkers have already suffered. The Fourth Amendment protects against this—and hopefully, so will the Second Circuit,” said Matthew Cavedon, Director of the Project on Criminal Justice at the Cato Institute. A copy of the brief can be found here.


**Court Case:** Leslie v. City of New York


**Affiliate:** New York


[Civil Liberties Groups File Amicus Brief Opposing Nypd’s Illegal, Secret Seizure And Storage Of Dna. Explore Press Release.](https://www.aclu.org/press-releases/civil-liberties-groups-file-amicus-brief-opposing-nypds-illegal-secret-seizure-and-storage-of-dna)


-


New York


Jul 2026


Privacy & Technology


### Leslie v. City of New York


**Status:** Ongoing


[Leslie V. City Of New York. Explore Case.](https://www.aclu.org/cases/leslie-v-city-of-new-york)


-


News & Commentary


Jul 2026


Privacy & Technology


### State Chief Privacy Officers Can Have Real Power


Utah has given its CPO a meaningful role — and he has thoughts on how other states should follow


**By:** Jay Stanley


[State Chief Privacy Officers Can Have Real Power. Explore News & Commentary.](https://www.aclu.org/news/privacy-technology/state-chief-privacy-officers)


-


News & Commentary


Jul 2026


Privacy & Technology


### Leaked Police Drone Videos Highlight Need For Independent Review of the Technology’s Usefulness


And other takeaways from watching three hours of aerial police videos


**By:** Jay Stanley


[Leaked Police Drone Videos Highlight Need For Independent Review Of The Technology’s Usefulness. Explore News & Commentary.](https://www.aclu.org/news/privacy-technology/leaked-drone-video-takeaways)
