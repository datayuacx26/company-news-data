---
schema_version: "1.0.0"
document_id: "9c4569d67ab55debdae65f5d427cee62049dcbaa4d659d6b2055df7cae9e3164"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-news-import-6fa26b21f70d"
canonical_url: "https://www.certara.com/blog/the-send-2023-updates-nonclinical-drug-developers-need-to-know/"
published_at: "2026-04-06T13:07:48+00:00"
first_seen_at: "2026-07-30T08:43:47.512613+00:00"
fetched_at: "2026-07-30T08:43:49.249261+00:00"
content_hash: "sha256:2ab1e2b52e49a7496d913031fee029f52599ddd0fcb13272aeabafe67d5f5fc0"
---

# CDISC SEND updates nonclinical drug developers need to know

ShareShareShare


Peggy Zorn Senior Director, Product Management


April 6, 2026


The[Standard for Exchange of Nonclinical Data (SEND)](https://www.cdisc.org/standards/foundational/send) is a standardized, electronic format for exchanging non-clinical drug development data. SEND was developed by the[Clinical Data Interchange Standards Consortium (CDISC)](https://www.cdisc.org/) , a globally recognized, not-for-profit organization that develops[data standards](https://www.certara.com/blog/a-guide-to-cdisc-standards-used-in-clinical-research/) with the input of pharmaceutical industry experts. The[U.S. Food and Drug Administration (FDA)](https://www.fda.gov/) requires drug developers to use the SEND standard because this consistent format allows regulatory agencies to streamline the review of the non-clinical sections of drug submissions.


Sponsors may struggle when navigating SEND data standard regulations because of a lack of knowledge of the requirements.


In this blog, I’ll discuss recent updates to SEND and other data standards that you should be aware of, which include:


- Changes in version 3.1.1 of SEND
- The introduction of requirements for SEND submissions to the FDA’s Center for Biologics Evaluation and Research (CBER)
- Expansion of the requirements for SEND Implementation Guide version 1 for[Animal Rule](https://www.certara.com/blog/the-animal-rule-what-if-a-clinical-trial-simply-isnt-ethical/) (SENDIG-AR V1.0)
- The introduction of requirements for SEND Implementation Guide version 1.1 for Developmental and Reproductive Toxicology (DART) studies (SENDIG-DART v1.1)
- The requirement for the latest version of the[Define-XML standard](https://www.certara.com/blog/using-define-xml-for-faster-better-quality-and-more-efficient-studies/) (Version 2.1)
- The introduction of requirements for SEND Implementation Guide V1.0 for Genetic Toxicology (SENDIG-Genetox V1.0) for In Vivo Micronucleus and Comet Assay Data
- Upcoming changes for SEND version 4.0, scheduled for publication by CDISC in Q4 2026.


### Latest CDISC SEND Requirements


As of 3/15/2023, there were several standard version requirement changes published in the[FDA Data Standards Catalog](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/data-standards-catalog-v90) that are related to SEND and nonclinical data and submissions.


- SEND Implementation Guide 3.1.1, the latest version of the SEND standard, became required for studies with start dates after 3/15/2023.
- SEND 3.1 or SEND 3.1.1 datasets are required for submissions to the FDA Center for Biologics Evaluation and Research (CBER) for studies starting after 3/15/2023. Previously only FDA Center for Drug Evaluation and Research (CDER) submissions required[SEND datasets](https://www.certara.com/blog/whats-driving-the-increased-demand-for-send-dataset-services/) .
- SEND Implementation Guide for Animal Rule V1.0 (SENDIG-AR V1.0) is required for[Investigational New Drug (IND) submissions](https://www.certara.com/services/send-dataset-services-for-fda-ind-submissions/) for studies starting after 3/15/2023 submitted to CDER. The requirement for this standard for studies included in NDA submissions submitted to CDER started 3/15/2022.
- SEND Implementation Guide for Developmental and Reproductive Toxicology studies (DART) V1.1 (SENDIG-DART v1.1) is required for embryo-fetal development (EFD) studies starting after 3/15/2023 that are included in NDA submissions. The requirement for IND submissions for SENDIG-DART V1.1 took effect 3/15/2024.
- V2.1 of the define.xml standard is required for studies starting after 3/15/2023. Define.xml files are required as part of all SEND dataset submission packages.


Of note, both SEND 3.1.1 and define.xml 2.1 are now required for studies starting after 3/15/2023. However, because support and requirement end dates haven’t been published for the prior versions of the SEND or define standards in the FDA Data Standards Catalog, either SEND 3.1 or SEND 3.1.1 or define 2.0 or define 2.1 are acceptable, although the latest versions of the standards are preferred.


In December 2023, FDA updated the Data Standards Catalog to include SEND Implementation Guide V1.0 for Genetic Toxicology (SENDIG-Genetox V1.0) with Date Support Begins and Date Requirement Begins. The FDA started support for SENDIG-Genetox V1.0 on 12/13/2023, and the requirement began for studies starting after 3/15/2025.


In April 2024, the FDA updated the Data Standards Catalog to include SEND Implementation Guide for Animal Rule V1.0 (SENDIG-AR V1.0) with Date Support Begins and Date Requirement Begins for studies submitted to CBER. CBER support for SENDIG-AR V1.0 started on 3/26/2024, and the requirement will begin for studies starting after 3/15/2027.


White paper


Navigate FDA SEND requirements with confidence


Learn how to streamline SEND submissions and support compliance with expert insights and best practices.


[Access the white paper](https://www.certara.com/white-paper/fda-send-dataset-requirements-for-nonclinical-studies/)


## CDISC SEND V3.1.1


SEND IG Version 3.1.1 was originally released in March of 2021 and primarily covers changes to the Pharmacokinetic Concentration and Pharmacokinetic Parameter domains (PC and PP domains) to ensure that creation of time/concentration curves could be done by the FDA using the data from these two domains.


To support this, there are changes to variable permissibility, assumptions, and examples (including cross domain examples) to make it clear what data is expected to be populated for the data to be used as intended. Most notably, the PCELTM and PCTPTREF variables were changed to Expected rather than Permissible and PCDTC was changed from Expected to Permissible. In addition, the unscheduled flag variable was added to the PC domain to support flagging of unscheduled results.


## CDISC SEND for CBER


For many years, the SEND data requirements only applied to CDER submissions. As of March 15, 2023, the SEND standard also applies to CBER submissions.


The study and data types included for CBER are the same as for CDER, except for the requirement for SENDIG-DART v1.1 for embryo-fetal development studies, which is not required for CBER. In-scope study types for CBER include single, repeat dose, carcinogenicity, and cardiovascular and respiratory safety pharmacology studies. The data types required are those modeled in CoDEx V1.0 for SEND V3.1.


The FDA has included information in the Technical Conformance Guide specifically related to the handling of immune response data for CBER submissions in SEND 3.1 or SEND 3.1.1. It is not modeled in this version of the SEND IG but will be included in the next version as the Immunogenicity Specimen Assessments (IS) domain is planned to be included in SEND V4.0.


In the meantime, CBER prefers that immune response data be included in either the LB domain or in the IS domain as a custom domain for SEND 3.1/SEND 3.1.1. Custom domains are supported starting with SEND v.3.1.


## SENDIG-AR V1.0


SEND Implementation Guide for Animal Rule V.1.0 (SENDIG-AR V1.0) became a requirement for studies starting after 3/15/2022 included in NDA submissions to CDER. As of 3/15/2023, the requirement was extended to IND submissions to CDER as well.


In April 2024, the FDA updated the Data Standards Catalog to include SENDIG-AR V1.0 with Date Support Begins and Date Requirement Begins for studies submitted to CBER. CBER support for SENDIG-AR V1.0 started on 3/26/2024 and the requirement will begin for studies starting after 3/15/2027.


Studies submitted under the Animal Rule differ from general toxicology studies in that the[Animal Rule applies to using adequate and well-controlled studies in animal models rather than in humans](https://www.certara.com/blog/the-animal-rule-what-if-a-clinical-trial-simply-isnt-ethical/) for situations where conducting clinical trials in humans is unethical. These conditions are life-threatening and related to exposure to lethal or permanently disabling toxic chemical, biological, radiological, or nuclear substances.


While typical toxicology studies submitted as part of NDA or IND applications focus on testing the toxic effects of treatments, Animal Rule studies focus on the natural history of diseases and testing treatment safety and efficacy in animal models.


SENDIG-AR V1.0 introduces several new domains and variables to the SEND model as well as updates for SEND-controlled terminology.


## SENDIG-DART V1.1


Embryo-fetal development studies started on or after 3/15/2023 submitted in NDA applications to CDER require SEND datasets. The requirement for INDs took effect 3/15/2024.


Embryo-fetal development studies are modeled in the SEND implementation guide for Developmental and Reproduction Toxicology V1.1 (SEND-IG DART V1.1). This IG contains new domains for pregnancy and fetal findings, new timing variables to track results relative to gestation start, and significant additions to SEND controlled terminology requirements.


The FDA has also provided clarification on requirements for SEND-IG DART V.1.1 in the[Technical Conformance Guide](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/study-data-technical-conformance-guide-technical-specifications-document) . The clarifications are helpful in determining specific SEND requirements for common development scenarios.


SENDIG-DART V1.2 was released in June 2023 and includes modeling and examples for general toxicology studies conducted on juvenile animals, as well as the addition of the Developmental Milestones (DP) domain. It’s unknown when this updated version of the DART IG will be supported or required by FDA. But if that does happen, it will be posted in the FDA Data Standards Catalog.


FDA Technical Rejection Criteria is not currently implemented for[eCTD](https://www.certara.com/globalsubmit-ectd-submission-software/) module 4.2.3.5.2, which is where Embryo-Fetal Development studies are located. So simplified ts.xpt domains do not need to be submitted to indicate SEND datasets are not required for a study. If you do submit a simplified ts.xpt domain in this eCTD module, the FDA has confirmed that it will not interfere or prevent electronic receipt or validation.


## Define.xml V2.1


Define.xml V2.1 is an FDA requirement for studies starting after 3/15/2023. This latest version of define.xml is like V2.0, with notable changes listed above.


- More than one data standard can be referenced, using the new def:Standards element. Thus, for SEND studies where you may be using SEND V3.1 for most domains but SDTM for a custom domain, both standards can be referenced.
- Introduction of SubClass, which allows programmers to provide a more descriptive classification of their datasets. As of today, the only defined use cases for this concept exist for[Analysis Data Model (ADaM)](https://www.certara.com/blog/3-things-you-should-know-about-adam-standards/) submissions.
- New method for identifying empty datasets and variables with no values. This is flagged with a new attribute called def:HasNoData on both the ItemGroupDef element (for datasets) and the ItemRef element (for variables).
- ORIGINs were also enhanced to include both Type and Source (although for SEND datasets, only ORIGIN Type is used).


A full list of changes in Define 2.1 is included in the Define 2.1 specification, available from the[CDISC website](https://www.cdisc.org/standards/data-exchange/define-xml) .


## SENDIG-Genetox V1.0


The Standard for Exchange of Nonclinical Data Implementation Guide: Genetic Toxicology V1.0 (SEND-IG Genetox V1.0) was published by CDISC on 06/28/2023. The FDA added SEND-IG Genetox 1.0 to the Data Standards Catalog on 12/13/2023, with support for the new IG for both CDER and CBER started on 12/13/2023 and as a requirement for all studies with start dates after 03/15/2025.


Study types supported in SEND-IG Genetox V1.0 include In Vivo Micronucleus and Comet Assay. SEND-IG Genetox V1.0 provides data examples from each of the supported study types and details associated controlled terminology requirements.


## What’s coming for CDISC SEND


When we look ahead for SEND, the biggest change coming is related to the finalization and implementation of the next major version of SEND, V4.0. This version is still in development with CDISC but is currently projected to be published as a final standard in Q4 2026.


SEND V4.0 has significant changes, including the addition of several new domains covering in-vivo genetic toxicology, cell phenotyping, immunogenicity, ophthalmology examinations, pharmacokinetic inputs, nervous system test results, skin test results, and scoring scales. Additional changes planned to the existing standard include an update of the Microscopic Findings (MI) domain to include targeted staining, sexual maturity, and reproductive cycle results, new variables for result modifiers, category/subcategory, and time to detection as well as depreciation of the Tumor Findings (TF) domain.


At this point, CDISC’s final publication date for SEND 4.0 is planned for Q4 2026. Although this could change based on feedback from the public review comments and other factors. For more information regarding SEND 4.0 or to get involved in the development and finalization of this standard, visit the[CDISC SEND](https://www.cdisc.org/standards/foundational/send) website.


Does your non-clinical drug program need data analysis and visualization support? S[END Explorer®](https://www.certara.com/send-explorer-software/) is a validated, web-based application that provides advanced viewing, data summarization, and visualization capabilities for nonclinical study data. Using SEND Explorer, scientists can generate single- and multi-study visualizations to make data-driven decisions and quickly address questions from development teams and regulatory authorities.


White paper


FDA SEND Dataset Requirements for Nonclinical Studies


This white paper gives an overview of how to submit datasets in SEND format. It also offers valuable insights into how to streamline your submission and avoid common pitfalls.


[Read the white paper](https://www.certara.com/white-paper/fda-send-dataset-requirements-for-nonclinical-studies/)


## Author


[Peggy Zorn](https://www.certara.com/teams/peggy-zorn/) Senior Director, Product Management


Peggy is the Customer Success lead for SEND Explorer. She has a strong background in nonclinical data management, warehousing and the SEND standard, including serving as the lead of the CDISC SEND controlled terminology team for over 7 years. She continues to be involved in the CDISC SEND team as well as the PhUSE nonclinical working group.


Prior to joining Certara, Peggy worked for 15 years at Pfizer, with 6 of those years focused on nonclinical data management and warehousing. She also worked for INDS for several years, which is the company that originally developed the SEND Explorer dataset reviewer and visualization tool.


***Author’s note:** this blog post was originally published in October 2023 and has been updated for accuracy and comprehensiveness.*


## Learn more about SEND Explorer


Access and visualize nonclinical data for confident decisions


[View SEND Explorer](https://www.certara.com/send-explorer-software/)


## Contact us


## You May Also Like


AllCertara Products - SEND Explorer


[Unlock the Power of SEND Data: From FDA Compliance to Global Regulatory Readiness for Preclinical data \[Korean Webinar\]](https://www.certara.com/webinar/unlock-the-power-of-send-data-from-fda-compliance-to-global-regulatory-readiness-for-preclinical-data-korean-webinar/)


[Unlock the Power of SEND Data: From FDA Compliance to Global Regulatory Readiness for Preclinical data \[Korean Webinar\]](https://www.certara.com/webinar/unlock-the-power-of-send-data-from-fda-compliance-to-global-regulatory-readiness-for-preclinical-data-korean-webinar/)[Webinar](https://www.certara.com/category/webinar/)


### [Unlock the Power of SEND Data: From FDA Compliance to Global Regulatory Readiness for Preclinical data \[Korean Webinar\]](https://www.certara.com/webinar/unlock-the-power-of-send-data-from-fda-compliance-to-global-regulatory-readiness-for-preclinical-data-korean-webinar/)


[2026 SEND Explorer Virtual User Group Meeting](https://www.certara.com/live-events/2026-send-explorer-virtual-user-group-meeting/)


[2026 SEND Explorer Virtual User Group Meeting](https://www.certara.com/live-events/2026-send-explorer-virtual-user-group-meeting/)[Live Events](https://www.certara.com/category/live-events/)


### [2026 SEND Explorer Virtual User Group Meeting](https://www.certara.com/live-events/2026-send-explorer-virtual-user-group-meeting/)


[Virtual Control Groups, Actually: What SEND Makes Possible (and What It Doesn’t)](https://www.certara.com/on-demand-webinar/virtual-control-groups-actually-what-send-makes-possible-and-what-it-doesnt/)


[Virtual Control Groups, Actually: What SEND Makes Possible (and What It Doesn’t)](https://www.certara.com/on-demand-webinar/virtual-control-groups-actually-what-send-makes-possible-and-what-it-doesnt/)[On-Demand Webinar](https://www.certara.com/category/on-demand-webinar/)


### [Virtual Control Groups, Actually: What SEND Makes Possible (and What It Doesn’t)](https://www.certara.com/on-demand-webinar/virtual-control-groups-actually-what-send-makes-possible-and-what-it-doesnt/)
