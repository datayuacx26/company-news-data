---
schema_version: "1.0.0"
document_id: "a77cecec93be3231ec6340668cf114ffb87ec83ed8f7e9f4b7219b25a7390600"
company_key: "telos-corporation-common-stock"
company: "Telos Corporation"
source_id: "telos-corporation-common-stock-news-import-796eeee97fe2"
canonical_url: "https://www.telos.com/blog/2024/04/09/migrating-from-fedramp-rev-4-to-fedramp-rev-5/"
published_at: "2024-04-09T11:58:46+00:00"
first_seen_at: "2026-07-26T02:36:21.990964+00:00"
fetched_at: "2026-07-28T22:26:01.008185+00:00"
content_hash: "sha256:2aa91140f484df7983e8ace0be5aca9c932d92d3292b5ba8a327f27cf29e7112"
---

# Migrating from FedRAMP Rev. 4 to FedRAMP Rev. 5

FedRAMP, or the Federal Risk and Authorization Management Program, sets the bar for cloud security standards within the federal government. According to the[GSA’s website](https://www.gsa.gov/technology/government-it-initiatives/fedramp#:~:text=The%20Federal%20Risk%20and%20Authorization,for%20cloud%20products%20and%20services.) , the program “…provides a standardized approach to security assessment, authorization, and[continuous monitoring](https://www.telos.com/offerings/xacta-for-fedramp-continuous-monitoring/) for cloud products and services.”


The program was established in 2011; as of today, there are more than 330 authorized cloud service offerings.


Transitioning from Rev. 4 to Rev. 5 signifies an evolution in security protocols, reflecting a commitment on the part of the FedRAMP Program Management Office (PMO) to staying ahead of emerging threats and adapting to evolving cybersecurity landscapes. By undertaking this migration, organizations are maintaining the highest level of security measures mandated by FedRAMP. Rev. 5 introduced many changes across the security baseline, but the addition of the new Supply Chain Risk Management Control Family (SR) represents a significant change in the scope of FedRAMP protection.


We recently went through this process at Telos® when the Xacta® team migrated the technology behind the SaaS version of our[Xacta IT risk management platform](https://www.telos.com/offerings/xacta/) from the previous version (NIST 800-53 Rev. 4) to the newest version (NIST 800-53 Rev. 5).


The migration process itself was facilitated through Xacta. Xacta 360™ provides a comprehensive framework for navigating the complexities of FedRAMP compliance and simplifies the process of upgrading from an earlier version of a framework to a newer one.


Utilizing Xacta Data Exchange (XDE), we were quickly able to identify gaps between Rev. 4 and Rev. 5, which allowed our team to focus time and energy on the new additions to the framework. (XDE is an Xacta proprietary format that allows for the transmission of assessment data associated artifacts between different Xacta instances in a machine readable format.)


We leveraged control inheritance from our IaaS platform, which removed the need for implementation statements for almost 100 controls. XDE allowed us to identify which control from Rev. 4 matches with the Rev. 5 catalog and automatically copy implementation details over to the new framework.


Once the automatic migration of implementation statements was completed, the team was able to focus on implementation statements for the new Rev. 5 control family, Supply Chain Risk Management (SR).


Xacta 360 also supports the exporting of an SSP in Open Security Controls Assessment Language (OSCAL) format. (Telos is an early adopter of OSCAL and a[contributor in the development](https://www.telos.com/offerings/accelerate-fedramp-compliance-through-automatic-generation-of-fedramp-documentation/) of OSCAL tools/functionality.) As[FedRAMP states on their blog](https://www.fedramp.gov/FedRAMP-moves-to-automate-the-authorization-process/) , they have worked with NIST and other organizations to develop this standard, which “…can be applied to the publication, implementation and assessment of security controls.”


Incorporating FedRAMP Rev. 5 standards into the FedRAMP High Initiative represents a proactive approach by the FedRAMP program office to maintain parity with NIST SP 800-53, Rev. 5.


As cyber threats continue to evolve in sophistication and frequency, staying aligned with the latest FedRAMP requirements is essential for cloud services providers (CSPs) who work on an ongoing basis to maintain a strong defense posture for cloud products and services.


Organizations leveraging the Xacta IT and cyber risk management platform can confidently navigate the complexities of the migration process between versions of the NIST RMF and other major standards, knowing they are equipped with the necessary tools and expertise to uphold the highest measures of security and compliance and are able to document such compliance on an ongoing basis.


If you are interested in learning more about how your organization can benefit from using Xacta, please[reach out to request a demo](https://www.telos.com/offerings/xacta/xacta-demonstration-request/) .


[Xacta](https://www.telos.com/blog/all-posts/?topic=xacta)[cybersecurity](https://www.telos.com/blog/all-posts/?tag=cybersecurity)[FedRAMP](https://www.telos.com/blog/all-posts/?tag=fedramp)[NIST](https://www.telos.com/blog/all-posts/?tag=nist)


Michael McGehee


Project Manager Deputy


Michael McGehee joined Telos in May of 2020 and currently serves as Project Manager Deputy.


[Read full bio](https://www.telos.com/blog/contributors/#michael-mcgehee)
