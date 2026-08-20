---
schema_version: "1.0.0"
document_id: "c0cade4dc6b06ae3b67c7c3c396882b9b89834e72105f2576623a06d8ab3a756"
company_key: "fluence-energy-inc-class-a-common-stock"
company: "Fluence Energy Inc."
source_id: "fluence-energy-inc-class-a-common-stock-rss-8dd7677c12d9"
canonical_url: "https://blog.fluenceenergy.com/cybersecurity-battery-energy-storage-mitigating-risks-changing-policy-landscape"
published_at: "2025-04-03T14:00:00+00:00"
first_seen_at: "2026-07-20T03:32:26.767335+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:198824c0f15cd8ac7ebc3f6c067ba2c11e7f4db509da29ba3948a3e72d960ea0"
---

# Cybersecurity in Battery Energy Storage: Mitigating Risks in a Changing Policy Landscape

The transformation of the electric grid brings new complexities to its security. Power systems that were once manually operated and physically isolated are now highly digital, interconnected, and increasingly reliant on software-based controls. This shift has introduced significant advantages—enabling faster response times, more efficient system management, and better integration of new technologies. However, it has also placed grid-connected assets at the center of a growing cybersecurity conversation.


[Battery energy storage systems (BESS)](https://fluenceenergy.com/energy-storage-technology/) are a fundamental part of our modern grid infrastructure. Storage allows operators to stabilize supply and demand, optimize system efficiency, and support critical infrastructure during power disruptions. But as with all advanced grid assets, energy storage operates within a digital environment where cyber threats are increasing in both sophistication and frequency.


While concerns about digitalization introducing new vulnerabilities are valid, modern energy storage systems can be designed with security from the outset—unlike many legacy power assets that have long operated with outdated, unpatched control systems. The key challenge is not the presence of digital technology itself, but rather how well it is secured and how upstream vulnerabilities in the supply chain are identified and mitigated. When securely designed, integrated, and maintained, storage can enhance the resilience of grid operations, particularly as attackers increasingly target earlier stages of the supply chain as a tactic to disrupt operations.


## Understanding the Growing Cyber Threat to Energy Infrastructure


Cyber threats targeting power infrastructure are no longer theoretical—they are already happening. The[2015 cyberattack on Ukraine’s power grid](https://www.cisa.gov/news-events/ics-alerts/ir-alert-h-16-056-01) demonstrated how nation-state actors could remotely disrupt operations, causing widespread outages by manipulating industrial control systems. This attack was not just about shutting off power; it was designed to create chaos by disabling circuit breakers, manipulating data, and delaying recovery efforts.


More recently,[Volt Typhoon](https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-038a) , a state-sponsored cyber campaign attributed to China, has been identified as an active threat to critical infrastructure in the United States. Cyber actors have embedded persistent malware within systems tied to the power sector and other critical industries. Intelligence reports indicate that this malware is designed not for immediate disruption but for strategic positioning—waiting for the right geopolitical moment to be activated. This form of cyber pre-positioning is particularly concerning because it suggests that adversarial nations are not simply probing infrastructure for vulnerabilities but actively planning for scenarios where they could trigger disruptions to undermine national stability.


These threats extend beyond the United States. In 2021,[Australian energy company CS Energy fell victim to a ransomware attack](https://www.csenergy.com.au/news/cs-energy-responds-to-cyber-security-incident) that attempted to take control of corporate networks. While the attack was neutralized before impacting grid operations, the incident served as a stark reminder that cybersecurity threats have real consequences. Just around two years later, in 2023,[Denmark’s energy sector experienced a significant cyberattack](https://www.securityweek.com/22-energy-firms-hacked-in-largest-coordinated-attack-on-denmarks-critical-infrastructure/) when attackers exploited vulnerabilities in a firewall and compromised 22 energy organizations within a few days. The attack, which caused temporary disruption and required isolating the affected companies from the network to maintain overall grid stability, underscored the vulnerability of critical energy infrastructure and highlighted the need for improved cybersecurity maturity.


## How Regulatory Environments Are Reshaping Cybersecurity Expectations


Governments worldwide are responding to these growing risks with stricter policies to enforce minimum cybersecurity standards that support more resilience across critical infrastructure. According to[an analysis conducted by MIT](https://cams.mit.edu/wp-content/uploads/The-Global-Matrix-of-Cybersecurity-Compliance.pdf) , over 150 countries have now enacted cybersecurity legislation, and there are over 170 regulations that focus on specific features, types, enforcement bodies, regional applicability, and purposes. This surge of policy and regulation creates a complex and frequently changing compliance environment yet is driving a common floor across industries for cybersecurity and supply chain security.


In the United States, there is a specific focus on minimizing risk from potential threat actors related to foreign adversaries—a concern that is shared by both the Republican and Democratic parties. For example, during his first term, President Trump[signed an Executive Order on “Securing the United States Bulk-Power System”](https://trumpwhitehouse.archives.gov/presidential-actions/executive-order-securing-united-states-bulk-power-system/) that prohibited the acquisition, importation, transfer, or installation of bulk-power system infrastructure that originated in, or was otherwise subject to the direction of, a foreign adversary country. While the Biden administration repealed that Executive Order, it too took steps to limit exposure to threats from foreign adversaries in critical infrastructure in multiple cases, including restricting the import or sale of connected vehicles and associated hardware and software with a sufficient nexus to China or Russia. For both administrations, the message was clear—the United States government is deeply concerned with the disruption risks associated with controls equipment originating in a foreign adversary country.


At least seven states have also moved policy through legislative or executive action that, at the very least, scrutinizes relationships with companies from China or other foreign adversaries. While most of those policies focus on public sector contracting,[Texas’ Lone Star Infrastructure Protection Act](https://capitol.texas.gov/tlodocs/87R/billtext/html/SB02116F.htm) stands out as an example of a policy that prohibits private businesses from entering into contracts with companies linked to adversarial nations that would grant them direct or indirect access to critical infrastructure.


Similar efforts are underway globally. In Europe, the[NIS2 Directive](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive) expands cybersecurity obligations for energy infrastructure, while the[Cyber Resilience Act (CRA)](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act) mandates that manufacturers ensure critical software and hardware components are designed with security controls that can withstand evolving threats.


While sweeping rules around hardware components from countries of concern are not common in Europe—which traditionally takes an open-trade approach and has a heterogeneous landscape of security legislation—the region is showing signs of directional policy development. In a recent proposal for prequalification criteria for renewable auctions under the[Net-Zero Industry Act](https://commission.europa.eu/strategy-and-policy/priorities-2019-2024/european-green-deal/green-deal-industrial-plan/net-zero-industry-act_en) , the European Commission proposed limiting operational asset control from outside Europe and setting higher documentation requirements on cybersecurity safeguards for countries of concern. These regulatory shifts indicate a clear trajectory: supply chain security and cybersecurity will only become more central to energy procurement decisions.


For developers, independent power producers (IPPs), and utilities, adapting to these policy shifts means embedding cybersecurity into every stage of project development and procurement. Systems that cannot demonstrate clear supply chain traceability and robust cybersecurity protections will face increasing scrutiny—from local and federal governments, financers, and insurers—making proactive risk management essential to project success.


## Mitigating Cyber and Supply Chain Risks in Battery Storage Systems


In today's highly dynamic landscape, procurement teams must navigate rapidly changing policies, evolving technologies, and shifting geopolitical threats. This complexity requires a sophisticated understanding of how these factors should influence buying criteria and potential impact on financial or reputational risk. As the regulatory environment tightens and threats grow more sophisticated, a comprehensive approach to risk mitigation becomes essential for long-term operational security.


### Understanding Hardware and Software Risk Exposure


Mitigating cybersecurity risks starts with understanding the vulnerabilities inherent in hardware and software components. Key components such as the battery management system (BMS), inverters, and DC power management systems rely on software and firmware that can be remotely updated, creating potential entry points for malicious actors.


To address these risks, it is essential to have a clear understanding of your system bill of materials (BOM) and supply chain. This includes knowing where each component is sourced, who developed the software, and how it is maintained. Traceability and documentation are critical, as they enable developers to identify potential vulnerabilities and ensure compliance with current and future regulatory requirements.


### Supply Chain Diversification


Diversifying the supply chain is another key strategy for mitigating cybersecurity risks. Relying solely on a single country for critical components increases your exposure to supply chain disruptions. By working with storage solution providers that source components from a mix of domestic and international suppliers, developers can access a more resilient supply chain that is better equipped to adapt to changing policy environments, disruptions in supply chains, or escalating trade wars.


Risk is mitigated even further with technology suppliers that build key controls hardware and software in the United States or with friendly trade allies that adhere to identifiable secure development requirements.


*Existing Fluence System Production, Utah, USA*


### Defining Divisions of Responsibility


Clear contractual agreements are essential for ensuring that cybersecurity responsibilities are well-defined between asset owners and technology suppliers. This includes specifying who is responsible for security updates, patching, and incident response. Service level agreements (SLAs) with technology providers should clearly outline these responsibilities to ensure that all parties are aligned.


When evaluating providers, factors to consider include their geographical location, whether they manage services internally or outsource to third parties, their track record with battery storage assets, and their support availability.


### Adherence to Cybersecurity Standards


Finally, cybersecurity protection requires adherence to well-established industry standards and regulatory frameworks. For example, compliance with the[North American Electric Reliability Corporation Critical Infrastructure Protection (NERC CIP)](https://www.nerc.com/pa/Stand/Pages/ReliabilityStandards.aspx) for grid-connected assets provides a foundation for security controls and monitoring capabilities and is a key priority for energy storage operators in the United States, mandating fundamental controls for protecting critical infrastructure from cyber threats. Compliance requirements like this help verify that software components throughout the supply chain meet security standards and that vulnerabilities can be quickly identified and addressed.


## The Future of Secure Energy Storage


As the energy sector continues to transition toward a more digital and interconnected future, resilience is defined not only by physical reliability but also by digital security. The evolving policy landscape, coupled with the growing threat of state- and non-state sponsored cyberattacks, underscores the importance of taking proactive steps to mitigate risks across the development process—from technology selection, to installation, and long-term operations. By understanding hardware and software risk exposure, diversifying the supply chain, and defining clear divisions of responsibility, developers can better protect their projects from cyber threats.


At Fluence, we are committed to helping our customers navigate this complex landscape. Our energy storage solutions are designed with[cybersecurity at their core](https://blog.fluenceenergy.com/hubfs/Cybersecurity-at-Fluence_BR-52-01-EN.pdf#:~:text=Enterprise-class,%20industrial-grade%20security%20across%20our%20core%20business%20systems,,grid%20infrastructure%20secure%20and%20supporting%20international%20cybersecurity%20standards.) , incorporating secure network architectures, remote access controls, and continuous monitoring to detect and mitigate potential threats. As the policy landscape continues to evolve, we remain dedicated to providing our customers with the tools and expertise they need to stay ahead of emerging risks.


The trends we have covered are significant and bipartisan, showing no signs of abating. In a world where geopolitical tensions are rising and cyber threats are becoming more sophisticated, taking proactive steps to de-risk exposure is more important than ever. Those who address these challenges head-on will be best positioned for long-term success, ensuring that energy storage remains a trusted, indispensable tool for securing the power grid of the future.


### Navigate the complex landscape of energy cybersecurity with confidence. Speak to our experts about tailored solutions for your battery storage needs.
