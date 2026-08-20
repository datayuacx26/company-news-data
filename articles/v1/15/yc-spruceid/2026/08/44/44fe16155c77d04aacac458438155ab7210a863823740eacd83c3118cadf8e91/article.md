---
schema_version: "1.0.0"
document_id: "44fe16155c77d04aacac458438155ab7210a863823740eacd83c3118cadf8e91"
company_key: "yc-spruceid"
company: "SpruceID"
source_id: "yc-spruceid-rss-1607485977f6"
canonical_url: "https://blog.spruceid.com/recommendations-to-cms-on-medicaid-verification/"
published_at: "2026-08-04T21:07:00+00:00"
first_seen_at: "2026-08-07T23:59:47.766381+00:00"
fetched_at: "2026-08-07T23:59:49.724989+00:00"
content_hash: "sha256:ee7af83ed75ddb5f8e8d9cb3a506712e942f8d677c0b642486e29dba13398ad9"
---

# Recommendations to CMS on Medicaid Verification

The Centers for Medicare & Medicaid Services (CMS) recently issued an interim final rule implementing new Medicaid community engagement requirements. States will need to establish processes for verifying qualifying activities and exemptions while balancing administrative cost, program integrity, beneficiary burden, and the risk of erroneous coverage loss.


SpruceID submitted comments to CMS focused specifically on this implementation challenge. We did not take a position on the merits of the statutory community engagement requirement itself. Instead, we focused on how states can verify compliance and exemptions without creating avoidable procedural barriers for eligible individuals.


Our recommendations start from a simple principle: states should use reliable information already available to them wherever possible. When existing data cannot resolve a case, individuals should have additional ways to provide reliable evidence, including, where appropriate, standards-based verifiable digital credentials. Those digital pathways should expand existing options, not become new requirements.


## The stakes of getting verification right


The design of these verification processes will materially affect both coverage outcomes and administrative workload. Recent Medicaid experience shows why. During the unwinding of the continuous enrollment provision,[more than 25 million people](https://www.kff.org/medicaid/medicaid-enrollment-tracker/?ref=blog.spruceid.com) were disenrolled, and approximately 69 percent of those disenrollments were classified as procedural. In Arkansas’s 2018 work-requirement implementation,[more than 18,000 adults](https://www.kff.org/medicaid/state-data-for-medicaid-work-requirements-in-arkansas?ref=blog.spruceid.com) lost coverage for failure to meet reporting requirements in less than one year. Subsequent research found that[more than 95 percent](https://www.nejm.org/doi/full/10.1056/NEJMsr1901772?ref=blog.spruceid.com) of the target population appeared either to satisfy the requirement or qualify for an exemption.


The scale of the new requirement makes those lessons especially important. Forty-three states must implement the requirement, and roughly 20 million expansion enrollees may be subject to it. States estimate that existing data matching may verify compliance or exemption for only[60 to 80 percent](https://www.kff.org/medicaid/an-early-look-at-policy-decisions-as-states-get-ready-to-implement-work-requirements?ref=blog.spruceid.com) of affected enrollees.


Ex parte verification can and should resolve as many cases as possible without requiring action from beneficiaries. But even effective data matching will leave a meaningful number of cases unresolved, including individuals whose work, income, or exemption status may not be consistently represented in conventional government or payroll datasets.


Our eight recommendations below focus both on maximizing data-first verification and on creating reliable, low-burden pathways for the cases it cannot resolve.


### **1. Maximize ex parte verification, automatically apply supported exemptions, and preserve self-attestation where documentation is not reasonably available**


Ex parte verification should remain the primary mechanism for determining whether an individual satisfies a requirement or qualifies for an exemption.


There is evidence that better use of existing data can materially reduce administrative burden. In Minnesota, Code for America’s work expanded ex parte consideration to[approximately 70 percent](https://codeforamerica.org/news/human-centered-ex-parte-benefits-renewals?ref=blog.spruceid.com) of a caseload that had previously been renewed manually and reduced worker time per renewal from about 70 minutes to 15 minutes.


Where reliable federal or state data already establishes the relevant fact, states should use that information before requesting documentation from the individual. Similarly, where available data establishes that someone qualifies for a mandatory exemption, the state should apply that exemption without requiring a separate application or duplicative submission.


We also encouraged CMS to preserve flexibility around self-attestation. CMS should consider extending the period during which states may rely on self-attestation when reliable electronic data is unavailable, or clarify that documentation is not “reasonably available” when obtaining it would impose a disproportionate burden or create a foreseeable risk of erroneous termination.


Beneficiary-submitted evidence should serve as an additional pathway for cases that cannot be resolved through authoritative data, not as a substitute for improving state verification systems.


### **2. Recognize pre-authorized, consent-based data sharing as ex parte verification**


Relevant information may also be held outside government systems, including by employers, payroll providers, ride-sharing platforms, peer-to-peer tasking platforms, or services that hold verifiable digital credentials on an individual’s behalf.


We recommended that CMS recognize certain pre-authorized, consent-based data-sharing arrangements as ex parte verification when the information is directly available to the agency and the individual does not need to take action at the time of the eligibility review.


For example, an individual could authorize a specific source in advance to share a limited set of information with a state agency for eligibility determination. When that information is later needed, the agency could retrieve it through the authorized pathway and verify its source and integrity.


Consent is essential to this model. Individuals should be able to revoke an authorization at any time. Revocation should route them to another verification pathway rather than be treated as evidence of noncompliance.


These arrangements should also follow strong data-minimization principles. States should request, receive, and retain only the attributes necessary for the eligibility determination and applicable audit requirements, rather than complete credentials, transaction histories, device identifiers, or unrelated underlying records when a narrower verification result is sufficient.


### **3. Recognize standards-based verifiable digital credentials as an optional form of beneficiary-submitted evidence**


When a case cannot be resolved through ex parte verification, individuals may need to provide documentation themselves.


We recommended that CMS clarify that states may accept standards-based verifiable digital credentials as one optional form of beneficiary-submitted evidence when those credentials are issued by an authorized source and contain the information necessary for the eligibility determination.


A[verifiable digital credential](https://spruceid.com/learn/vdc-intro?ref=blog.spruceid.com) is a digitally signed record issued by an entity such as an employer, payroll provider, educational institution, community service organization, health care provider, or government agency. Its[digital signature](https://spruceid.com/learn/digital-signatures?ref=blog.spruceid.com) allows a receiving system to verify the issuer and confirm that the information has not been altered.


CMS and states would still need to define the trust framework around those credentials: which issuers are authorized to attest to particular facts, which data elements are accepted, and how expiration, revocation, status, and audit requirements are handled.


Use of digital credentials should remain voluntary. They should supplement, not replace, paper, telephone, in-person, caseworker-assisted, and other existing pathways.


A credential validation failure also should not, by itself, support denial or termination. If validation cannot be completed because of technical unavailability, expiration, issuer-status uncertainty, or format incompatibility, the state should provide a reasonable opportunity to use another verification pathway. An inability to complete electronic validation is not the same as an affirmative determination of noncompliance.


### **4. Permit authorized providers and agencies to issue reusable exemption attestations**


Some exemptions may remain valid across multiple eligibility reviews. Requiring an individual to repeatedly obtain the same underlying documentation can create unnecessary work for beneficiaries, providers, agencies, and eligibility staff.


We recommended that CMS permit states to accept, at the individual’s election, a verifiable digital credential from an authorized health care provider, government agency, or other approved source attesting that the individual qualifies for a specified exemption.


Importantly, the attestation should contain only what is needed for the eligibility determination, for example, the applicable exemption category, validity period, issuer, and expiration or review date,[without requiring](https://spruceid.com/learn/selective-disclosure?ref=blog.spruceid.com) disclosure of underlying clinical or personal information that is not necessary.


This could allow an individual to reuse a current exemption attestation rather than repeatedly requesting equivalent documentation.


CMS should also distinguish between permanent or long-term exemptions and time-limited exemptions, such as pregnancy or temporary medical circumstances, and establish validity and renewal rules appropriate to each category.


### **5. Support optional cross-program status attestations where program rules align**


People interacting with Medicaid may also participate in programs such as SNAP or TANF, and in some circumstances a determination under one program may be relevant to the Medicaid community engagement requirement.


We recommended that CMS coordinate with the Food and Nutrition Service and the Administration for Children and Families to define a common, optional status attestation where program rules align.


A participating agency could issue a signed credential indicating that, for a defined period, an individual has been determined to satisfy or be exempt from a specified requirement. The individual could then elect to present that attestation to another participating program or state.


The receiving program would still evaluate the attestation under its own applicable rules. This is not a proposal for automatic cross-program acceptance where legal standards differ.


The goal is to reduce duplicative collection when an existing determination is sufficient. A beneficiary-mediated model may also reduce the need for agencies to build a new point-to-point data exchange every time similar information needs to move between programs, while leaving existing interagency data-sharing processes intact.


### **6. Permit federal modernization funding to support interoperable evidence exchange**


States will need technical capabilities to implement these verification processes effectively.


We recommended that CMS clarify that Government Efficiency Grants and applicable federal matching funds may support open, interoperable capabilities for issuing, receiving, validating, and managing digital evidence used in eligibility determinations.


Potential investments could include issuer trust registries, credential validation services, beneficiary presentation tools, caseworker interfaces, credential status services, accessibility testing, and integration with existing eligibility systems.


Funding conditions should require conformance with published standards, portability across vendors, appropriate privacy and security controls, preservation of nondigital pathways, and transparent performance reporting.


Success should be measured by operational and beneficiary outcomes, not simply by deployment of new technology. Relevant measures include ex parte verification rates, processing time, administrative cost, successful resolution of documentation requests, appeals, and procedural denial rates.


### **7. Establish voluntary state pilots before the 2028 documentation transition**


The period before 2028 offers an opportunity to test these approaches before the post-2027 documentation requirements take effect.


We recommended that CMS invite interested states to pilot optional credential-based evidence submission during 2027.


Each pilot should preserve all existing verification pathways, define authorized credential issuers and accepted data elements, establish accessibility and privacy safeguards, and interoperate with existing federal and state services rather than creating a separate eligibility system.


Pilots should also include an evaluation plan that measures processing time, successful verification, eligibility-worker effort, appeals, procedural denials, and beneficiary experience. The objective should be to understand whether these approaches reduce burden and improve verification in practice before they are adopted more broadly.


Participation must remain voluntary. Neither Medicaid coverage nor participation in another public benefit program should depend on enrolling in a pilot or possessing a digital identity or credential.


### **8. Apply proportionate identity assurance without mandating a particular provider, application, or biometric method**


Digital submission of evidence may require identity and authentication controls, but those controls should be proportionate to the risk of the transaction and should not create a new barrier to Medicaid eligibility.


We recommended that CMS allow states to accept multiple equivalent approaches consistent with the CMS Interoperability Framework and NIST’s[Digital Identity Guidelines](https://pages.nist.gov/800-63-4/?ref=blog.spruceid.com) rather than requiring a single identity provider, application, or biometric method.


Depending on the use case, those approaches could include appropriately assured state accounts and mobile driver’s licenses, among other methods.


States should also preserve assisted and nondigital alternatives for individuals who lack conventional identity documents, smartphones, reliable connectivity, or the ability to complete remote identity proofing. Biometric verification should not be required as the sole means of access.


Where printable or offline digital evidence is supported, states should establish clear validation and status-checking requirements before that evidence is used in an adverse eligibility decision.


## **Expanding verification options without creating new requirements**


Taken together, our recommendations are intended to expand the ways individuals can establish compliance or exemption status, not replace existing processes or make digital credentials a prerequisite for Medicaid.


The primary approach should remain maximized ex parte verification and automatic application of supported exemptions. When existing government data cannot resolve a case, states should be able to accept additional forms of reliable evidence, including optional standards-based verifiable digital credentials issued within a defined trust framework.


Existing standards, including the[W3C Verifiable Credentials](https://spruceid.com/learn/w3c-vc?ref=blog.spruceid.com) Data Model, the[ISO/IEC 18013](https://spruceid.com/learn/iso-18013-5?ref=blog.spruceid.com) series,[OpenID for Verifiable Presentations](https://spruceid.com/learn/oid4vp?ref=blog.spruceid.com) , and NIST’s Digital Identity Guidelines, provide many of the technical building blocks for this approach.


But technical standards alone are not enough. CMS and participating states would still need to establish the program-specific rules that make the system trustworthy: who is authorized to issue particular evidence, which claims can be accepted, how long evidence remains valid, how revocation and status checking work, what must be retained for audit purposes, and how disputed or unavailable credentials are handled.


These implementation decisions will determine whether the new verification processes add another layer of administrative burden or create an opportunity to make eligibility determination more efficient and reliable.


Eligible individuals should not lose coverage solely because an authoritative record is unavailable through automated data matching or because they cannot successfully navigate a documentation process. Maximizing ex parte verification, while providing additional reliable evidence pathways for unresolved cases, can help states meet their verification responsibilities without making the process itself an unnecessary barrier.


Our full comments to CMS provide additional detail on these recommendations, their implementation considerations, and the standards that could support them. The comments are available upon request at hello@spruceid.com.


Building digital services that scale take the right foundation.


[Talk to our team](https://spruceid.com/contact-us?ref=blog.spruceid.com)


**About SpruceID** : SpruceID builds digital trust infrastructure for government. We help states and cities modernize identity, security, and service delivery — from digital wallets and SSO to fraud prevention and workflow optimization. Our standards-based technology and public-sector expertise ensure every project advances a more secure, interoperable, and citizen-centric digital future.


Share
