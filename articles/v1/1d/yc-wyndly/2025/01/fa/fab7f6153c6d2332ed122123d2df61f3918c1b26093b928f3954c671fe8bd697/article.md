---
schema_version: "1.0.0"
document_id: "fab7f6153c6d2332ed122123d2df61f3918c1b26093b928f3954c671fe8bd697"
company_key: "yc-wyndly"
company: "Wyndly"
source_id: "yc-wyndly-news-import-0d4c71359e34"
canonical_url: "https://www.wyndly.com/blogs/learn/alex-bargar-hipaa-for-startups"
published_at: "2025-01-16T01:30:56+00:00"
first_seen_at: "2026-07-22T20:39:46.609626+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:a0ccbb116f30e7228f34408a880b0d2564c6ce20289d0e28bdebd60567b3bdde"
---

# HIPAA for Startups with Alex Bargar, Cofounder of TeachMeHIPAA and Juno Medical

*This post is authored by Alex Bargar.*


**


Alex Bargar is the cofounder or was on the founding team of several healthcare startups, including


[TeachMeHIPAA (a HIPAA training platform)](https://teachmehipaa.com/) , Juno Medical (a tech enabled chain of family healthcare clinics), and Simple Health (a DTC women’s healthcare company). Alex has a decade of experience administering HIPAA compliance programs in high growth startups.


Navigating the intricate world of healthcare compliance can be challenging for entrepreneurs embarking on new startups in this sector. One of the key regulations that healthcare founders must understand is the Health Insurance Portability and Accountability Act (HIPAA), designed to safeguard individuals' medical information. In this Q&A, we shine a light on how to assess your organizational responsibilities under HIPAA, and provide pragmatic advice for how to move quickly without exposing your patients to privacy risks.


**What is HIPAA?**


HIPAA, which stands for the Health Insurance Portability and Accountability Act, is a federal law enacted in the United States in 1996. It establishes a set of national standards to protect sensitive patient health information from being disclosed without the patient's consent or knowledge. These standards apply to all forms of Protected Health Information (PHI) – whether electronic, written, or oral. The primary goal of HIPAA is to ensure that individuals' health information is properly protected, while allowing the flow of health information needed to provide high-quality health care. It encompasses a broad range of data privacy and security provisions to ensure the confidentiality, integrity, and availability of electronic protected health information.


**I’m building in healthcare. Do I need to comply with HIPAA?**


Short answer:


*maybe* .


Longer answer: The rules and requirements of HIPAA only apply to entities that are considered Covered Entities, or Business Associates. We’ll dive momentarily into how to assess whether or not you really are a Covered Entity or a Business Associate.


**So how do I know if I’m a covered entity?**


The Department of Health and Human Services (HHS) has created a helpful table (reproduced below).


**A Healthcare Provider**


**A Health Plan**


**A Health Care Clearinghouse**


This includes providers such as:


- Doctors


- Clinics


- Psychologists


- Dentists


- Chiropractors


- Nursing Homes


- Pharmacies


...but only if they transmit any information in an electronic form in connection with a transaction for which HHS has adopted a standard.


This includes:


- Health insurance companies


- HMOs


- Company health plans


- Government programs that pay for health care, such as Medicare, Medicaid, and the military and veterans health care programs


This includes entities that process nonstandard health information they receive from another entity into a standard (i.e., standard electronic format or data content), or vice versa.


Source:


[https://www.hhs.gov/hipaa/for-professionals/covered-entities/index.html](https://www.hhs.gov/hipaa/for-professionals/covered-entities/index.html)


You’ll note that at a high level, HIPAA applies to healthcare providers, insurance companies, and healthcare clearinghouses. But the


*‘only if they transmit any information in an electronic form in connection with a transaction for which HHS has adopted a standard.’* modifier is a critical one. This clause specifically refers to the transmission of insurance claims.


**So a healthcare provider that does not bill insurance is not subject to the rules and requirements of HIPAA** .


If you’re building in the healthcare space and are neither a provider, payer, or clearinghouse, you should still consider whether you may be a Business Associate.


**OK - so am I a Business Associate?**


First, to be a Business Associate, you must work on behalf of either a Covered Entity or another Business Associate. If you don’t, then you’re not. If you do, you’ll need to look at the type of information you receive in the course of conducting business. If any of the information you receive is protected health information, you’re a Business Associate and do need to comply with the rules and requirements of HIPAA.


**What is PHI?**


PHI is


is information, including demographic data, that relates to:


- the individual's past, present or future physical or mental health or condition,


- the provision of health care to the individual, or


- the past, present, or future payment for the provision of health care to the individual,


and that identifies the individual or for which there is a reasonable basis to believe it can be used to identify the individual.


Put simply, PHI is information that is identifiable, and that relates to an individual's health (at any time), any treatment they have received, or payment for care received. Understanding what makes healthcare information “identifiable” is critical to assessing whether or not you are receiving, creating, or using PHI.


**I need to comply with HIPAA - now what?** **** There’s a popular misconception that if your organization must comply with HIPAA, everything slows to a crawl. There are a large number of tools and resources you can use to assist in complying with HIPAA that exist on a broad spectrum of cost and complexity. Though in the most basic form, complying with HIPAA means the following:


1. Adopt a set of internal security & privacy policies that inform your staff on how they must manage and protect patient health information


2. Publish a patient-facing Notice of Privacy Practices (NPP) which clearly outlines to patients how you may use and disclose their PHI


3. Train your staff regularly around the rules and requirements of HIPAA compliance (this is where solutions like TeachMeHIPAA come into play with our


[low cost HIPAA training solution](https://teachmehipaa.com/) )


4. Execute a Business Associate Agreement (BAA) with any third party who must access PHI in order to provide you with a product or service


Many companies comply with HIPAA in part by implementing sophisticated software locks on employee machines, adopting an extremely conservative posture for which vendors they work with, and by creating extensive security steps required for patients to access their own sensitive data.


**But these are not explicitly mandated.** **** **** For early stage healthcare companies who must comply with HIPAA, I recommend finding templatized security & privacy policies, notice of privacy practices, and BAA’s, and reviewing them carefully to determine what’s feasible for you to implement vs. what’s not. It’s critical to ensure that only authorized users can access PHI, that you’re not sharing PHI with vendors who haven’t signed a BAA, and that you respect patient’s rights with respect to their PHI. But you have flexibility with respect to how you implement many of the rules and requirements of HIPAA.


**I’m not a Covered Entity or Business Associate - can I do anything I want?**


No! First, consider whether you plan to transition into being a Covered Entity or Business Associate in the future.


If not, familiarize yourself with state laws around healthcare privacy, as many states have their own rules and requirements around data privacy to which you may still be subject. Oftentimes, adopting internal policies that mirror those required under HIPAA can be a way to reduce the likelihood you’re falling afoul of any state law, as they are rarely if ever more stringent than HIPAA. From a practical standpoint, this looks like reviewing templatized HIPAA policies and adopting as many of them as you can internally to ensure best practices are being followed.


If you are planning to become a Covered Entity or Business Associate in the future (for example, if you’re currently a cash only healthcare provider who plans to accept insurance down the road), in addition to the above guidance, you should build your tech/platform stack carefully to ensure you don’t develop a reliance on sharing healthcare information with vendors who cannot execute a BAA. Here are some common examples.


1. For internal communications, Slack can support HIPAA compliance, while Facebook Workplace cannot. So when selecting a messaging platform, select one capable of a HIPAA upgrade at the outset to prevent costly platform transitions or product redesign


2. For CRM, consider selecting an EHR, or a HIPAA compliant CRM at the outset like Salesforce, instead of using an off the shelf solution without any HIPAA compliance capability


3. For corporate email, select a solution like Google Workspace who can upgrade to HIPAA compliance at no extra cost


**What are some red flags and things to avoid?**


Regardless of your HIPAA obligations, there are a few common practices in 2023 that you should avoid to play it as safe as possible.


1. Advertising pixels & other tracking mechanisms - many contemporary advertising platforms make it easy to send extensive data about who interacts with your platform/product and how.


**Be careful about exposing any potential PHI to an advertising partner**
2. Data sharing or sales - finding ways to monetize your data can be tempting, especially in the era of AI.


**Sharing or selling PHI without authorization if not for an allowable reason under HIPAA can be a serious offense** , and can run afoul of state laws or regulations


**How does my approach to HIPAA change as I grow?**


When first getting your business off the ground, you should be able to leverage publicly available templated policy documents and low cost training to substantially reduce your risk of HIPAA non compliance. As your practice scales, or your business operations become more sophisticated, ensuring that your policies are extremely well calibrated (and upheld) becomes increasingly more important to reduce the likelihood of a breach. Around the Series A stage I often advise HIPAA-compliant startups to conduct an internal audit, and to consider engaging a consultant to dial in your internal security & privacy policies. Consider more sophisticated HIPAA compliance platforms at this stage as well, such as Vanta or SecureFrame who can help automate a variety of checks to ensure ongoing compliance on the technical side.


**Wrapping up**


Compliance exists on a spectrum, and the same is true of HIPAA compliance. While it’s important to understand your obligations under HIPAA (and whether you even have any), taking straightforward steps to protect patient data is something any founder can (and should) do. There are a wealth of free resources available on how to comply with HIPAA, including what types of policies you can adopt internally and which vendors you can expect to safely work with.
