---
schema_version: "1.0.0"
document_id: "58f1b1fcc3bed9231014755632116420bc4937a4ccac76adad8df741abba151e"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/software-supply-chain-security-omdia-2026-report/"
published_at: "2026-08-04T15:10:16+00:00"
first_seen_at: "2026-08-04T15:12:55.559817+00:00"
fetched_at: "2026-08-04T15:13:53.558824+00:00"
content_hash: "sha256:ec9cd5dabcb24d8ba90a445e54ad04cf2d00442f60a64309062b8724050bc9b3"
---

# The Software Supply Chain Is Under Siege. Devs Are Still the First Line of Defense

*A new report from Omdia focuses on security issues in the software supply chain, how organizations are responding, and where the biggest gaps remain*


In the heat map of cybersecurity vulnerabilities today, among the most intense hot spots is the software supply chain. In fact, it was the shift of the modern attack surface away from isolated systems to the software supply chain that connects them—and Docker’s role in safeguarding that interconnected reality—[that first drew me to Docker](https://www.linkedin.com/pulse/why-i-joined-docker-security-center-software-supply-chain-docker-atcoc/) .


So when[Omdia](https://omdia.tech.informa.com/) recently released a report, with Docker among its sponsors, that laid out in detail the extent to which the software supply chain is under siege, I wanted to share some highlights.


## **Key data points**


Here are some data points that caught my attention:


- Over three-fourths of organizations experienced a software supply chain incident in the preceding 12 months.
- AI tech was the top-ranked supply chain risk (40%), ahead of third-party and open-source code (39%), and software dependencies (38%).
- Nearly half (45%) of orgs do not feel they have robust software supply chain security, compared to 55% who do.
- More than half of orgs (51%) rate secure containers as very effective in securing third-party and open-source code components.
- Shifting security left so that developers can secure their code is a high priority for 98% of organizations—and for 32% of those, it’s their top application security priority.


## **Third-party code and AI usage expand attack surface**


A key finding was that increasing usage of third-party code and AI adoption pose security risks that organizations need to address.


Building applications using third-party libraries, open source dependencies, and AI-generated code saves developers a ton of time, so it’s no surprise this trend is on the rise. But it’s code they didn’t write, and as these time-saving inputs keep growing, so do the attack surfaces they expose.


- 77% of organizations reported experiencing a software supply chain incident in the 12 months prior to the survey (carried out in February 2026).
- Notably, the most common attacks (38%) involved exploits that took advantage of known vulnerabilities in third-party software.


Source: Omdia Research Report, *Securing the Software Supply Chain: Strategic Approaches to Support Scaling Development with AI Adoption* , April 2026


## **Third-party code usage trending upward**


Third-party code usage, including open-source software, isn’t going away. In fact, it’s gaining momentum.


- 38% of organizations report that more than half of their total software code comes from third-party sources—expected to jump to 58% of organizations in 12 months.
- Similarly, 31% of orgs report more than half of their code is comprised of OSS—expected to jump to 51% of orgs in 12 months.


The report found that OSS is vital to developers and must be supported, and that orgs are either confident (50%) or completely confident (31%) that their developers are only using secure OSS.


## **AI tops security concerns**


It should come as no surprise that, as devs increasingly use AI tech to develop software, AI tops the list of concerns around software supply chain risks (40%), ahead of third-party code (39%) and software dependencies (38%).


In the rapidly evolving threat landscape, new types of cyber attacks are emerging that are very different from CVEs (Common Vulnerabilities and Exposures). Take the Shai-Hulud campaign pioneered by TeamPCP, which automates and scales software supply chain attacks using stolen credentials to weaponize well-known packages and inject infostealers deep into the ci/stack or developer laptops.


Using third-party software including OSS is problematic for orgs on multiple fronts. The most common challenges are around vulnerability management.


- Orgs worry about vulnerability remediation (39%) and/or identifying vulnerabilities in the code (36%).
- And, because AI tools often pull from third-party and OSS code, 35% worry about AI increasing or generating vulnerable code.


## **Current solutions often fall short**


There appears to be a fair degree of awareness around the need to secure the software supply chain. While many orgs are looking to bolster their software supply chain security, nearly half (45%) do not feel they have robust security in this area, compared to 55% who do.


At the risk of tooting our own horn, secure container services or libraries of hardened container images was the highest-rated tool for being “very effective” in securing third-party and OSS code components. In fact, out of 11 security tool categories, it was the only one rated as very effective by a majority of organizations (51%).


## **SBOMs play key role in boosting security**


Another key finding was that effective inventory and SBOM (software bill of materials) tools can lead to measurably better security outcomes.


SBOMs are essential because they eliminate structural blindness, providing transparency into the hundreds of third-party components that form the “ingredients” of a modern application. They are even more effective when paired with a VEX statement (Vulnerability Exploitability eXchange), which tells customers whether flagged vulnerabilities pose a risk or not—potentially saving security teams thousands of hours spent chasing “ghost” vulnerabilities.


According to the report, SBOMs help orgs manage software supply chain risk in a range of ways, including more efficient vulnerability mitigation (73%), implementing security controls and processes to mitigate risk (72%), and helping meet compliance regulations (68%).


However, among organizations that generate an SBOM as part of their application development processes, less than half (42%) do so as a mandatory part of the process for all applications. More than half (55%) generate SBOMs on a case-by-case basis.


Producing SBOMs and understanding code composition ranked fourth among challenges orgs face with using third-party software including OSS.


## **Action needed—fast**


The report underscores the need for preventative measures and rapid response in the face of a quickly evolving threat landscape. Among the impacts of software supply chain incidents are the following:


- Nearly half of orgs (46%) faced unauthorized access to applications and data.
- More than one-third had SLAs impacted by remediation steps (37%) and/or experienced stolen developer credentials, secrets, or keys (35%).
- Organizations also suffered loss of data, introduction of malware and ransomware, and fines for noncompliance.


These impacts underscore the need to mitigate risk as early as possible in the development lifecycle—ideally catching and remediating issues before applications are deployed.


## **Investment plans and shifting security left**


When asked about their spending plans in the face of these risks, orgs responded as follows:


- Nearly two-thirds (62%) expect to make significant investments in software supply chain security.
- 37% anticipate making more modest investments.


A final key finding was that investment plans prioritizing AI require collaboration across teams. That’s largely because the job of securing the software supply chain increasingly falls to those on the front line: developers.


In fact, shifting security left so that developers can secure their code is a high priority for 98% of organizations—and for 32% of those, it’s their top application security priority.


## **The need to support development**


One of the more resonant issues surfaced in the report was the need to support developers on the front lines. Despite the support for shifting security left to eliminate the security team as a bottleneck for remediating security issues, nearly half (45%) of security teams have only moderate or less influence over security products and processes for developers.


And while the majority of respondents believe their developers are mostly (38%) or completely (45%) comfortable taking on security responsibilities, orgs whose developers are less comfortable need to remove as much friction as possible from the process—for example, by making sure security tasks are not disruptive to the development process, and that security tools roll out consistently across development teams and work within development workflows.


The software supply chain isn’t getting simpler, and neither are the threats targeting it. If you’re evaluating how your organization can better secure third-party code, AI-generated code, and open source dependencies,[the full Omdia report](https://www.docker.com/resources/software-supply-chain-security-report/) offers a deeper look at the trends, data, and practical recommendations shaping software supply chain security. Download the report to see where your organization stands and where to focus next.
