---
schema_version: "1.0.0"
document_id: "cde378994620bc4d1610cbff6c1f96750083d561ca1d8186ef61ec6684204663"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-27-7/"
published_at: "2026-07-03T13:00:49+00:00"
first_seen_at: "2026-07-20T23:17:13.754958+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:94088dd5678b8807a0d9c36a030388fbabda7306e54a84d30fc91ef3042577a4"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 27

## The Good | Authorities Apprehend Iranian Cybercriminal & Extradite UNC3944 Hacker


Montenegrin law enforcement, alongside the FBI, have[apprehended](https://www.reuters.com/world/montenegro-police-fbi-arrest-iranian-wanted-by-us-hacking-2026-06-26/) a 39-year-old dual Iranian and Turkish citizen wanted by the U.S. government for several cybercrime offenses


. Arrested in Kotor, the suspect faces charges in the Southern District Court of New York for conspiracy to commit computer fraud, hacking, and[identity theft](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-personally-identifiable-information-pii-personal-health-information-phi/) .


Since 2013, this individual allegedly orchestrated mass cyberattacks against more than 150 American universities and inflicted damages estimated at over $3.4 billion. Investigators say the stolen data and compromised academic[credentials](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-credential-theft/) directly benefited the Islamic Revolutionary Guard Corps and various Iranian state entities. The case now proceeds to a High Court judge for formal extradition hearings. The arrest follows recent[warnings](https://www.cisa.gov/news-events/cybersecurity-advisories/aa26-097a) from U.S. cybersecurity agencies regarding escalating Iranian state-sponsored operations targeting critical domestic infrastructure.


A 19-year-old dual United States and Estonian citizen, Peter Stokes, has been[extradited](https://www.justice.gov/opa/pr/alleged-member-criminal-cyber-hacking-group-scattered-spider-arrested-finland-and-extradited) to face federal charges for his role as a core member of the UNC3944


( *aka* Scattered Spider, oktapus) cybercrime syndicate. Finnish authorities initially apprehended Stokes at the Helsinki airport as he attempted to board a flight to Japan. Prosecutors are accusing him of orchestrating multiple high-profile corporate[breaches](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-a-data-breach/) , using intense social engineering tactics against IT helpdesks to bypass multi-factor authentication controls.


Source: U.S. DoJ


In one notable May 2025[incident](https://www.sentinelone.com/blog/dragonforce-ransomware-gang-from-hacktivists-to-high-street-extortionists/) , UNC3944 compromised a multibillion-dollar retailer, demanding an $8 million ransom while inflicting over $2 million in operational disruption and remediation costs. UNC3944 operators are also responsible for more than 100 network intrusions globally, all of which yield upwards of $100 million in illicit extortion payments. Stokes remains in federal custody in Chicago, facing charges of fraud, conspiracy, and computer intrusion.


## The Bad | Russian Intelligence Exploit Phishing Campaigns To Steal Signal Backup Keys


CISA and the FBI are[warning](https://www.ic3.gov/PSA/2026/PSA260626) that Russian state-sponsored threat actors have made large strides in evolving their phishing operations to target the backup recovery keys of Signal users


. In an update to their[March 2026](https://www.ic3.gov/PSA/2026/PSA260320) advisory, the two agencies attribute this ongoing activity to Russian Intelligence Services (RIS), including Russia’s Federal Security Service (FSB) Border Guards and the country’s military.


Tracked as UNC5792 and UNC4221, these campaigns specifically target high-value individuals, including government officials, military personnel, journalists, and policy analysts. Previously, operators focused on harvesting standard verification codes or tricking users into silently linking unauthorized devices. Now, they employ[social engineering](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-social-engineering/) to access private communications without compromising the application’s underlying end-to-end encryption.


During targeted intrusions, attackers masquerade as official Signal support personnel and send direct messages falsely claiming the platform requires mandatory[two-factor verification](https://www.sentinelone.com/cybersecurity-101/identity-security/what-is-multi-factor-authentication-mfa/) following alleged international cyberattacks. The operators systematically guide victims through the specific process of enabling the[Secure Backups feature](https://signal.org/blog/introducing-secure-backups/) , instructing them to paste their newly generated recovery key directly into the chat interface.


Once adversaries obtain this critical key, they seamlessly download and decrypt the victim’s entire historical message archive onto their own controlled devices. Simply registering a new account under the same phone number does not natively invalidate a compromised key – users must actively generate a new backup key within their application settings to effectively secure future communications


.


Source: U.S. Rewards for Justice Program


The U.S. Department of State recently announced a substantial reward of up to[$10 million](https://rewardsforjustice.net/rewards/unc5792/) for information leading to the identification or location of these operatives. Through the Rewards for Justice program, federal authorities actively seek actionable intelligence regarding the syndicates’ operational infrastructure, illicit funding mechanisms, and direct affiliations with Russian intelligence services.


## The Ugly | Unknown Hackers Breach Department of Homeland Security Information Network


The Department of Homeland Security is actively investigating a cyberattack that recently compromised its Homeland Security Information Network (HSIN)


. The network is an information sharing platform used by federal, state, local, and private-sector partners, specifically to share sensitive but unclassified data amongst the government and internationally.


According to an initial[report](https://www.nextgov.com/cybersecurity/2026/06/hackers-breached-dhs-information-sharing-network-people-familiar-say/414534/) , an unidentified threat actor orchestrated the intrusion between late May and early June. Investigators indicate that the attackers targeted the[HSIN’s](https://www.dhs.gov/homeland-security-information-network-hsin) core servers alongside a SharePoint environment designed for extensive interagency collaboration.


Source: dhs.gov


While officials have not yet attributed the breach to a specific foreign government or syndicate, the full extent of data exposure remains unclear. The compromised platform routinely supports real-time incident management, intelligence exchange regarding persons of interest, and operational coordination. Because the United States is overseeing security for World Cup matches across the country, experts raise concerns that the intrusion could have exposed critical security planning, response procedures, and communication protocols


.


In a public[statement](https://www.wired.com/story/a-dhs-data-hub-exposed-sensitive-intel-to-thousands-of-unauthorized-users/) to the press, a departmental spokesperson confirmed the incident, clarifying that the breach strictly involved an unclassified legacy information-sharing environment. Security personnel promptly isolated the affected systems and mitigated the underlying vulnerability before starting[forensics](https://www.sentinelone.com/cybersecurity-101/services/what-is-digital-forensics-and-incident-response-dfir/) .


Officials emphasized that the attack did not impact classified networks, and the primary system remains operational for authorized partners. As the investigation continues, authorities strongly urge U.S. government staff, contractors, and associated partners to remain vigilant while defense teams harden the underlying infrastructure against further unauthorized network access attempts.
