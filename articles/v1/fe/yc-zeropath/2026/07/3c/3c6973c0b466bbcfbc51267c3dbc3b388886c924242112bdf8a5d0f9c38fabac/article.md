---
schema_version: "1.0.0"
document_id: "3c6973c0b466bbcfbc51267c3dbc3b388886c924242112bdf8a5d0f9c38fabac"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/eu-cybersecurity-regulation-report"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-05T04:10:16.390689+00:00"
fetched_at: "2026-08-05T04:10:18.200500+00:00"
content_hash: "sha256:a1672306826a91fd3479802d027f2ba35daba173c72c1dd5b1407b1f95ca00c4"
---

# The CISO's Guide to EU Cybersecurity Regulation (2026 Edition)

## Executive summary: EU cybersecurity compliance in 2026


Eight EU instruments now shape what a security program must do, and most of them are already live. This guide covers all eight in plain English: what each one is, whether it reaches you, what it actually requires, and what to do next.


Four form the core. **NIS2** (Directive (EU) 2022/2555) sets the cybersecurity baseline for medium and large organizations across 18 critical sectors: ten risk-management measures, incident reporting on a 24-hour/72-hour/one-month clock, and personal liability for the management body. It applies now, through national laws. The **Cyber Resilience Act (CRA)** makes security a condition of selling hardware and software in the EU; its first hard deadline is 11 September 2026, when 24-hour reporting of actively exploited vulnerabilities begins. The **Radio Equipment Directive (RED)** already bit: its cybersecurity requirements for wireless products became mandatory on 1 August 2025. And **DORA** has governed financial-sector ICT risk since 17 January 2025, reaching through EU financial entities into their vendors' contracts, wherever those vendors sit.


Four more belong on the radar. The **CER Directive** covers physical resilience; designation letters are arriving now, and obligations follow within roughly ten months of notification. The **Cybersecurity Act** created a voluntary certification framework, with the EUCC scheme live since 27 February 2025. The **Cyber Solidarity Act** builds EU response capacity and asks nothing of private companies. And the **AI Act** reaches its general application date on 2 August 2026, though its high-risk obligations were deferred to 2 December 2027 and 2 August 2028; the near-term duties are transparency and general-purpose AI rules, not the high-risk tier.


One thread runs through every chapter: these regulations ask for the same five things, and each lands on a desk you already have. Fast incident reporting, vulnerability management with SBOMs, secure development you can evidence, supply-chain control, and board-level accountability. Build one program, and produce eight sets of evidence from it.


## In this guide


- Based outside the EU? These laws can still reach you
- The eight instruments at a glance
- Map your exposure: five questions
- NIS2: the one with the 24-hour clock and the liable board
- The Cyber Resilience Act (CRA)
- The Radio Equipment Directive (RED)
- DORA: the Digital Operational Resilience Act
- Also on your radar: CER, the Cybersecurity Act, and the Cyber Solidarity Act
- The EU AI Act: what it adds to your security program
- One program, many regulations
- The unified checklist: eight things to run this quarter
- The EU compliance calendar
- How ZeroPath helps you get ready
- Glossary
- References


## Based outside the EU? These laws can still reach you


No EU cybersecurity law asks where you are incorporated. The triggers are commercial: what you sell, where, and to whom.


Instrument The extraterritorial trigger


**CRA** (Cyber Resilience Act), Reg. (EU) 2024/2847 You make a product with digital elements available on the Union market. "Manufacturer" duties attach to whoever markets it under its own name or trademark, with no EU establishment required ([Reg. 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) ;[Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-summary) ).


**NIS2** , Dir. (EU) 2022/2555 You are an Annex I or II entity type providing services in the Union and you clear the size test (Art. 2(1)). Cloud, data centre and content delivery network (CDN) providers, DNS and top-level domain (TLD) services, managed service and managed security providers, plus online marketplaces, search engines and social platforms answer instead to the Member State of their **main establishment** . With no EU establishment, you must appoint an **EU representative** and fall under that state's jurisdiction; without one, any Member State you serve may take legal action (Art. 26(1)(b), (2), (3)) ([Dir. 2022/2555](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ).


**NIS2, as a supplier** Contractual, not a direct legal duty. Art. 21(2)(d) makes in-scope entities manage the "security-related aspects" of relationships with their "direct suppliers or service providers" ([Dir. 2022/2555](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ). What reaches a foreign supplier is the customer's obligation, enforced by losing the deal rather than by a fine.


**DORA** (Digital Operational Resilience Act), Reg. (EU) 2022/2554 You sell ICT services to an EU financial entity, which must impose defined contract terms. For critical or important functions these include "unrestricted rights of access, inspection and audit" (Art. 30(3)(e)(i)). Designation as a **critical ICT third-party provider (CTPP)** makes it direct: financial entities may only keep using a third-country CTPP if it establishes an EU subsidiary within 12 months of designation (Art. 31(12)) ([Reg. 2022/2554](https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng) ). The European Supervisory Authorities published the first CTPP list on 18 November 2025 ([EBA](https://www.eba.europa.eu/publications-and-media/press-releases/european-supervisory-authorities-designate-critical-ict-third-party-providers-under-digital) ).


**RED** (Radio Equipment Directive), Dir. 2014/53/EU You place radio equipment on the EU market. Its cybersecurity requirements (Art. 3(3)(d)–(f)), activated by Delegated Regulation (EU) 2022/30, apply to equipment placed on the market from 1 August 2025, the date fixed by Delegated Regulation (EU) 2023/2444 ([EUR-Lex](https://eur-lex.europa.eu/eli/reg_del/2023/2444/oj/eng) ;[SGS](https://www.sgs.com/en-dk/news/2025/06/red-cybersecurity-requirements-mandatory-on-1-august-2025) ).


**AI Act** , Reg. (EU) 2024/1689 You place an AI system or general-purpose AI (GPAI) model on the Union market, "irrespective of whether those providers are established … within the Union or in a third country" (Art. 2(1)(a)). The hook people miss: a third-country provider or deployer is caught "where the output produced by the AI system is used in the Union" (Art. 2(1)(c)) ([Reg. 2024/1689](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng) ). That reaches a model hosted anywhere in the world serving an EU customer with no EU sale.


**CER** (Critical Entities Resilience Directive), Dir. (EU) 2022/2557 Indirect. Duties attach only to entities a Member State identifies as critical, which requires operating on its territory; that deadline passed on 17 July 2026 ([Taylor Wessing](https://digital-regulation.taylorwessing.com/welcome/the-critical-entities-resilience-directive-cer) ). Your EU customer may have been designated, and NIS2 then applies to it regardless of size (NIS2 Art. 2(3)) ([Dir. 2022/2555](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ).


**Cybersecurity Act** , Reg. (EU) 2019/881 No direct duty; a market-access lever. Certification "shall be voluntary, unless otherwise specified by Union law or Member State law" (Art. 56(2)) ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2019/881/oj/eng) ). The first scheme, the Common Criteria-based EUCC for ICT products, has been open to vendors since 27 February 2025 with EU-wide recognition; a renewed framework was proposed on 20 January 2026 ([Commission](https://digital-strategy.ec.europa.eu/en/policies/cybersecurity-certification-framework) ).


**Cyber Solidarity Act** , Reg. (EU) 2025/38 Indirect: it builds *state* capability, in force since 4 February 2025. The commercial hook is the EU Cybersecurity Reserve, buying incident response from "trusted managed security service providers" chosen on the Art. 17(2) criteria (Art. 14(2)). Third-country control limits sit not there but in the amended Digital Europe Programme (Art. 22; recitals 8–9) ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2025/38/oj/eng) ).


**EU regulation follows the product, the service, and the customer, not your headquarters.**


## The eight instruments at a glance


One row per instrument. Every cell is unpacked, with citations, in the section it points to.


Instrument What it is Who's in scope Key deadline Core obligations Max penalty


**NIS2** , Dir. (EU) 2022/2555 Cybersecurity baseline for critical sectors Medium and large entities in 18 sectors; some types at any size Live now: national laws apply since 18 October 2024 Measures, reporting, registration At least €10M or 2% of group worldwide turnover (essential); €7M or 1.4% (important), whichever is higher


**CRA** , Reg. (EU) 2024/2847 Product-security law enforced via CE marking Anyone selling hardware or software into the EU, wherever headquartered 11 September 2026 (reporting); 11 December 2027 (full application) Secure products, SBOM, report €15M or 2.5% of worldwide turnover, whichever is higher


**RED** , Dir. 2014/53/EU CE-marking cybersecurity rules for wireless products Radio equipment placed on the EU market Mandatory since 1 August 2025 Harden connected hardware National penalties; loss of CE mark, recalls, market bans


**DORA** , Reg. (EU) 2022/2554 Directly binding ICT-risk rulebook for finance 20 categories of financial entities, plus their ICT vendors Applies since 17 January 2025; registers filed each spring Evidence, test, report National penalties; critical vendors: up to 1% of average daily worldwide turnover, daily, up to six months


**CER** , Dir. (EU) 2022/2557 Physical-resilience twin of NIS2 Entities designated by member states in 11 sectors Designation letters arriving now; obligations 9–10 months after notification Assess, harden, notify National penalties


**Cybersecurity Act** , Reg. (EU) 2019/881 ENISA mandate plus voluntary EU certification framework Vendors that choose, or are asked, to certify EUCC scheme live since 27 February 2025 Certify when asked None (voluntary)


**Cyber Solidarity Act** , Reg. (EU) 2025/38 EU-level detection and emergency-response capacity Member-state authorities; security providers may bid into the Reserve In force since 4 February 2025 Nothing mandatory None


**AI Act** , Reg. (EU) 2024/1689 Risk-tiered product-safety law for AI Providers and deployers reaching the EU market, or whose output is used in the EU 2 August 2026 (general application); high-risk from 2 December 2027 Adversarial testing, logging, reporting Up to €35M or 7% of worldwide turnover, whichever is higher (SMEs: the lower)


## Map your exposure: five questions


Answer in order; every "yes" adds a regime.


**1. Do you ship software or hardware with digital elements to EU customers?**


- Yes → **CRA** . Art. 14 reporting of actively exploited vulnerabilities and severe incidents starts **11 September 2026** , ahead of full application on 11 December 2027, and it covers products already on the market ([Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-summary) ).
- Any of it wireless? → also **RED** .


**2. Are you an entity type in NIS2 Annex I or II, operating in the EU, above the size test?**


- The annexes cover **18 sectors** : 11 of high criticality in Annex I, 7 others in Annex II ([Commission](https://digital-strategy.ec.europa.eu/en/news/commission-refers-ireland-spain-france-and-netherlands-court-justice-failing-transpose-rules) ).
- Size test: at least medium-sized under Recommendation 2003/361/EC, i.e. above the small-enterprise ceilings of fewer than 50 staff and turnover or balance sheet total of €10 million (Art. 2(1)) ([Arthur Cox](https://www.arthurcox.com/knowledge/nis2-sme-guidelines-how-do-they-apply-and-thresholds/) ).
- Size is not the whole test: several categories are in scope at any size (Art. 2(2)–(4)) ([Dir. 2022/2555](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ); the NIS2 deep-dive below has the list.
- **Member-state-variable.** NIS2 is a directive with a minimum-harmonisation clause (Art. 5), so national scope can be wider; check each transposition. On 8 July 2026 the Commission referred Ireland, Spain, France and the Netherlands to the Court of Justice for failing to notify full transposition ([Commission](https://digital-strategy.ec.europa.eu/en/news/commission-refers-ireland-spain-france-and-netherlands-court-justice-failing-transpose-rules) ).
- Not in scope but selling to someone who is? NIS2 reaches you as a **contractual** requirement. CER designation only happens if you operate on a Member State's territory.


**3. Are you a financial entity in the EU, or do you sell ICT services to one?**


- Financial entity → **DORA** directly. ICT vendor → through the Art. 30 contract terms. Systemically important → possible **CTPP** designation.


**4. Do you ship AI features to EU users, or produce output used in the EU?**


- Yes → **AI Act** . Prohibitions apply since 2 February 2025, GPAI model obligations since 2 August 2025. Art. 50 transparency still lands on 2 August 2026 and was not deferred; systems already on the market get until 2 December 2026 for the Art. 50(2) marking duty. The Digital Omnibus on AI, Reg. (EU) 2026/1744 (in force 27 July 2026), moved high-risk obligations to 2 December 2027 for standalone Annex III systems and 2 August 2028 for AI embedded in Annex I products ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2026/1744/oj/eng) ;[Gibson Dunn](https://www.gibsondunn.com/eu-ai-act-omnibus-agreement-postponed-high-risk-deadlines-and-other-key-changes/) ).
- AI inside a product? Then **CRA overlaps** : one product, two sets of essential requirements.


**5. Selling to EU governments or enterprises that ask for certification?**


- Yes → **Cybersecurity Act schemes** . Note the CRA link: a scheme proves CRA conformity only where the Commission has said it may, and third-party assessment is generally mandatory for Annex III class II and Annex IV products ([Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-summary) ).


Every "yes" above points to a chapter below: four deep dives (NIS2, the CRA, RED and DORA), then the radar tier, then the single program that covers them all, and a calendar to keep watch on.


## NIS2: the one with the 24-hour clock and the liable board


### In one sentence


Directive (EU) 2022/2555 requires medium-sized and large organizations across 18 critical sectors to run a defined set of cybersecurity risk-management measures, report significant incidents on a 24-hour/72-hour/one-month cadence, and put the management body personally on the hook for both ([EUR-Lex](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ;[Commission](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive) ).


Deal with this one first. It is the broadest of the four core regulations in this guide, and the wait-and-see window has closed. By 8 July 2026 only Ireland, Spain, France and the Netherlands had still not notified full transposition, and on that date the Commission referred all four to the Court of Justice, asking for a lump sum plus daily penalties until they do ([Commission](https://digital-strategy.ec.europa.eu/en/news/commission-refers-ireland-spain-france-and-netherlands-court-justice-failing-transpose-rules) ;[tracker](https://digital-strategy.ec.europa.eu/en/policies/nis-transposition) ).


### Who's in scope


Scope runs through two annexes. Annex I lists 11 "sectors of high criticality": energy, transport, banking, financial market infrastructures, health, drinking water, waste water, digital infrastructure, information and communications technology (ICT) service management (business-to-business), public administration, space. Annex II adds 7 "other critical sectors": postal and courier services, waste management, chemicals, food, manufacturing, digital providers, research ([EUR-Lex, Annexes I–II](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ).


Then a size test. Article 2(1) covers entities in those annexes that qualify as medium-sized under Recommendation 2003/361/EC or exceed those ceilings. The shorthand is 50+ staff or turnover above €10 million, but the Recommendation's ceilings are the actual test. Article 2(2) catches some entities **regardless of size** , including providers of public electronic communications networks and services, trust service providers, top-level domain (TLD) name registries and domain name system (DNS) service providers ([EUR-Lex, Art. 2](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ).


**Essential vs. important.** Large entities in an Annex I sector are *essential* ; qualified trust service providers, TLD registries and DNS providers are essential at any size; everything else in either annex is *important* (Article 3(1)–(2)). Not cosmetic. Essential entities get a comprehensive ex ante and ex post supervisory regime and must document compliance on demand. Important entities get a light, ex post only regime that starts when evidence of non-compliance reaches the authority (Article 33(1);[recital 122](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ). Fine ceilings differ. And only Article 32(5) lets an authority suspend a certification and ask that a named individual be barred from management.


**Does this reach a company outside the EU?** Two ways. A non-EU entity offering cloud computing, data centre, content delivery network (CDN), DNS, managed service or managed security service (MSP/MSSP), online marketplace, search or social-networking services in the Union must designate an EU representative, and then falls under that member state's jurisdiction. Skip the designation and any member state where you provide services may take legal action against you (Article 26(1)(b) and 26(3)). Or you get pulled in commercially, as a direct supplier your in-scope EU customers have to assess under Article 21(2)(d) and 21(3). No EU footprint required.


### Key dates


Date What happened / happens


16 January 2023 Directive entered into force ([EUR-Lex](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) )


17 October 2024 Member states to adopt and publish national measures (Article 41)


18 October 2024 National measures apply; the original NIS Directive (2016/1148) repealed (Articles 41, 44)


17 April 2025 National lists of essential and important entities due; reviewed at least every two years (Article 3(3))


20 January 2026 Commission proposes targeted NIS2 amendments, COM(2026) 13. A proposal, not yet law ([Commission](https://digital-strategy.ec.europa.eu/en/library/proposal-directive-regards-simplification-measures-and-alignment-cybersecurity-act) )


8 July 2026 Ireland, Spain, France and the Netherlands referred to the Court of Justice ([Commission](https://digital-strategy.ec.europa.eu/en/news/commission-refers-ireland-spain-france-and-netherlands-court-justice-failing-transpose-rules) )


Rolling, per country Operative deadlines are national. Italy: entities first listed in 2025 get 18 months from their listing notification to adopt the baseline measures set by ACN, the national cybersecurity agency; entities first listed in 2026 have until 31 July 2027 ([ACN](https://www.acn.gov.it/portale/faq/nis/misure-di-sicurezza-e-notifica-di-incidenti) )


### Core obligations


- **Board approval and oversight.** Management bodies must approve the risk-management measures, oversee implementation, and can be held liable for the entity's infringements of Article 21. Members of the management body are required to follow training; member states must also encourage entities to offer similar training to staff on a regular basis (Article 20).
- **Ten baseline measures.** Article 21(2) demands an all-hazards approach covering *at least* : policies on risk analysis and information system security; incident handling; business continuity, backup management, disaster recovery and crisis management; supply chain security covering direct suppliers and service providers; security in acquisition, development and maintenance, including vulnerability handling and disclosure; procedures to assess the effectiveness of the measures; basic cyber hygiene and cybersecurity training; cryptography and, where appropriate, encryption; human resources security, access control policies and asset management; and, where appropriate, multi-factor or continuous authentication plus secured voice, video, text and emergency communications.
- **Supplier diligence.** Weigh each direct supplier's specific vulnerabilities and the overall quality of their products, cybersecurity practices and secure development procedures (Article 21(3)).
- **Reporting on a fixed clock.** For a significant incident, submissions go to the national computer security incident response team (CSIRT) or, where applicable, the competent authority: early warning within 24 hours of becoming aware; notification within 72 hours with an initial severity assessment and, where available, indicators of compromise; intermediate reports on request; final report within one month **of the notification** , not of the incident (Article 23(4)). An incident is significant if it has caused or could cause severe operational disruption or financial loss to you, or considerable material or non-material damage to others (Article 23(3)).
- **Registration.** Supply your name, contact details including IP ranges, sector and subsector, and the member states where you provide in-scope services, then notify any change within two weeks (Article 3(3)–(4)). In practice, via a national portal.


### What changes in your security program


Less than CISOs fear, and in different places. The Article 21(2) list maps cleanly onto ISO/IEC 27001 or the NIST Cybersecurity Framework, so for a functioning program this is a mapping-and-evidence exercise, not a rebuild. The work clusters in three places:


- **Evidence, not controls.** Article 21(2)(f) requires procedures to assess whether the measures actually work: test results, exercise records, remediation trails. Rarely kept, always asked for.
- **Supply chain becomes a register.** A classified ICT supplier inventory, security requirements written into contracts, periodic verification. A clause in the master agreement does not get you there.
- **Vulnerability disclosure.** Coordinated disclosure sits inside the legal baseline. If you have no intake path for outside reports, that is a gap on the face of the Directive.


Digital-infrastructure, ICT-service-management, digital-provider and trust-service entities have more to work with. Commission Implementing Regulation (EU) 2024/2690 of 17 October 2024 elaborates their requirements at EU level, and ENISA's technical guidance of 26 June 2025 works through 13 requirement areas with a mapping to existing standards ([ENISA](https://www.enisa.europa.eu/news/supporting-nis2-implementation-through-actionable-guidance) ;[the guidance](https://www.enisa.europa.eu/publications/nis2-technical-implementation-guidance) ). Not legally binding: check your national authority first.


### Penalties


Category Ceiling for infringing Article 21 or 23


Essential entities A maximum of at least €10,000,000 or at least 2% of total worldwide annual turnover in the preceding financial year **of the undertaking to which the entity belongs** , whichever is higher (Art. 34(4))


Important entities A maximum of at least €7,000,000 or at least 1.4% of that same worldwide annual turnover, whichever is higher (Art. 34(5))


Three details matter more than the numbers. These are *minimum maxima* : member states must provide at least this much and may go further. The turnover base is the **group's** , not the entity's, so a small in-scope subsidiary of a large parent does not get a small ceiling. And fines sit *on top of* the supervisory measures in Articles 32(4), 32(5) and 33(4), rather than replacing them (Article 34(2)).


### Common misconceptions and gotchas


**"My member state hasn't transposed, so nothing applies."** Only four states had failed to notify full transposition as of 8 July 2026, so most readers already sit inside a live national regime. And if you operate across borders you are governed by every member state where you provide services. A state's delay is the Commission's case against that state, not a defense for you.


**"It's one rulebook, and it's a project with an end date."** It's a directive, so 27 national laws sit on top of it, with different portals, catalogues, clocks and recurring registration windows. Italy shows the shape of it: ACN publishes its own baseline catalogue, a lighter annex for important entities and a heavier one for essential entities, and your adoption deadline depends on the year you were first listed ([ACN](https://www.acn.gov.it/portale/faq/nis/misure-di-sicurezza-e-notifica-di-incidenti) ). Multi-country operations need a per-country matrix.


**"Supply chain security means collecting a SOC 2."** Article 21(3) asks about each supplier's *specific* vulnerabilities and secure development procedures. A certificate is an input, not an answer.


**"The rules are about to be watered down, so let's wait."** The 20 January 2026 proposal is real, and for some entities it cuts the other way: alongside simpler jurisdictional rules and a certification route to showing Article 21 compliance, it would make submarine-cable operators and European Digital Identity and Business Wallet providers essential entities regardless of size, add a "small mid-cap" important-entity category, and require ransomware-specific disclosures. Counsel expects adoption in late 2026 or, more likely, 2027, then a 12-month transposition period. Current national rules govern for years yet ([DLA Piper](https://privacymatters.dlapiper.com/2026/02/eu-nis2-update-eu-moves-to-harmonise-cyber-controls-refine-scope-and-add-new-in-scope-entities/) ).


### Do this next


1. **Run the scope test per country and write down the reasoning.** Sector against Annex I/II, size against Recommendation 2003/361/EC, then the Article 2(2) regardless-of-size categories. Non-EU providers: check Article 26(3) for the EU representative duty.
2. **Find yourself on the national lists and register.** Confirm status on each portal and diary the recurring windows, including the two-week deadline for notifying changes.
3. **Map the ten Article 21(2) measures to existing controls and mark the gaps.** Digital-infrastructure, ICT-service, digital-provider and trust-service entities: map against Implementing Regulation (EU) 2024/2690 using the ENISA guidance.
4. **Build and rehearse the 24/72/one-month runbook.** Significance criteria in writing, named submitters, tested portal access, templates drafted, one tabletop.
5. **Brief the board in writing and log the training.** Minute the Article 20 approval and keep attendance records. Personal liability attaches here.


## The Cyber Resilience Act (CRA)


**Your first hard deadline is 11 September 2026, not December 2027.** From that date, manufacturers must report actively exploited vulnerabilities and severe incidents to their coordinating national CSIRT (computer security incident response team) and ENISA on a 24-hour clock, and the duty reaches products already in customers' hands ([Article 69(3), Regulation (EU) 2024/2847](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847) ). If you planned around 2027, you have about six weeks of runway on the part that bites first.


### In one sentence


The CRA (Regulation (EU) 2024/2847) makes cybersecurity a condition of selling hardware or software in the EU, enforced through CE marking and market surveillance rather than a data-protection-style regulator ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act) ).


### Who's in scope


The unit of regulation is the **"product with digital elements" (PDE)** : any software or hardware product plus its remote data processing solutions, including components placed on the market separately. It is in scope if made available on the EU market in the course of a commercial activity and its intended or reasonably foreseeable use includes a direct or indirect logical or physical data connection to a device or network ([Commission summary](https://digital-strategy.ec.europa.eu/en/policies/cra-summary) ).


Plainly: your on-prem software, desktop agent, SDK, firmware and libraries sold as components are all PDEs. Cloud services come in only as *remote data processing solutions* , meaning processing whose absence would stop the product performing one of its functions. Pure SaaS generally sits outside the CRA and is picked up by NIS2 or DORA instead, where those apply ([DLA Piper, 16 February 2026](https://www.dlapiper.com/en/insights/publications/2026/02/cyber-resilience-act-the-fine-line-between-saas-and-digital-products) ). Four roles carry obligations: **manufacturer** (anyone marketing a PDE under their own name or trademark, whether for payment, monetization or free of charge), **importer** , **distributor** , and the optional **authorised representative** .


**Does this reach a company outside the EU?** Yes. Place software or hardware on the EU market under your own brand and you are the manufacturer, full stop. No EU-establishment trigger, no requirement to appoint an EU entity.


Risk class drives how much paperwork you need, not whether you are in scope:


Class Examples Conformity assessment


Default (most products) Business apps, games, most consumer software Self-assessment (module A)


Important, Class I (Annex III) Password managers, browsers, VPNs, SIEM, identity and privileged-access management, operating systems, routers, antimalware Self-assessment **only** with harmonised standards, common specifications or an EU certification scheme; otherwise notified body


Important, Class II (Annex III) Hypervisors and container runtimes, firewalls, IDS/IPS, tamper-resistant microprocessors and microcontrollers Third-party assessment (or EU certification scheme)


Critical (Annex IV) Hardware devices with security boxes, smart meter gateways, smartcards and other secure elements Third-party assessment (or EU certification scheme)


Sources:[Annexes III and IV](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847) ;[Commission summary](https://digital-strategy.ec.europa.eu/en/policies/cra-summary) ; category descriptions fixed by[Implementing Regulation (EU) 2025/2392](http://data.europa.eu/eli/reg_impl/2025/2392/oj) . Note the trap for security vendors: a SIEM or password manager sits in Class I, so self-assessment is on the table, but only once suitable harmonised standards exist.


### Key dates


Date What happens


10 December 2024[Entered into force](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (20th day after OJ publication on 20 November 2024)


11 June 2026 Chapter IV (Articles 35–51) applies: notifying authorities designated, notified bodies can be appointed


**11 September 2026** **Article 14 reporting applies; ENISA's Single Reporting Platform goes live**


11 December 2027 Full application: essential requirements, CE marking, technical documentation


11 June 2028 Existing EU type-examination certificates covering cybersecurity requirements stop being valid, unless they lapse sooner (Article 69(1))


Sources:[Articles 69 and 71](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847) ;[Commission summary](https://digital-strategy.ec.europa.eu/en/policies/cra-summary) ;[Commission: CRA reporting obligations](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting) ;[ENISA SRP FAQ](https://www.enisa.europa.eu/topics/product-security-and-certification/single-reporting-platform-srp) . Products placed on the market before 11 December 2027 fall under the essential requirements only if **substantially modified** after that date (Article 69(2)). The reporting duty has no such carve-out (Article 69(3)).


### Core obligations


- **Run a cybersecurity risk assessment** per product, let it drive your Annex I implementation, document it, and put it in the technical documentation (Article 13(2)–(4)).
- **Ship with no known exploitable vulnerabilities** and a secure-by-default configuration, resettable to original state (Annex I, Part I(2)(a)–(b)).
- **Produce an SBOM,** software bill of materials, "in a commonly used and machine-readable format covering at the very least the top-level dependencies" (Annex I, Part II(1)). Authorities can request it.
- **Remediate without delay** , ship security updates separately from functionality updates where technically feasible, and publicly disclose fixed vulnerabilities once a fix is out (Annex I, Part II(2) and (4)).
- **Enforce a coordinated vulnerability disclosure (CVD) policy** and publish a contact address covering your product and its third-party components (Annex I, Part II(5)–(6)).
- **Distribute updates securely and free of charge** for a support period of **at least five years** , or the expected use time if that is shorter, and state its end month and year at the point of purchase (Annex I, Part II(7)–(8); Article 13(8) and (19)).
- **Report on the clock** from 11 September 2026: 24-hour early warning, 72-hour notification, then a final report within 14 days of a fix being available (vulnerabilities) or one month after the 72-hour filing (severe incidents), submitted once via ENISA's Single Reporting Platform (Article 14).


### What changes in your security program


First, **product security becomes a market-access function** . The gate is a CE mark and an EU declaration of conformity, which means an auditable artifact chain for every SKU, owned by someone: risk assessment, technical documentation (Annex VII), SBOM, conformity record. And because "no known exploitable vulnerabilities" plus a machine-readable SBOM plus documented third-party due diligence (Article 13(5); recital 34) must all hold at release, dependency scanning now has to produce records you would hand to a regulator.


Second, **your PSIRT gets a regulatory stopwatch** . A product security incident response team that triages weekly will not survive a 24-hour early warning, and the trigger is "becoming aware" of active exploitation, including through threat intelligence and third-party reports rather than only your own detections. You need a named on-call decision-maker with authority to file, a pre-drafted template, and registered platform access *before* the incident.


Third, **open source needs a scope call** . Only free and open-source software (FOSS) **made available on the market in the course of a commercial activity** is in scope. Software not monetized by its manufacturer is not commercial activity, and individual contributors to projects not under their responsibility are out entirely. Foundations giving sustained support to FOSS intended for commercial use become **open-source software stewards** under Article 24, a light-touch regime of a cybersecurity policy, cooperation with authorities and reporting; under Article 64(10)(b) they cannot be fined at all ([Commission: CRA and open source](https://digital-strategy.ec.europa.eu/en/policies/cra-open-source) ).


### Penalties


Article 64 sets three tiers, each "whichever is higher" ([Article 64](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847) ):


Infringement Maximum fine


Annex I essential requirements; Article 13 (manufacturer obligations); Article 14 (reporting) **€15,000,000 or 2.5% of total worldwide annual turnover**


Authorised-representative, importer and distributor duties (Arts. 18–23); declaration of conformity, CE marking, technical documentation and conformity assessment (Arts. 28, 30–32); notified-body duties (Arts. 39, 41, 47, 49, 53) **€10,000,000 or 2%**


Incorrect, incomplete or misleading information to notified bodies or market surveillance authorities **€5,000,000 or 1%**


Two carve-outs sit in Article 64(10): micro and small enterprises cannot be fined for missing the 24-hour early-warning deadline specifically (Article 14(2), point (a); Article 14(4), point (a)), and open-source software stewards cannot be fined at all. Fines sit *on top of* any corrective or restrictive measures market surveillance authorities apply (Article 64(9)), and Article 65 opens the door to collective redress under Directive (EU) 2020/1828.


### Common misconceptions and gotchas


- **"We have until December 2027."** No. Article 14 reporting starts 11 September 2026 and reaches your installed base (Article 69(3)).
- **"CRA is an IoT law."** It is horizontal. Commercial software with no hardware near it is squarely in scope.
- **"We're not established in the EU, so where do we report?"** Article 14(7) sets a cascade. Absent an EU main establishment, you file through the CSIRT of the Member State where your authorised representative sits; failing that your importer's, then your distributor's, then the Member State with most users. Work this out now, not in 24 hours.
- **"We'll wait for the harmonised standards."** They are late. Standardisation request M/606 covers 41 standards ([Commission: CRA standardisation](https://digital-strategy.ec.europa.eu/en/policies/cra-standardisation) ), and none had been cited in the Official Journal as of this writing. In early July 2026 the Commission published a draft amendment pushing the 2026 delivery dates back roughly two months: 31 October 2026 for type A and vulnerability-management type B, 31 December 2026 for type C, with the implementing decision not yet published ([IBF Solutions, 6 July 2026](https://www.ibf-solutions.com/en/news-and-knowledge/technical-papers-and-news-on-ce-marking/current-status-of-standardisation-for-the-cyber-resilience-act) ). Standards only give a *presumption* of conformity anyway; you can comply by documenting your own technical measures.
- **"Reporting is just an email."** It is a registered account. The Single Reporting Platform requires an EU Login, and your right to file for a manufacturer is validated by the coordinating CSIRT after first access. ENISA has confirmed no APIs at launch, so bulk automated filing is out ([ENISA SRP FAQ, 17 July 2026](https://www.enisa.europa.eu/topics/product-security-and-certification/single-reporting-platform-srp) ).
- **"Security updates can be a paid support tier."** Not under Annex I, Part II(8): free of charge for the support period, with a narrow exception for tailor-made products.
- **"Every patch means a new conformity assessment."** No. A security update that reduces risk without changing intended purpose is not a substantial modification (recital 39). A feature update that changes intended function can be.


### Do this next


1. **Build the product inventory and classify it.** Every PDE you place on the EU market, mapped to default / Class I / Class II / critical, with an accountable owner. Everything else depends on this.
2. **Stand up an SBOM pipeline** emitting machine-readable SBOMs (CycloneDX or SPDX) covering at least top-level dependencies, per release, retained as evidence.
3. **Publish a CVD policy and security contact** , and make sure intake reaches a real triage rotation. Annex I, Part II(5) requires you to *enforce* the policy; a page on the website is only the start.
4. **Get reporting-ready before 11 September 2026.** Determine your coordinating CSIRT under Article 14(7), create EU Login accounts for two or three named filers, draft the 24h/72h/final templates against ENISA's field list, and run one tabletop against the clock.
5. **Set and publish support periods.** Fix an end date per product, confirm it clears the five-year floor, and surface it at purchase and in the technical documentation.


SAST/SCA, SBOM generation and automated patching tooling substantially covers these obligations.


## The Radio Equipment Directive (RED): the cybersecurity deadline that already passed


**Skip this section if you don't ship hardware.** RED covers physical products that emit or receive radio waves; pure SaaS (software as a service) is out of scope. If you sell anything wireless in Europe, keep reading. This one already bit.


### In one sentence


The Radio Equipment Directive ([Directive 2014/53/EU](https://eur-lex.europa.eu/eli/dir/2014/53/oj/eng) ) is the CE-marking law for wireless products, and since 1 August 2025 three of its cybersecurity clauses have been mandatory: Article 3(3)(d) network protection, (e) personal data and privacy, (f) protection from fraud.[Delegated Regulation (EU) 2022/30](https://eur-lex.europa.eu/eli/reg_del/2022/30/oj/eng) is what switched them on.


### Who's in scope


Article 1 of the Delegated Regulation splits scope three ways:


Requirement Applies to


**3(3)(d)** network protection Radio equipment that can itself communicate over the internet, **directly or via any other equipment**


**3(3)(e)** data and privacy Internet-connected equipment, plus childcare-only equipment, toys under Directive 2009/48/EC, and wearables, in each case where the device can process personal, traffic, or location data


**3(3)(f)** fraud Internet-connected equipment that lets the holder or user transfer money, monetary value, or virtual currency


In practice: home alarms, smart cameras and locks, thermostats, baby monitors, fitness trackers, connected toys, routers, wireless payment terminals ([BSI](https://www.bsigroup.com/siteassets/pdf/en/products-and-services/gl-rs-pcert-lg-ict-dt-nsp-mp-digitaldomain-0025-red-guide-and-readiness.pdf) ). Note the phrase "via any other equipment": a Bluetooth sensor that reaches the internet only through a phone app is *indirectly* connected, and in scope. Recital 8 of 2022/30 adds that RED draws no line between radio and non-radio functions, so all aspects and parts of the device have to comply.


[Article 2](https://eur-lex.europa.eu/eli/reg_del/2022/30/oj/eng) carves out equipment also covered by the Medical Devices ((EU) 2017/745) or in vitro diagnostic ((EU) 2017/746) Regulations. For civil aviation ((EU) 2018/1139), vehicle type-approval ((EU) 2019/2144), and electronic road tolling (Directive (EU) 2019/520), only (e) and (f) switch off. Point (d) still applies to cars, aircraft, and toll tags.


**Does this reach a company outside the EU?** Yes. RED bites on *placing on the market* , defined as the first making available of radio equipment on the Union market ([Art. 2(1)(10)](https://eur-lex.europa.eu/eli/dir/2014/53/oj/eng) ), wherever you're incorporated.[Regulation (EU) 2019/1020, Art. 4](https://eur-lex.europa.eu/eli/reg/2019/1020/oj/eng) separately requires an economic operator established in the Union to hold your declaration of conformity for market surveillance authorities and produce the technical documentation on request.


### Key dates


Date What happens


1 August 2024 Original application date, postponed a year by[Delegated Regulation (EU) 2023/2444](https://eur-lex.europa.eu/eli/reg_del/2023/2444/oj/eng)


30 January 2025 EN 18031-1/-2/-3:2024 cited in the Official Journal, with restrictions ([Decision (EU) 2025/138](https://eur-lex.europa.eu/eli/dec_impl/2025/138/oj/eng) )


**1 August 2025** **Requirements became mandatory. The date held.**


10 December 2027 Last day 2022/30 applies


11 December 2027 2022/30 repealed ([Delegated Regulation (EU) 2026/339](https://eur-lex.europa.eu/eli/reg_del/2026/339/oj/eng) ); the Cyber Resilience Act applies in full


### Core obligations


Conformity runs through three harmonised standards: **EN 18031-1** (network protection), **EN 18031-2** (data and privacy, covering childcare, toys, and wearable equipment), **EN 18031-3** (fraud). The mechanism families common to all three are access control, authentication, secure update, secure storage, secure communication, confidential cryptographic keys, and cryptography. Each part adds more: resilience and traffic control on the network side, logging and user notification on the data side. A product-level cybersecurity risk assessment goes in the technical file ([BSI](https://www.bsigroup.com/siteassets/pdf/en/products-and-services/gl-rs-pcert-lg-ict-dt-nsp-mp-digitaldomain-0025-red-guide-and-readiness.pdf) ).


**The restrictions matter more than the standards.** EN 18031 was cited *with restrictions* , and each one carries a conformity-route consequence ([Decision (EU) 2025/138, Annex](https://eur-lex.europa.eu/eli/dec_impl/2025/138/oj/eng) ;[Commission DG GROW guidance](https://www.vdma.eu/documents/d/group-34568/com-red-hs-cyber-guidance_v1) ):


- The "rationale" and "guidance" sections are informative only: no presumption of conformity, and no notified body needed either.
- Using clauses 6.2.5.1/6.2.5.2 to let a user **set no password at all** forfeits presumption. Decline that option and you stay self-assessed.
- In EN 18031-2, the access-control clauses 6.1.3 to 6.1.6 forfeit presumption if parental or guardian access control isn't ensured.
- In EN 18031-3, **clause 6.3.2.4 (secure-update assessment criteria) confers no presumption whatever your design** , so a third-party conformity assessment is mandatory.


[RED Art. 17(3)](https://eur-lex.europa.eu/eli/dir/2014/53/oj/eng) lets you self-assess through internal production control (Annex II) where you've applied the published harmonised standards. Art. 17(4) forces a notified body where you haven't applied them, or applied them only in part: EU-type examination (Annex III) or conformity based on full quality assurance (Annex IV). When the Commission issued its guidance, only 24 notified bodies were competent for RED cybersecurity ([DG GROW](https://www.vdma.eu/documents/d/group-34568/com-red-hs-cyber-guidance_v1) ), so check the current list on the[Commission's RED page](https://single-market-economy.ec.europa.eu/sectors/electrical-and-electronic-engineering-industries-eei/radio-equipment-directive-red_en) ; lead times are real.


### What changes in your security program


This is a CE-marking problem wearing a security costume. The evidence an authority asks for (technical file, declaration of conformity, risk assessment, test results) belongs to your regulatory and product teams, not to the security operations center. Firmware builds need signing and an auditable update path. Shared default passwords are out, and letting the user run with no password at all costs you the presumption of conformity. Someone has to own re-assessment when a release changes security-relevant behavior.


### Penalties


No EU-wide fine schedule exists. RED is a directive, so[Article 46](https://eur-lex.europa.eu/eli/dir/2014/53/oj/eng) leaves penalties to the Member States: they must be "effective, proportionate and dissuasive," and may include criminal penalties for serious infringements. What you'd pay depends on which national law catches you. The sanction that really bites is losing the right to CE mark, and with it product withdrawal, recall, seizure, and market bans ([BSI](https://www.bsigroup.com/siteassets/pdf/en/products-and-services/gl-rs-pcert-lg-ict-dt-nsp-mp-digitaldomain-0025-red-guide-and-readiness.pdf) ).


### Common misconceptions and gotchas


- **"We're not internet-connected."** Indirect connection through a phone or hub counts.
- **"Only the radio part is regulated."** Recital 8 says the whole product is.
- **"EN 18031 is voluntary, so we can ignore it."** The standards are voluntary; the essential requirements are not. Skipping the standards means a notified body.
- **"The CRA replaces this, so we can wait."** No.[Delegated Regulation (EU) 2026/339](https://eur-lex.europa.eu/eli/reg_del/2026/339/oj/eng) , adopted on 16 February 2026 and published on 29 April 2026, repeals 2022/30, but only with effect from 11 December 2027, the date the[Cyber Resilience Act (Regulation (EU) 2024/2847)](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) starts to apply in full (Art. 71). The transfer of cybersecurity requirements from RED to the CRA was agreed back in 2023 ([Commission RED page](https://single-market-economy.ec.europa.eu/sectors/electrical-and-electronic-engineering-industries-eei/radio-equipment-directive-red_en) ). What governs is the date of placing on the market: recital 5 of 2026/339 confirms authorities keep policing RED compliance for equipment placed on the EU market between 1 August 2025 and 10 December 2027, repeal or no repeal. Recital 3 adds that the CRA's Annex I covers every element of RED Article 3(3)(d), (e) and (f), so your engineering work carries forward even as the legal basis changes.


### Do this next


1. **Draw the scope line.** Per SKU with a radio: does it reach the internet, directly or indirectly; can it process personal, traffic, or location data; can it move money? Check the Article 2 carve-outs before exempting anything.
2. **Gap-assess against EN 18031** , part by part. Start with default credentials, the update mechanism, and key storage, where consumer hardware fails most often.
3. **Pick your conformity route now.** If a restriction bites, especially EN 18031-3 clause 6.3.2.4, book a notified body competent for Articles 3.3(d)/(e)/(f) early.
4. **Make firmware and update security a release gate,** with evidence collected into the technical file as you go, so it carries forward to the CRA.


## DORA: the Digital Operational Resilience Act


### In one sentence


DORA (Regulation (EU) 2022/2554) is the EU's directly binding rulebook for how financial firms manage information and communication technology (ICT) risk, and it reaches through them to the vendors they depend on.


### Who's in scope


The scope goes well past banks. Article 2(1) lists **twenty** categories of "financial entity" at points (a) to (t): banks and insurers, yes, but also insurance intermediaries, investment firms, payment and e-money institutions, crypto-asset service providers, fund managers, trading venues, central counterparties (CCPs), central securities depositories (CSDs), credit rating agencies, administrators of critical benchmarks, crowdfunding platforms and occupational pension institutions ([DORA](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022R2554) ). Point (u) adds ICT third-party service providers, who are in scope without being "financial entities."


**Does this reach a company outside the EU?** Yes, twice over. Sell ICT services to an EU financial entity and DORA's contract regime (Art. 30) lands on your paper via your customer, wherever you're headquartered. And if you matter systemically to the sector, the European Supervisory Authorities (ESAs) can designate you a **critical ICT third-party provider (CTPP)** and supervise you directly. After that, Art. 31(12) gives you 12 months to establish an EU subsidiary, or your EU customers must stop using you.


### Key dates


Date What happened / happens


17 January 2025 Applies in full, every Member State, no transition period (Art. 64)


18 November 2025 First CTPP list published: 19 providers ([EBA](https://www.eba.europa.eu/publications-and-media/press-releases/european-supervisory-authorities-designate-critical-ict-third-party-providers-under-digital) )


31 March 2026 National authorities forward Registers of Information to the ESAs; reference date 31 December 2025 ([BaFin FAQ](https://www.bafin.de/SharedDocs/FAQs/DE/DORA/Informationsregister_und_Anzeigepflichten/1_Allgemeines/01.html) ). Firm-level windows are national: Luxembourg's ran 11 February–31 March ([CSSF](https://www.cssf.lu/en/2026/02/dora-submission-timeframe-for-register-of-information-edesk-portal-open-as-of-11-february-2026/) ), Germany's 9–30 March ([BaFin](https://www.bafin.de/DE/unternehmen-maerkte/aufsicht/alle-unternehmen/dora/Informationsregister_und_Anzeigepflichten/Informationsregister_und_Anzeigepflichten_node.html) )


~18 November 2026 Third-country CTPPs designated in 2025 need an EU subsidiary in place (Art. 31(12))


### Core obligations: the five pillars


DORA's substance sits in Chapters II to VI:


1. **ICT risk management.** A documented framework that the management body defines, approves and answers for (Art. 5(2)).
2. **Incident management, classification and reporting.** Classify against[Delegated Regulation (EU) 2024/1772](https://eur-lex.europa.eu/eli/reg_del/2024/1772/oj) , then report on the clock below.
3. **Resilience testing.** Entities other than microenterprises must test every system supporting a critical or important function **at least yearly** (Art. 24(6)), drawing on the Art. 25(1) menu: vulnerability assessments, open-source analysis, "source code reviews where feasible," scenario-based tests, penetration testing.
4. **ICT third-party risk management.** A Register of Information covering every ICT contractual arrangement (Art. 28(3)), plus the mandatory contract content in Art. 30.
5. **Information sharing.** Permissive, not mandatory: you *may* exchange threat intelligence with other financial entities (Art. 45).


The reporting clock comes from[Delegated Regulation (EU) 2025/301, Art. 5](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202500301) :


Report Deadline


Initial notification Within **4 hours** of classifying the incident as major, and no later than **24 hours** from becoming aware of it


Intermediate Within **72 hours** of the initial notification, even if nothing has changed


Final No later than **one month** after the intermediate (or latest updated intermediate) report


Weekend and bank-holiday relief exists (Art. 5(4)), but it does not cover initial or intermediate reports from credit institutions, CCPs, trading venue operators, or firms also classed as essential or important entities under NIS2 (Art. 5(5)).


Threat-led penetration testing (TLPT) is the advanced tier, and you don't opt into it: competent authorities identify which financial entities have to perform it (Art. 26(8)). Those entities, excluding microenterprises and the Art. 16(1) group, must run TLPT at least every three years on live production systems, at a frequency their supervisor can raise or lower (Art. 26(1)).


### What changes in your security program


DORA rarely asks for new controls. It asks for the ones you already run to be evidenced, timed and inventoried. Two things tend to break in practice.


First, **your vendor inventory becomes a regulatory filing.** The Register is kept per contractual arrangement, and it's where supervisory review starts. In the ESAs' 2024 dry run, only 6.5% of the registers submitted by almost 1,000 financial entities passed every data-quality check; 116 checks were applied ([ESAs, via EIOPA](https://www.eiopa.europa.eu/esas-dry-run-exercise-shows-goal-reporting-registers-information-under-digital-operational-2024-12-17_en) ). The 2026 cycle applied the same rules to more fields, so a register accepted in 2025 could be rejected in 2026 ([CSSF](https://www.cssf.lu/en/2026/02/dora-submission-timeframe-for-register-of-information-edesk-portal-open-as-of-11-february-2026/) ).


Second, **four hours is a design constraint, not a policy line.** Classification has to run fast enough for the clock to be survivable, and someone with authority has to be reachable on a Sunday.


The volumes are real. BaFin alone logged 525 serious ICT incident reports in the first three quarters of 2025, about 70% from credit institutions, and notes that ordinary IT failures such as faulty updates still outnumber successful cyberattacks ([BaFin](https://www.bafin.de/EN/die-bafin/publikationen-daten/risiken-im-fokus/Fokusrisiken_2026/Druckansicht/Fokusrisiken_2026_druck_node_en.html) ).


### Penalties


Be precise here, because most summaries are wrong. **DORA sets no EU-wide maximum fine for financial entities.** Art. 50(3) obliges Member States to set penalties that are "effective, proportionate and dissuasive." Art. 50(4) fixes the minimum toolkit: cease-and-desist orders, forced cessation of a practice, measures "of a pecuniary nature," telecom traffic records, public naming. The euro figure belongs to national law, so it depends on who supervises you.


The widely quoted "€10 million or 2% of worldwide turnover" is not DORA. It comes from[NIS2, Art. 34(4)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022L2555) , where essential entities breaching Articles 21 or 23 face fines "of a maximum of **at least** EUR 10 000 000 or … at least 2 % of the total worldwide annual turnover … whichever is higher." That "at least" makes it a floor on the maximum rather than a cap, and it gets copied onto DORA explainers by mistake.


For CTPPs, DORA does set numbers. Where a CTPP won't comply with the Lead Overseer's information requests, investigations or remediation reports (Art. 35(1)(a)–(c)) and 30 calendar days have passed, Art. 35(6)–(8) allows a **periodic penalty payment charged daily for up to six months, of up to 1% of the CTPP's average daily worldwide turnover in the preceding business year** . That lever is not available when a CTPP simply ignores the Lead Overseer's *recommendations* (Art. 35(1)(d)). For those, the sharpest tool isn't financial: as a last resort, authorities can require financial entities to suspend or terminate the service (Art. 42(6)). CTPPs also pay oversight fees covering the Lead Overseer's costs (Art. 43), and opting in to designation costs a fixed €50,000 fee under Delegated Regulation (EU) 2024/1505 ([EIOPA](https://www.eiopa.europa.eu/digital-operational-resilience-act-dora/dora-oversight_en) ).


### Common misconceptions and gotchas


- **"We're too small."** Size buys proportionality, not exemption. Articles 5 to 15 switch off only for the narrow Art. 16(1) list: small and non-interconnected investment firms, exempted payment, e-money and credit institutions, and small pension institutions. Even they must run a simplified but documented framework.
- **"We're a non-EU vendor, so it's our customer's problem."** Your customer can't sign a compliant contract unless you accept audit and inspection rights for the financial entity, its appointed third party, the competent authority *and* the Lead Overseer (Art. 30(3)(e)), plus subcontracting and location disclosure (Art. 30(2)(a)–(b)) and an exit-transition period (Art. 30(3)(f)).
- **"CTPP designation only hits hyperscalers."** The[first list](https://www.eba.europa.eu/sites/default/files/2025-11/e388451b-356b-408a-bbf2-b8e425865d75/List%20of%20designated%20CTPPs.pdf) spans consultancies, telecoms, data providers and IT services firms alongside cloud providers, and the ESAs re-publish it yearly (Art. 31(9)).
- **"Third-country branches are out."** Not necessarily. Following[Q&A #102](https://www.eiopa.europa.eu/qa-regulation/questions-and-answers-database/dora102-3097_en) in the ESAs' database, the CSSF now requires third-country branches of supervised entity types to file a Register. That Q&A answers differently for credit institutions and insurers than for insurance intermediaries, so check your own supervisor's position.


### Do this next


**If you're a financial entity:**


1. Reconcile the Register against your real contract set at the 31 December 2025 reference date: one row per arrangement, valid Legal Entity Identifiers (LEIs), before the next window opens.
2. Rehearse the four-hour clock end to end (detection, classification, submission), including a weekend scenario.
3. Confirm annual testing covers every critical-or-important-function system, remediation tracked to closure.
4. Check whether you're in the TLPT population and when your three-year cycle falls due.


**If you're an ICT vendor selling to EU financial entities:**


1. Pre-write your Art. 30 clause pack: audit and inspection rights (including for authorities and the Lead Overseer), incident-notification support, service-level targets, exit-transition period. Then you stop renegotiating deal by deal.
2. Publish your subcontracting chain and service locations; your customer needs both for its register.
3. Build a resilience evidence pack: test results, incident history, continuity evidence, certifications.
4. Assess your CTPP exposure honestly. If designation is plausible, plan the EU-subsidiary question now, not in the 12 months after a designation letter.


## Also on your radar: CER, the Cybersecurity Act, and the Cyber Solidarity Act


Three more EU instruments come up constantly in board decks and vendor pitches, and all three are routinely oversold. None of them will land on your desk as a hard compliance deadline the way NIS2, DORA, or the CRA will. But each has a specific trigger that flips it from background noise to must-act, and knowing that trigger is the point of this chapter.


---


### 5A. CER Directive: Directive (EU) 2022/2557


**In one sentence.** The CER (Critical Entities Resilience) Directive is the physical-resilience twin of NIS2: can you keep delivering an essential service through sabotage, natural hazard, insider threat or a public health emergency? Not, is your network secure ([European Commission](https://home-affairs.ec.europa.eu/policies/internal-security/counter-terrorism-and-radicalisation/protection/critical-infrastructure-resilience_en) ).


**Who's in scope.** Eleven sectors: energy, transport, banking, financial market infrastructure, health, drinking water, wastewater, digital infrastructure, public administration, space, and food production, processing and distribution ([CER Directive, Annex](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ). Ten of them mirror NIS2's Annex I "sectors of high criticality"; food is the odd one out, in NIS2's Annex II ([NIS2 Annexes](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ). A designated critical entity automatically counts as an *essential* entity under NIS2, whatever tier it would otherwise have landed in ([NIS2, Art. 3(1)(f)](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ;[DLA Piper](https://privacymatters.dlapiper.com/2026/07/eu-cer-directive-enters-a-new-phase-as-critical-entity-designation-deadline-arrives/) ).


There's no size threshold, so a small regional water operator can be designated while a much larger software vendor isn't. In most member states you're in scope only once your national authority identifies you, on the test of whether an incident at your infrastructure would significantly disrupt an essential service ([Art. 6](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ;[DLA Piper](https://privacymatters.dlapiper.com/2026/07/eu-cer-directive-enters-a-new-phase-as-critical-entity-designation-deadline-arrives/) ). Germany and the Czech Republic are expected to run self-assessment instead. Serve six or more member states and you also become an entity "of particular European significance" ([Art. 17](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ).


**Key dates.**


Date What happens


16 January 2023 Directive entered into force ([EUR-Lex](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ;[European Commission](https://home-affairs.ec.europa.eu/policies/internal-security/counter-terrorism-and-radicalisation/protection/critical-infrastructure-resilience_en) )


17 October 2024 Transposition deadline. Twenty-four of 27 missed it ([Industrial Cyber](https://industrialcyber.co/regulation-standards-and-compliance/european-commission-adopts-infringement-decisions-against-member-states-for-not-transposing-security-directives/) ); most have notified since ([European Commission](https://ec.europa.eu/commission/presscorner/detail/en/ip_26_910) )


28 April 2026 Commission refers seven laggards (Bulgaria, France, Luxembourg, the Netherlands, Poland, Spain, Sweden) to the Court of Justice, seeking financial sanctions ([IP/26/910](https://ec.europa.eu/commission/presscorner/detail/en/ip_26_910) )


**17 July 2026** Deadline to identify critical entities; each one must be told within a month ([Art. 6(1), 6(3)](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) )


That last date has just passed, and designation letters are arriving now, unevenly, member state by member state ([DLA Piper](https://privacymatters.dlapiper.com/2026/07/eu-cer-directive-enters-a-new-phase-as-critical-entity-designation-deadline-arrives/) ).


**Core obligations once designated.** One check first: if you were designated in banking, financial market infrastructure or digital infrastructure, your member state must tell you Chapters III and IV don't apply unless national law says otherwise ([Art. 6(3)](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ). DORA and NIS2 already cover that ground. For everyone else:


- An all-hazards risk assessment covering cross-sectoral and cross-border dependencies, within nine months of notification and every four years after ([Art. 12](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ;[WTW](https://www.wtwco.com/en-gb/insights/2025/09/are-you-ready-to-comply-with-the-cer-directives-resilience-requirements) ).
- Proportionate technical, security and organisational measures (prevention, physical protection of premises, response, recovery, employee security) written into a resilience plan, with a named contact for the authority ([Art. 13](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ;[Osborne Clarke](https://www.osborneclarke.com/insights/eu-critical-entities-resilience-directive-what-impact-your-organisation) ).
- Background checks in duly reasoned cases for sensitive roles, on conditions each member state sets ([Art. 14](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ).
- Incident notification: initial report within 24 hours of becoming aware, detailed report within a month of that ([Art. 15](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ).
- Penalties are national. Article 22 asks only that they be "effective, proportionate and dissuasive," so the number depends on your member state's transposing law ([Art. 22](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) ).


One simplification is in flight: the Digital Omnibus proposal of 19 November 2025 would route CER and NIS2 incident reports through a single ENISA-operated entry point ([EPRS](https://epthinktank.eu/2026/01/05/cybersecurity-act-review-what-to-expect/) ;[Bird & Bird](https://www.twobirds.com/en/insights/2025/digital-omnibus-package-single-eu-harmonised-incident-reporting-regime-across-cyber-and-data-protect) ). A routing change, not a lighter test.


**Does this reach a company outside the EU?** Only indirectly. Designation attaches to the operator of the EU infrastructure, so a parent company anywhere else in the world is caught through its EU subsidiaries running essential services, never through its home operations.


**Why it's radar-tier.** In most member states you can't opt in, and you owe nothing until a national authority identifies you. The clock is short once it does.


> **When to care:** the day a designation letter arrives, because you then have roughly ten months to stand up a resilience program from scratch.


---


### 5B. EU Cybersecurity Act: Regulation (EU) 2019/881


**In one sentence.** The Cybersecurity Act gave ENISA a permanent mandate and created the European cybersecurity certification framework (ECCF): a set of voluntary EU-wide certification schemes for ICT products, services and processes that is slowly turning into a procurement expectation ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cybersecurity-act) ;[EPRS](https://epthinktank.eu/2026/01/05/cybersecurity-act-review-what-to-expect/) ).


**Who's in scope.** No one is obliged to certify. It matters to vendors and service providers selling ICT products, services or processes into the EU and, since the 2025 amendment, to managed security service providers doing incident response, penetration testing, security audits and consultancy ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cybersecurity-act) ;[EPRS](https://epthinktank.eu/2026/01/05/cybersecurity-act-review-what-to-expect/) ).


**Key dates.**


Date What happens


27 June 2019 Regulation (EU) 2019/881 enters into force; ENISA gets a permanent mandate ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2019/881/oj/eng) )


4 February 2025 Regulation (EU) 2025/37 enters into force, extending the framework to managed security services. It was published in the Official Journal on 15 January 2025 ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2025/37/oj/eng) ;[European Commission](https://digital-strategy.ec.europa.eu/en/policies/cybersecurity-act) )


27 February 2025 The EUCC scheme becomes applicable under Implementing Regulation (EU) 2024/482 ([EUR-Lex](https://eur-lex.europa.eu/eli/reg_impl/2024/482/oj/eng) )


20 January 2026 Commission proposes a revised Cybersecurity Act, COM(2026) 11, alongside targeted NIS2 amendments ([European Commission](https://digital-strategy.ec.europa.eu/en/library/proposal-regulation-eu-cybersecurity-act) )


**Where the schemes actually stand.** Exactly one has been adopted: **EUCC** , the Common Criteria–based scheme for ICT products. **EUCS** (cloud), **EU5G** , the digital identity wallet scheme and **EUMSS** (managed security services) are all still under development ([EPRS](https://epthinktank.eu/2026/01/05/cybersecurity-act-review-what-to-expect/) ;[ENISA certification library](https://certification.enisa.europa.eu/index_en) ). EUCS has been stuck for years on whether to include sovereignty criteria such as data localisation and EU-based corporate headquarters. Member states are divided; Microsoft, Amazon and Google argue the criteria are non-technical, do nothing for security outcomes and restrict market access, while "cloud by Europe" advocates argue they are essential to strategic autonomy ([EPRS](https://epthinktank.eu/2026/01/05/cybersecurity-act-review-what-to-expect/) ).


**Why voluntary doesn't mean irrelevant.**


- NIS2 Article 24 lets member states *require* essential and important entities to use ICT products, services or processes certified under an ECCF scheme, and lets the Commission specify by delegated act which categories of entities must do so ([NIS2, Art. 24](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ).
- CRA Article 27 grants a presumption of conformity to products certified under a recognised European scheme such as EUCC, so certification can become the cheapest route to CRA compliance ([Cyber Resilience Act, Art. 27](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) ;[EPRS](https://epthinktank.eu/2026/01/05/cybersecurity-act-review-what-to-expect/) ).
- The January 2026 revision would speed the machinery up (schemes developed within 12 months by default) and add a trusted ICT supply-chain security framework aimed at third-country suppliers ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cybersecurity-act) ).


**Does this reach a company outside the EU?** Yes, commercially rather than legally. A non-EU vendor may need an EU certificate to stay eligible for EU enterprise and public-sector deals, and the EUCS sovereignty debate is the specific thing to watch, because sovereignty criteria would exclude some non-EU-controlled providers outright.


**Why it's radar-tier.** Certification is voluntary today and only one scheme is live.


> **When to care:** the first time an EU customer's RFP names a certification scheme, or the day a member state uses NIS2 Article 24 to mandate certified products in your sector.


---


### 5C. Cyber Solidarity Act: Regulation (EU) 2025/38


**In one sentence.** The Cyber Solidarity Act builds EU-level detection and response capacity (shared cross-border threat detection, a funded emergency response capability, and post-incident reviews) and creates essentially no obligations for private companies ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cyber-solidarity) ).


**Who's in scope.** Member States' crisis management authorities and their CSIRTs (computer security incident response teams), EU institutions and agencies, and ENISA as operator. Private security providers are involved only if they choose to bid in as Reserve suppliers ([ENISA](https://www.enisa.europa.eu/topics/eu-incident-response-and-cyber-crisis-management/eu-cybersecurity-reserve) ).


**Key dates.**


Date What happens


15 January 2025 Regulation (EU) 2025/38 published in the Official Journal ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2025/38/oj/eng) )


4 February 2025 Enters into force ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2025/38/oj/eng) ;[European Commission](https://digital-strategy.ec.europa.eu/en/policies/cyber-solidarity) )


26 August 2025 Commission and ENISA sign the contribution agreement handing ENISA operation of the EU Cybersecurity Reserve, with €36 million over three years under the Digital Europe Work Programme 2025–2027 ([ENISA](https://www.enisa.europa.eu/news/enisa-to-operate-the-eu-cybersecurity-reserve-with-eur-36-million) )


**The three pillars.**


1. **European Cybersecurity Alert System** : a network of national and cross-border Cyber Hubs (formerly "SOCs") using AI and data analytics to detect threats and share cross-border warnings ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cyber-solidarity) ).
2. **Cybersecurity Emergency Mechanism** : preparedness testing of entities in sectors such as finance, energy and healthcare; the **EU Cybersecurity Reserve** of pre-contracted incident-response services from vetted "trusted providers," deployable at the request of member states, EU bodies or associated third countries; and mutual assistance between member states ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cyber-solidarity) ).
3. **European Cybersecurity Incident Review Mechanism** : ENISA reviews specific significant or large-scale incidents at the request of the Commission or EU-CyCLONe, the EU's cyber crisis liaison network, and publishes lessons learned and recommendations ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cyber-solidarity) ).


**How providers get into the Reserve.** Services are procured from trusted managed security service providers through public procurement calls run by ENISA, which also assesses incoming support requests from member state crisis authorities, CSIRTs and CERT-EU ([ENISA](https://www.enisa.europa.eu/news/enisa-to-operate-the-eu-cybersecurity-reserve-with-eur-36-million) ). Every selected provider has passed an **ownership control assessment** establishing whether it is directly or indirectly controlled by member states, their nationals, or entities and nationals of specified eligible countries ([ENISA](https://www.enisa.europa.eu/topics/eu-incident-response-and-cyber-crisis-management/eu-cybersecurity-reserve) ).


**Does this reach a company outside the EU?** Negligibly as an obligation. As an opportunity, check eligibility before you invest in a bid: the ownership control assessment turns on EU or eligible-country control of the provider, which is a hard test for most providers controlled from outside the EU to meet on their own ([ENISA](https://www.enisa.europa.eu/topics/eu-incident-response-and-cyber-crisis-management/eu-cybersecurity-reserve) ).


**Why it's radar-tier.** It changes how the EU and your national authorities respond around you; it changes nothing you must do.


> **When to care:** if you sell incident response into Europe and want a Reserve contract, or if you want to understand who will actually show up when a cross-border incident hits your sector.


---


One more instrument belongs on the radar for most security teams, and it earns its own chapter: the AI Act.


## The EU AI Act: what it adds to your security program


### In one sentence


Regulation (EU) 2024/1689 (the "AI Act") is the EU's risk-tiered product-safety law for AI. What it asks of a security program is short and specific: adversarial resilience, logging, incident reporting.


### The 30-second structure


Four tiers. Prohibited practices. High-risk systems, the heavy tier: standalone Annex III uses (recruitment, credit scoring, critical infrastructure) plus AI as a safety component of an Annex I product. Transparency-only systems under Article 50. And **general-purpose AI (GPAI) models** , with a stricter sub-tier for systemic risk. Providers carry the engineering burden; deployers, oversight and logging.


**Does this reach companies outside the EU?** Yes, twice over. Article 2(1)(a) catches providers placing AI systems or GPAI models on the EU market whatever their establishment; Article 2(1)(c) catches third-country providers and deployers "where the output ... is used in the Union" ([Art. 2](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng#art_2) ).


### Key dates, and the July 2026 reset


Date What applies


1 August 2024 Enters into force


2 February 2025 Prohibitions (Art. 5), AI literacy (Art. 4)


2 August 2025 GPAI model obligations (Arts. 51–56); EU governance


2 August 2026 General application date; most of Art. 50 transparency; enforcement begins


**2 December 2027** High-risk obligations, standalone **Annex III** systems


**2 August 2028** High-risk obligations, **Annex I** embedded systems (machinery products now largely carved out)


The last two rows moved this month. Regulation (EU) 2026/1744, the "Digital Omnibus on AI," entered into force **27 July 2026** , amending Article 113 so Chapter III, Sections 1 to 3 apply from **2 December 2027** (Annex III) and **2 August 2028** (Annex I). The Commission's proposed harmonised-standards trigger did not survive, so these dates are fixed ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2026/1744/oj/eng) ;[Freshfields](https://www.freshfields.com/en/our-thinking/blogs/technology-quotient/eu-ai-act-unpacked-34-the-final-digital-omnibus-on-ai-key-amendments-to-the-a-102nber) ;[Gibson Dunn](https://www.gibsondunn.com/eu-ai-act-omnibus-agreement-postponed-high-risk-deadlines-and-other-key-changes/) ).


### Article 15, the requirement you engineer for


High-risk systems must achieve "an appropriate level of accuracy, robustness, and cybersecurity" and perform consistently in those respects throughout their lifecycle (Art. 15(1)). Paragraph 5 is the security duty: resilience against *unauthorised* third parties altering a system's use, outputs or performance by exploiting its vulnerabilities. Its third subparagraph requires measures, where appropriate, against training-data poisoning, model poisoning of pre-trained components, adversarial examples or model evasion, confidentiality attacks, and model flaws. Paragraph 4 adds resilience to errors and faults, permits redundancy and fail-safe designs, and requires continuously-learning systems to control **feedback loops** ([Art. 15](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng#art_15) ).


### The Cyber Resilience Act overlap: real relief, narrower than advertised


Article 12(1) of the **Cyber Resilience Act (CRA)** deems a product with digital elements that is also high-risk under AI Act Article 6 compliant with "the cybersecurity requirements set out in Article 15" on three cumulative conditions: CRA Annex I Part I for the product, Annex I Part II for the manufacturer's processes, and the Article 15 protection level demonstrated in the CRA EU declaration of conformity. Reg. (EU) 2026/1744 mirrors this inside the AI Act as new **Article 42(3)** . Both are bounded identically, "without prejudice to the requirements relating to accuracy and robustness." One leg of Article 15, not three ([CRA Art. 12](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng#art_12) ).


### GPAI with systemic risk (Article 55)


In force since 2 August 2025. Providers must evaluate models against state-of-the-art protocols, "including conducting and documenting adversarial testing"; assess and mitigate Union-level systemic risks; keep track of, document and report serious incidents "without undue delay" to the AI Office and, as appropriate, national authorities; and ensure "an adequate level of cybersecurity protection" for the model and its physical infrastructure (Art. 55(1)(a)–(d)). Article 56 codes of practice demonstrate compliance until a harmonised standard is published; only the standard grants a presumption of conformity ([Art. 55](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng#art_55) ).


### Deployer duties and incident clocks


Deployers assign human oversight to people with "the necessary competence, training and authority, as well as the necessary support," and monitor operation "on the basis of the instructions for use." If compliant use may still leave the system presenting an Article 79(1) risk, they tell the provider or distributor and the market surveillance authority without undue delay, and **suspend use** . Logs under their control are kept **at least six months** ([Art. 26](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng#art_26) ).


Serious incidents go to the market surveillance authorities of the Member State where they occurred **immediately** once a causal link, or its reasonable likelihood, is established, and within **15 days** of awareness. The limit drops to **2 days** for a widespread infringement or an Article 3(49)(b) incident (serious, irreversible disruption of the management *or* operation of critical infrastructure), and **10 days** if someone has died ([Art. 73](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng#art_73) ).


### Penalties (Article 99)


Fine ceiling (whichever is **higher** ) Trigger


Up to €35,000,000 or 7% of total worldwide annual turnover Prohibited practices (Art. 5)


Up to €15,000,000 or 3% Providers (Art. 16), deployers (Art. 26), authorised representatives, importers, distributors, notified bodies; Art. 50 transparency


Up to €7,500,000 or 1% Incorrect, incomplete or misleading information to notified bodies or authorities on request


For SMEs, including start-ups, the cap is whichever of the two is **lower** (Art. 99(6)) ([Art. 99](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng#art_99) ). Routing matters: Article 15 is absent from Article 99(4), so a breach lands in the 3% tier via Article 16(a), the provider's duty to conform with Chapter III Section 2.


### Gotchas


- **The Commission's timeline page is stale.** As of 29 July 2026 the[AI Act Service Desk timeline](https://ai-act-service-desk.ec.europa.eu/en/ai-act/timeline/timeline-implementation-eu-ai-act) still shows Annex III from 2 August 2026 and Annex I from 2 August 2027, footnoting the Omnibus as a *proposal* .
- **The extra time is real; the requirements are not softer.** 2 August 2026 still brings transparency and enforcement, and Article 15's text is untouched. Its *application* can now be limited by delegated act under the new Article 2(13) where Annex I Section A law gives equivalent protection.


### Do this next


1. **Inventory and classify.** Per AI feature: provider or deployer, and Annex III, Annex I embedded, transparency-only, or out of scope. That sets which date binds you.
2. **Add adversarial testing to the program.** Poisoning, evasion, extraction and prompt-injection testing on a recurring cadence, results retained. That evidence is your Article 15 conformity record.
3. **Extend the incident muscle you built for NIS2 and DORA.** Add the Article 73 clocks to that runbook, plus the Article 55(1)(c) channel if you ship a systemic-risk model.
4. **Watch the timeline.** It moved once in 2026 already, and Article 2(13) delegated acts are due by 2 August 2027.


## One program, many regulations


Read all eight instruments and the same five demands reappear under different article numbers, and each demand lands on a desk that already exists in your security organization. The sections below walk the org chart: for each department, what every regulation asks of it and where the regimes diverge. Build the capability once, then produce eight sets of evidence from it. The difficulty sits in the seams; clocks, recipients and anchor dates differ just enough to break a runbook written for one regime.


### Incident response: reporting on a 24/72-hour clock


Regulation The clock Who receives it


NIS2 24h early warning → 72h notification → final report one month after **the notification** (Art. 23(4)) National computer security incident response team (CSIRT) or competent authority


CRA (from 11 Sep 2026) 24h early warning → 72h notification → final within 14 days of a fix (vulnerabilities) or one month after the 72h filing (severe incidents) (Art. 14) Coordinating CSIRT **and** ENISA, simultaneously, via the Single Reporting Platform


DORA Initial within **4 hours of classifying** the incident as major, and no later than 24h from awareness → intermediate within 72h → final one month after the intermediate Financial competent authority


AI Act Serious incidents immediately on establishing a causal link, and within 15 days; 2 days for a widespread infringement or serious, irreversible disruption of critical infrastructure's management or operation; 10 days where someone has died (Art. 73) Market surveillance authorities; AI Office for systemic-risk general-purpose AI (Art. 55(1)(c))


CER (once designated) Initial within 24h, comprehensive report within one month (Art. 15) National competent authority


Sources:[NIS2](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) Art. 23;[CRA](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847) Art. 14, with the[Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting) and[ENISA](https://www.enisa.europa.eu/topics/product-security-and-certification/single-reporting-platform-srp) on routing;[Del. Reg. (EU) 2025/301](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202500301) Art. 5;[AI Act](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng#art_73) Art. 73;[CER Directive](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) Art. 15 and[EPRS](https://www.europarl.europa.eu/thinktank/en/document/EPRS_BRI(2026)785675) .


The trap is everything after the first 24 hours. DORA's clock starts at *classification* , NIS2's final report runs from your *notification* , the CRA's from the *fix* . One incident at a regulated firm that also ships software means three filings, three recipients, three anchor dates.


Relief is proposed, not delivered. The Digital Omnibus Regulation tabled on 19 November 2025 would build an ENISA-operated **Single Entry Point** on the CRA platform, so one submission satisfies NIS2, CER, DORA, GDPR and eIDAS reporting. Clocks and receiving authorities stay as they are; the one substantive change is the GDPR breach deadline moving from 72 to 96 hours. Parliament expects the entry point live 18 months after entry into force, stretching to 24 if testing says otherwise ([EPRS, 17 March 2026](https://www.europarl.europa.eu/thinktank/en/document/EPRS_BRI(2026)785675) ;[Alston & Bird](https://www.alstonprivacy.com/eu-moves-toward-a-single-entry-point-for-security-incident-reporting/) ). It was still at *Tabled* stage in first reading on 22 May 2026 ([EP Legislative Train](https://www.europarl.europa.eu/legislative-train/theme-a-new-plan-for-europe-s-sustainable-prosperity-and-competitiveness/file-digital-package) ). Build for today's five clocks.


### Product security: vulnerability management and SBOM


Regulation What it demands


[CRA](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847) No known exploitable vulnerabilities at release (Annex I, Part I(2)(a)); a machine-readable software bill of materials (SBOM) covering at least top-level dependencies (Part II(1)); prompt remediation, disclosure of fixed vulnerabilities, an enforced coordinated-disclosure policy (Part II(2)–(6))


[NIS2](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) **Vulnerability handling and disclosure** inside secure acquisition, development and maintenance (Art. 21(2)(e))


[DORA](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022R2554) Vulnerability assessments, open-source analysis, "source code reviews where feasible" (Art. 25(1)) on every critical-or-important-function system, at least yearly for all but microenterprises (Art. 24(6))


Cybersecurity Act EUCC is the only scheme adopted so far; CRA Art. 27 gives products certified under a recognised European scheme a presumption of conformity, often the cheapest evidence route ([EPRS](https://epthinktank.eu/2026/01/05/cybersecurity-act-review-what-to-expect/) )


### Application security and engineering: secure-by-design, with a documented development lifecycle


Regulation What it demands


[CRA](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847) Annex I requirements driven by a per-product risk assessment filed in the technical documentation (Art. 13(2)); secure-by-default configuration


[NIS2](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) Secure acquisition, development and maintenance (Art. 21(2)(e)), plus procedures to test that the measures work (Art. 21(2)(f))


[RED](https://eur-lex.europa.eu/eli/dir/2014/53/oj/eng) The EN 18031 mechanism families (access control, authentication, secure update, secure storage, secure communication, confidential cryptographic keys, cryptography), plus a product risk assessment in the technical file ([BSI](https://www.bsigroup.com/siteassets/pdf/en/products-and-services/gl-rs-pcert-lg-ict-dt-nsp-mp-digitaldomain-0025-red-guide-and-readiness.pdf) )


[AI Act](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng#art_15) Accuracy, robustness and cybersecurity across the lifecycle, resilient to data and model poisoning, adversarial examples, confidentiality attacks and model flaws (Art. 15(1), (5))


### Third-party risk management: supply chain and vendor control


Regulation What it demands


[NIS2](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) Supply chain security for direct suppliers (Art. 21(2)(d)), weighing each supplier's **specific** vulnerabilities and secure development procedures (Art. 21(3)); a certificate is an input, not an answer


[DORA](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022R2554) Register of Information for every ICT contractual arrangement (Art. 28(3)); mandatory contract terms including audit rights for the entity, its authority and the Lead Overseer (Art. 30(3)(e)) and an exit-transition period (Art. 30(3)(f))


[CRA](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847) Due diligence on third-party components before integration (Art. 13(5); recital 34)


Cybersecurity Act The January 2026 revision proposal would add a trusted ICT supply-chain security framework aimed at third-country suppliers ([Commission](https://digital-strategy.ec.europa.eu/en/policies/cybersecurity-act) )


### Governance and the board: minuted accountability


NIS2 is the sharp end. The management body must approve the risk-management measures, oversee implementation, follow training, and can be held liable for the entity's Article 21 infringements ([Art. 20](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) ). Miss a remediation deadline as an essential entity and Article 32(5) lets an authority suspend a certification and ask that a CEO-level person be barred from management functions. DORA hands the ICT risk-management framework to the management body to define, approve and answer for ([Art. 5(2)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022R2554) ). The AI Act makes deployers assign human oversight to people with "the necessary competence, training and authority" ([Art. 26(2)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng#art_26) ). All three want the same artifact: a minuted decision.


---


## The unified checklist: eight things to run this quarter


**1. Scope yourself, per country and per product, in writing.** Sector and size for NIS2 (Annexes I–II plus the Art. 2(2) regardless-of-size categories), product classification for the CRA, radio SKUs for RED, the twenty entity categories in DORA Art. 2(1), provider-or-deployer and risk tier for the AI Act. CER owes you nothing until a designation letter arrives; after that, roughly ten months from notification to comply with the resilience requirements ([Osborne Clarke](https://www.osborneclarke.com/insights/eu-critical-entities-resilience-directive-what-impact-your-organisation) ). Use the decision flow near the top of this guide as the worksheet.


**2. Build one incident runbook, parameterized per regulation.** One detection and triage path, one significance assessment, then a table of clocks, recipients and thresholds per regime. *NIS2, CRA, DORA, CER, AI Act.*


**3. Register your reporting access before you need it.** The CRA Single Reporting Platform needs an EU Login, the coordinating CSIRT validates your right to file after first access, and ENISA has confirmed no application programming interfaces (APIs) at this stage ([ENISA](https://www.enisa.europa.eu/topics/product-security-and-certification/single-reporting-platform-srp) ). Settle your coordinating CSIRT under Art. 14(7), name two or three filers, run one tabletop against the clock. *CRA, NIS2, DORA.*


**4. Stand up SBOM generation and vulnerability handling across every product.** Machine-readable SBOMs per release, retained as evidence; a published disclosure policy whose intake reaches a real triage rota; remediation tracked to closure. *CRA, NIS2, DORA, Cybersecurity Act.*


**5. Turn secure development into evidence.** Policies, tooling output and test results, assembled into the CRA technical documentation, the RED technical file, and the NIS2 Art. 21(2)(f) effectiveness record. Add recurring adversarial testing if you ship AI. *CRA, NIS2, RED, AI Act.*


**6. Run a third-party program with contractual flow-downs.** A classified ICT supplier register, security requirements in contracts, periodic verification. Financial entities: reconcile the DORA Register against your real contract set: in the ESAs' 2024 dry run, only 6.5% of the registers filed by almost 1,000 financial entities passed every data-quality check; 116 checks were applied ([EIOPA](https://www.eiopa.europa.eu/esas-dry-run-exercise-shows-goal-reporting-registers-information-under-digital-operational-2024-12-17_en) ). Vendors: pre-write your Art. 30 clause pack. *NIS2, DORA, CRA.*


**7. Brief the board and minute the sign-off.** The exposure, straight, and read every tier as "whichever is higher." NIS2: a maximum of at least €10,000,000 or 2% of group worldwide turnover for essential entities, €7,000,000 or 1.4% for important entities (Art. 34(4)–(5)). CRA: €15,000,000 or 2.5% for Annex I, Art. 13 and Art. 14 breaches (Art. 64). AI Act: €35,000,000 or 7% for prohibited practices, though for small and medium-sized enterprises the ceiling flips to whichever is lower (Arts. 99(3), 99(6)). Log the Article 20 training. *NIS2, DORA, CRA, AI Act.*


**8. Set support periods and watch the calendar.** Publish a support-period end month and year per product: at least five years, updates free of charge (CRA Annex I, Part II(7)–(8); Art. 13(8)). Then put the compliance calendar below in front of whoever owns your program roadmap, and review it every quarter. *CRA.*


## The EU compliance calendar


Every date in this guide in one view, built to monitor rather than read once. Review it quarterly. Expect the fastest movement in CRA harmonised standards and national NIS2 windows.


### Already in force


Since What applies


18 October 2024 NIS2: national measures apply; the original NIS Directive repealed (Arts. 41, 44) ([EUR-Lex](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) )


17 January 2025 DORA applies in full, in every member state, with no transition period (Art. 64) ([EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022R2554) )


2 February 2025 AI Act: prohibitions (Art. 5) and AI literacy (Art. 4) ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng) )


4 February 2025 Cyber Solidarity Act in force ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2025/38/oj/eng) )


27 February 2025 EUCC certification scheme applicable ([Impl. Reg. (EU) 2024/482](https://eur-lex.europa.eu/eli/reg_impl/2024/482/oj/eng) )


1 August 2025 RED Article 3(3)(d), (e) and (f) mandatory for radio equipment ([Del. Reg. (EU) 2023/2444](https://eur-lex.europa.eu/eli/reg_del/2023/2444/oj/eng) )


2 August 2025 AI Act: GPAI model obligations (Arts. 51–56) ([EUR-Lex](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng) )


### Rolling clocks to track


Cadence What to watch


Now, per member state CER designation letters (the identification deadline was 17 July 2026): first risk assessment due nine months after notification, resilience obligations at ten ([CER Arts. 6(3), 12(1)](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng) )


Per country, recurring NIS2 registration and listing windows, plus the two-week duty to notify changes (Art. 3(3)–(4)) ([EUR-Lex](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) )


Each spring DORA Registers of Information: national submission windows run in February and March, authorities forward them to the ESAs by 31 March, reference date 31 December ([BaFin](https://www.bafin.de/SharedDocs/FAQs/DE/DORA/Informationsregister_und_Anzeigepflichten/1_Allgemeines/01.html) )


Yearly The ESAs re-publish the DORA list of critical ICT third-party providers (Art. 31(9)); the first list landed 18 November 2025 ([EBA](https://www.eba.europa.eu/publications-and-media/press-releases/european-supervisory-authorities-designate-critical-ict-third-party-providers-under-digital) )


### Dates ahead


Date What lands


2 August 2026 AI Act general application; most Art. 50 transparency; enforcement begins


**11 September 2026** CRA Art. 14 reporting applies; ENISA Single Reporting Platform live


About 18 November 2026 Third-country CTPPs designated in 2025 need an EU subsidiary (DORA Art. 31(12))


2 December 2027 AI Act high-risk obligations, standalone Annex III systems ([Reg. (EU) 2026/1744](https://eur-lex.europa.eu/eli/reg/2026/1744/oj/eng) )


**11 December 2027** CRA applies in full: CE marking, essential requirements; RED Delegated Regulation 2022/30 repealed ([Del. Reg. (EU) 2026/339](https://eur-lex.europa.eu/eli/reg_del/2026/339/oj/eng) )


11 June 2028 Existing EU type-examination certificates covering cybersecurity requirements stop being valid, unless they lapse sooner (CRA Art. 69(1))


2 August 2028 AI Act high-risk obligations, Annex I embedded systems


## How ZeroPath helps you get ready


Nothing in this guide requires a specific tool. But a handful of the obligations are engineering work, done continuously and evidenced on demand, and that's where tooling earns its keep. Four capabilities, and the obligation language each one answers to:


ZeroPath capability Obligation it addresses Regulations


SAST + SCA vulnerability discovery On the basis of the manufacturer's cybersecurity risk assessment and where applicable, products with digital elements shall "be made available on the market without known exploitable vulnerabilities" ([CRA Annex I, Part I, pt 2(a)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#anx_I) ); NIS2 requires "security in network and information systems acquisition, development and maintenance, including vulnerability handling and disclosure" ([Art. 21(2)(e)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022L2555#art_21) ) CRA, NIS2


SBOM generation (CycloneDX export) "Identify and document vulnerabilities and components contained in products with digital elements, including by drawing up a software bill of materials in a commonly used and machine-readable format covering at the very least the top-level dependencies of the products" ([CRA Annex I, Part II, pt 1](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#anx_I) ); NIS2 supply-chain security ([Art. 21(2)(d)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022L2555#art_21) ) CRA, NIS2


Autofix / automatic PR creation "Address and remediate vulnerabilities without delay, including by providing security updates," with those updates "disseminated without delay" ([CRA Annex I, Part II, pts 2 and 8](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#anx_I) ); DORA's technical standards require patch deployment to be prioritized and installation deadlines to be set ([CDR (EU) 2024/1774, Art. 10(2)(f) and 10(4)(d)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R1774#art_10) ) CRA, NIS2, DORA


Continuous scanning + reporting "Apply effective and regular tests and reviews of the security of the product with digital elements" ([CRA Annex I, Part II, pt 3](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#anx_I) ); the evidence a conformity assessment then verifies ([Art. 3(27)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#art_3) ); DORA's technical standards require automated vulnerability scanning at least weekly on ICT assets supporting critical or important functions ([CDR (EU) 2024/1774, Art. 10(2)(b)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R1774#art_10) ) CRA, NIS2, DORA,[Cybersecurity Act schemes](https://www.enisa.europa.eu/topics/certification)


ZeroPath covers static application security testing (SAST), reachability-aware software composition analysis (SCA), secrets and infrastructure-as-code scanning, pull-request review, CycloneDX SBOM export, and AI-generated patches delivered as pull requests ([ZeroPath: SCA](https://zeropath.com/products/sca) ,[ZeroPath docs](https://zeropath.com/docs) ). If you want to work out how much of your readiness you can automate, the documentation is the place to start.


## Glossary


- **CE marking** : the marking by which a manufacturer indicates that a product with digital elements, and the processes behind it, conform with Annex I and other applicable Union harmonisation legislation ([CRA Art. 3(31)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#art_3) ). For software, it goes on the EU declaration of conformity or on the website accompanying the product ([Art. 30(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#art_30) ).
- **Conformity assessment** : the process of verifying whether the essential cybersecurity requirements in Annex I have been fulfilled ([CRA Art. 3(27)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#art_3) ).
- **Coordinated vulnerability disclosure (CVD)** : reporting a flaw to the vendor or a CSIRT so a fix ships before public disclosure. Each Member State must designate a CSIRT as CVD coordinator and let people report anonymously if they ask to ([NIS2 Art. 12(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022L2555#art_12) ).
- **CSIRT** : Computer Security Incident Response Team. The one designated as coordinator acts as trusted intermediary between the reporter and the vendor, and negotiates disclosure timelines ([NIS2 Art. 12(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022L2555#art_12) ).
- **Delegated act** : a Commission act adopted under a delegation written into a legislative act. It cannot change the essential elements of the law, and Parliament and Council can object or revoke the delegation ([European Commission](https://commission.europa.eu/law/law-making-process/adopting-eu-law/implementing-and-delegated-acts_en) ).
- **ENISA** : the EU Agency for Cybersecurity, headquartered in Athens with offices in Heraklion and Brussels ([European Union](https://european-union.europa.eu/institutions-law-budget/institutions-and-bodies/search-all-eu-institutions-and-bodies/european-union-agency-cybersecurity-enisa_en) ). Its mandate comes from the Cybersecurity Act,[Regulation (EU) 2019/881](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32019R0881) .
- **ESAs** : the three European Supervisory Authorities (EBA, EIOPA, ESMA). Under DORA they designate critical ICT third-party providers and appoint a Lead Overseer for each one ([DORA Art. 31(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022R2554#art_31) ,[EIOPA](https://www.eiopa.europa.eu/digital-operational-resilience-act-dora/dora-oversight_en) ).
- **Essential vs. important entity** : NIS2's two tiers. Annex I entities above the medium-enterprise ceilings are *essential* ; Annex I and II entities that don't qualify as essential are *important* ([Art. 3](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022L2555#art_3) ). Important entities get lighter, reactive supervision: competent authorities act *ex post* , on evidence or indication of non-compliance ([Art. 33](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022L2555#art_33) ).
- **General-purpose AI model (GPAI)** : a model that displays significant generality, performs a wide range of distinct tasks competently, and can be integrated into a variety of downstream systems ([AI Act Art. 3(63)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R1689#art_3) ).
- **Harmonised standard** : a standard drafted by a European Standardisation Organisation at the Commission's request ([CRA Art. 3(36)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#art_3) ). Once its reference is published in the *Official Journal* , conforming to it earns a presumption of conformity with the essential requirements it covers ([Art. 27(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#art_27) ).
- **Notified body** : a conformity assessment body designated by a Member State under Article 43 ([CRA Art. 3(29)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#art_3) ), independent of both manufacturers and market surveillance authorities. The Commission publishes the list ([Art. 44(2)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#art_44) ) through NANDO ([Commission CRA FAQ 6.9](https://cra.orcwg.org/faq/official/faq_6-9/) ).
- **SBOM** : a formal record containing details and supply-chain relationships of the components included in the software elements of a product ([CRA Art. 3(39)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R2847#art_3) ).
- **TLPT (threat-led penetration testing)** : an intelligence-led red-team exercise that mimics the tactics of real threat actors against a financial entity's critical live production systems ([DORA Art. 3(17)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022R2554#art_3) ). In-scope entities run one at least every three years ([Art. 26(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022R2554#art_26) ).
- **Transposition** : how Member States write an EU *directive* into national law by a deadline set in the directive. Regulations (CRA, DORA) apply directly; directives (NIS2) have to be transposed ([EUR-Lex](https://eur-lex.europa.eu/EN/legal-content/glossary/transposition.html) ).


## Disclaimer


This guide is informational and is not legal advice. EU cybersecurity law is still moving: delegated acts, harmonised standards, and national transposition measures continue to land, and Member State implementations differ. Verify every date, threshold, and penalty tier against the official text on EUR-Lex and your national authority's guidance, and consult qualified counsel before making compliance decisions.


## About the author


**Gaurav Sarraf** , ZeroPath Team, Founding Engineer.


## References


Grouped by instrument; primary sources (EUR-Lex, European Commission, EU agencies and authorities) first within each group. Inline citations in the body sometimes use the ELI or CELEX form of the same EUR-Lex text, with article anchors; each instrument is listed once below.


### NIS2


1. [Directive (EU) 2022/2555 (NIS2 Directive), full text — EUR-Lex, OJ L 333, 27.12.2022](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng)
2. [NIS2 Directive: securing network and information systems — European Commission](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive)
3. [Commission refers Ireland, Spain, France and the Netherlands to the Court of Justice for failing to transpose the rules on cybersecurity (8 July 2026) — European Commission](https://digital-strategy.ec.europa.eu/en/news/commission-refers-ireland-spain-france-and-netherlands-court-justice-failing-transpose-rules)
4. [NIS2 Directive transposition in EU countries — European Commission](https://digital-strategy.ec.europa.eu/en/policies/nis-transposition)
5. [Proposal for a Directive as regards simplification measures and alignment with the Cybersecurity Act, COM(2026) 13 (20 January 2026) — European Commission](https://digital-strategy.ec.europa.eu/en/library/proposal-directive-regards-simplification-measures-and-alignment-cybersecurity-act)
6. [Supporting NIS2 implementation through actionable guidance (26 June 2025) — ENISA](https://www.enisa.europa.eu/news/supporting-nis2-implementation-through-actionable-guidance)
7. [NIS2 Technical Implementation Guidance (26 June 2025) — ENISA](https://www.enisa.europa.eu/publications/nis2-technical-implementation-guidance)
8. [Domande frequenti NIS — Misure di sicurezza e notifica di incidenti — Agenzia per la Cybersicurezza Nazionale (Italy)](https://www.acn.gov.it/portale/faq/nis/misure-di-sicurezza-e-notifica-di-incidenti)
9. [NIS2 & SME guidelines: how do they apply and thresholds — Arthur Cox](https://www.arthurcox.com/knowledge/nis2-sme-guidelines-how-do-they-apply-and-thresholds/)
10. [EU: NIS2 Update — EU Moves to Harmonise Cyber Controls, Refine Scope, and Add New In-Scope Entities (3 February 2026) — DLA Piper](https://privacymatters.dlapiper.com/2026/02/eu-nis2-update-eu-moves-to-harmonise-cyber-controls-refine-scope-and-add-new-in-scope-entities/)


### Cyber Resilience Act (CRA)


1. [Regulation (EU) 2024/2847 (Cyber Resilience Act), full text — EUR-Lex, Official Journal L series, 20 November 2024](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847)
2. [Regulation (EU) 2024/2847 — ELI record (entry into force 10 December 2024) — EUR-Lex](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng)
3. [Commission Implementing Regulation (EU) 2025/2392 of 28 November 2025 on the technical description of important and critical product categories — EUR-Lex](http://data.europa.eu/eli/reg_impl/2025/2392/oj)
4. [Cyber Resilience Act — European Commission, Shaping Europe's Digital Future](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act)
5. [The Cyber Resilience Act — Summary of the legislative text — European Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-summary)
6. [Cyber Resilience Act — Reporting obligations — European Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting)
7. [Cyber Resilience Act — Open source — European Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-open-source)
8. [Cyber Resilience Act — Standardisation (standardisation request M/606) — European Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-standardisation)
9. [Single Reporting Platform (SRP) — Frequently Asked Questions, updated 17 July 2026 — ENISA](https://www.enisa.europa.eu/topics/product-security-and-certification/single-reporting-platform-srp)
10. [Commission CRA FAQ 6.9, Notified bodies — European Commission / ORC WG CRA Hub](https://cra.orcwg.org/faq/official/faq_6-9/)
11. [Cyber Resilience Act: the fine line between SaaS and digital products (16 February 2026) — DLA Piper](https://www.dlapiper.com/en/insights/publications/2026/02/cyber-resilience-act-the-fine-line-between-saas-and-digital-products)
12. [Current status of standardisation for the Cyber Resilience Act (6 July 2026) — IBF Solutions](https://www.ibf-solutions.com/en/news-and-knowledge/technical-papers-and-news-on-ce-marking/current-status-of-standardisation-for-the-cyber-resilience-act)


### Radio Equipment Directive (RED)


1. [Directive 2014/53/EU (Radio Equipment Directive) — EUR-Lex](https://eur-lex.europa.eu/eli/dir/2014/53/oj/eng)
2. [Commission Delegated Regulation (EU) 2022/30 — EUR-Lex](https://eur-lex.europa.eu/eli/reg_del/2022/30/oj/eng)
3. [Commission Delegated Regulation (EU) 2023/2444 (postponing application to 1 August 2025) — EUR-Lex](https://eur-lex.europa.eu/eli/reg_del/2023/2444/oj/eng)
4. [Commission Implementing Decision (EU) 2025/138 (citing EN 18031-1/-2/-3 with restrictions) — EUR-Lex](https://eur-lex.europa.eu/eli/dec_impl/2025/138/oj/eng)
5. [Commission Delegated Regulation (EU) 2026/339 (repealing Delegated Regulation (EU) 2022/30 with effect from 11 December 2027) — EUR-Lex](https://eur-lex.europa.eu/eli/reg_del/2026/339/oj/eng)
6. [Regulation (EU) 2019/1020 on market surveillance and compliance of products, Art. 4 — EUR-Lex](https://eur-lex.europa.eu/eli/reg/2019/1020/oj/eng)
7. [Guidance on the application of the harmonised standards series EN 18031:2024 in support of Commission Delegated Regulation 2022/30 — European Commission, DG GROW (mirror hosted by VDMA)](https://www.vdma.eu/documents/d/group-34568/com-red-hs-cyber-guidance_v1)
8. [Radio Equipment Directive (RED) — European Commission, DG Internal Market, Industry, Entrepreneurship and SMEs](https://single-market-economy.ec.europa.eu/sectors/electrical-and-electronic-engineering-industries-eei/radio-equipment-directive-red_en)
9. [Radio Equipment Directive Cybersecurity Testing — EN 18031 — BSI Group](https://www.bsigroup.com/siteassets/pdf/en/products-and-services/gl-rs-pcert-lg-ict-dt-nsp-mp-digitaldomain-0025-red-guide-and-readiness.pdf)
10. [RED cybersecurity requirements mandatory on 1 August 2025 — SGS](https://www.sgs.com/en-dk/news/2025/06/red-cybersecurity-requirements-mandatory-on-1-august-2025)


### DORA


1. [Regulation (EU) 2022/2554 (DORA), full text — EUR-Lex / Official Journal of the European Union](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32022R2554)
2. [Commission Delegated Regulation (EU) 2025/301 — content and time limits for major ICT-related incident notifications and reports — EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202500301)
3. [Commission Delegated Regulation (EU) 2024/1772 — classification criteria and materiality thresholds for ICT-related incidents — EUR-Lex](https://eur-lex.europa.eu/eli/reg_del/2024/1772/oj)
4. [Commission Delegated Regulation (EU) 2024/1774, Art. 10 — vulnerability and patch management — EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32024R1774#art_10)
5. [The European Supervisory Authorities designate critical ICT third-party providers under DORA (18 November 2025) — European Banking Authority](https://www.eba.europa.eu/publications-and-media/press-releases/european-supervisory-authorities-designate-critical-ict-third-party-providers-under-digital)
6. [List of designated CTPPs (PDF) — European Banking Authority](https://www.eba.europa.eu/sites/default/files/2025-11/e388451b-356b-408a-bbf2-b8e425865d75/List%20of%20designated%20CTPPs.pdf)
7. [DORA oversight — framework, Lead Overseer, Joint Examination Teams, opt-in procedure and fee — EIOPA](https://www.eiopa.europa.eu/digital-operational-resilience-act-dora/dora-oversight_en)
8. [ESAs' Dry Run exercise on reporting registers of information (17 December 2024) — ESAs press release, hosted by EIOPA](https://www.eiopa.europa.eu/esas-dry-run-exercise-shows-goal-reporting-registers-information-under-digital-operational-2024-12-17_en)
9. [DORA — submission timeframe for register of information (11 February 2026) — Commission de Surveillance du Secteur Financier (Luxembourg)](https://www.cssf.lu/en/2026/02/dora-submission-timeframe-for-register-of-information-edesk-portal-open-as-of-11-february-2026/)
10. [Risks in BaFin's Focus 2026 — Bundesanstalt für Finanzdienstleistungsaufsicht](https://www.bafin.de/EN/die-bafin/publikationen-daten/risiken-im-fokus/Fokusrisiken_2026/Druckansicht/Fokusrisiken_2026_druck_node_en.html)
11. [FAQ: Wann muss das Informationsregister eingereicht werden? — Bundesanstalt für Finanzdienstleistungsaufsicht](https://www.bafin.de/SharedDocs/FAQs/DE/DORA/Informationsregister_und_Anzeigepflichten/1_Allgemeines/01.html)
12. [Informationsregister und Anzeigepflichten — German submission window 9 to 30 March 2026 — Bundesanstalt für Finanzdienstleistungsaufsicht](https://www.bafin.de/DE/unternehmen-maerkte/aufsicht/alle-unternehmen/dora/Informationsregister_und_Anzeigepflichten/Informationsregister_und_Anzeigepflichten_node.html)
13. [Q&A DORA102 — scope of DORA for third-country entities and branches — ESAs Q&A database (EIOPA)](https://www.eiopa.europa.eu/qa-regulation/questions-and-answers-database/dora102-3097_en)


### CER Directive


1. [Directive (EU) 2022/2557 on the resilience of critical entities (CER Directive) — EUR-Lex](https://eur-lex.europa.eu/eli/dir/2022/2557/oj/eng)
2. [Critical infrastructure resilience — European Commission, DG HOME](https://home-affairs.ec.europa.eu/policies/internal-security/counter-terrorism-and-radicalisation/protection/critical-infrastructure-resilience_en)
3. [Commission decides to refer Bulgaria, France, Luxembourg, the Netherlands, Poland, Spain and Sweden to the Court of Justice for failing to transpose the CER Directive (IP/26/910, 28 April 2026) — European Commission](https://ec.europa.eu/commission/presscorner/detail/en/ip_26_910)
4. [EU: CER Directive enters a new phase as "critical entity" designation deadline arrives (17 July 2026) — DLA Piper](https://privacymatters.dlapiper.com/2026/07/eu-cer-directive-enters-a-new-phase-as-critical-entity-designation-deadline-arrives/)
5. [The EU Critical Entities Resilience Directive — What is the impact on your organisation? (29 July 2025) — Osborne Clarke](https://www.osborneclarke.com/insights/eu-critical-entities-resilience-directive-what-impact-your-organisation)
6. [Are you ready to comply with the CER Directive's resilience requirements? (29 September 2025) — WTW](https://www.wtwco.com/en-gb/insights/2025/09/are-you-ready-to-comply-with-the-cer-directives-resilience-requirements)
7. [The Critical Entities Resilience Directive (CER) — Taylor Wessing](https://digital-regulation.taylorwessing.com/welcome/the-critical-entities-resilience-directive-cer)
8. [European Commission adopts infringement decisions against member states for not transposing security directives (2 December 2024) — Industrial Cyber](https://industrialcyber.co/regulation-standards-and-compliance/european-commission-adopts-infringement-decisions-against-member-states-for-not-transposing-security-directives/)


### Cybersecurity Act


1. [Regulation (EU) 2019/881 (Cybersecurity Act) — EUR-Lex](https://eur-lex.europa.eu/eli/reg/2019/881/oj/eng)
2. [Commission Implementing Regulation (EU) 2024/482 (EUCC scheme) — EUR-Lex](https://eur-lex.europa.eu/eli/reg_impl/2024/482/oj/eng)
3. [Regulation (EU) 2025/37 on managed security services (amending Regulation (EU) 2019/881) — EUR-Lex](https://eur-lex.europa.eu/eli/reg/2025/37/oj/eng)
4. [EU Cybersecurity Act — European Commission, Shaping Europe's Digital Future](https://digital-strategy.ec.europa.eu/en/policies/cybersecurity-act)
5. [Proposal for a Regulation for the EU Cybersecurity Act, COM(2026) 11 (20 January 2026) — European Commission](https://digital-strategy.ec.europa.eu/en/library/proposal-regulation-eu-cybersecurity-act)
6. [EU Cybersecurity Certification Framework — European Commission](https://digital-strategy.ec.europa.eu/en/policies/cybersecurity-certification-framework)
7. [European cybersecurity certification schemes — ENISA certification library](https://certification.enisa.europa.eu/index_en)
8. [Certification — ENISA](https://www.enisa.europa.eu/topics/certification)
9. [Cybersecurity Act review: What to expect (At a Glance, 5 January 2026) — European Parliamentary Research Service](https://epthinktank.eu/2026/01/05/cybersecurity-act-review-what-to-expect/)


### Cyber Solidarity Act


1. [Regulation (EU) 2025/38 (Cyber Solidarity Act) — EUR-Lex](https://eur-lex.europa.eu/eli/reg/2025/38/oj/eng)
2. [EU Cyber Solidarity Act — European Commission, Shaping Europe's Digital Future](https://digital-strategy.ec.europa.eu/en/policies/cyber-solidarity)
3. [EU Cybersecurity Reserve — ENISA](https://www.enisa.europa.eu/topics/eu-incident-response-and-cyber-crisis-management/eu-cybersecurity-reserve)
4. [ENISA to operate the EU Cybersecurity Reserve with EUR 36 million (26 August 2025) — ENISA](https://www.enisa.europa.eu/news/enisa-to-operate-the-eu-cybersecurity-reserve-with-eur-36-million)


### AI Act


1. [Regulation (EU) 2024/1689 (Artificial Intelligence Act), Official Journal text — EUR-Lex](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng)
2. [Regulation (EU) 2026/1744 of 8 July 2026 amending Regulations (EU) 2024/1689, (EU) 2018/1139 and (EU) 2023/1230 (Digital Omnibus on AI) — EUR-Lex](https://eur-lex.europa.eu/eli/reg/2026/1744/oj/eng)
3. [Timeline for the Implementation of the EU AI Act — AI Act Service Desk, European Commission (DG CONNECT)](https://ai-act-service-desk.ec.europa.eu/en/ai-act/timeline/timeline-implementation-eu-ai-act)
4. [EU AI Act unpacked #34: The final Digital Omnibus on AI (10 July 2026) — Freshfields](https://www.freshfields.com/en/our-thinking/blogs/technology-quotient/eu-ai-act-unpacked-34-the-final-digital-omnibus-on-ai-key-amendments-to-the-a-102nber)
5. [EU AI Act Omnibus Agreement — Postponed High-Risk Deadlines and Other Key Changes (27 May 2026) — Gibson Dunn](https://www.gibsondunn.com/eu-ai-act-omnibus-agreement-postponed-high-risk-deadlines-and-other-key-changes/)


### Cross-cutting: incident-reporting simplification


1. [Simplifying cybersecurity reporting: The Digital Omnibus Single-Entry Point mechanism (Briefing, 17 March 2026) — European Parliamentary Research Service](https://www.europarl.europa.eu/thinktank/en/document/EPRS_BRI(2026)785675)
2. [The Digital Omnibus Regulation Proposal, procedure 2025/0360(COD), status as of 22 May 2026 — European Parliament, Legislative Train Schedule](https://www.europarl.europa.eu/legislative-train/theme-a-new-plan-for-europe-s-sustainable-prosperity-and-competitiveness/file-digital-package)
3. [EU Moves Toward a Single Entry Point for Security Incident Reporting (19 March 2026) — Alston & Bird](https://www.alstonprivacy.com/eu-moves-toward-a-single-entry-point-for-security-incident-reporting/)
4. [Digital Omnibus package: single EU harmonised incident reporting regime across cyber and data protection (15 December 2025) — Bird & Bird](https://www.twobirds.com/en/insights/2025/digital-omnibus-package-single-eu-harmonised-incident-reporting-regime-across-cyber-and-data-protect)


### EU institutions and other


1. [Transposition — EUR-Lex glossary](https://eur-lex.europa.eu/EN/legal-content/glossary/transposition.html)
2. [Implementing and delegated acts — European Commission](https://commission.europa.eu/law/law-making-process/adopting-eu-law/implementing-and-delegated-acts_en)
3. [European Union Agency for Cybersecurity (ENISA) — European Union](https://european-union.europa.eu/institutions-law-budget/institutions-and-bodies/search-all-eu-institutions-and-bodies/european-union-agency-cybersecurity-enisa_en)
4. [Software Composition Analysis — ZeroPath](https://zeropath.com/products/sca)
5. [Documentation — ZeroPath](https://zeropath.com/docs)
6. [ZeroPath](https://zeropath.com/)
