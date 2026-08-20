---
schema_version: "1.0.0"
document_id: "4bfc2fde476cf6af9de3ea9df99bc5c6beac6130f836dbb2b217626a8a30f53c"
company_key: "verisk-analytics-inc-common-stock"
company: "Verisk Analytics Inc."
source_id: "verisk-analytics-inc-common-stock-news-import-737963da0dcd"
canonical_url: "https://www.verisk.com/blog/cms-releases-section-111-nghp-user-guide-version-8.5/"
published_at: null
first_seen_at: "2026-07-22T18:42:47.461417+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:694f08fd9eb45e34461a06ee829ddd71fd726e8f7301ecc2665691b9f840582f"
---

# CMS Releases Section 111 NGHP User Guide (Version 8.5)

The Centers for Medicare and Medicaid Services (CMS) has released an updated[Section 111 NGHP User Guide (Version 8.5, July 13, 2026)](https://www.cms.gov/medicare/coordination-benefits-recovery/mandatory-insurer-reporting/user-guide) regarding Section 111 reporting related to non-group health plans (NGHPs) (liability, no-fault and workers’ compensation).


As usual, CMS lists the new updates in the beginning of each User Guide chapter in a “Summary” page. Reviewing these pages indicates that updates were made to Chapter I (Introduction); Chapter II (Registration Procedures); Chapter IV (Technical Information); and Chapter V (Appendices).


Overall, CMS has not announced any new or major policy changes regarding its core Section 111 reporting directives as part of Version 8.5. Rather, CMS’s new updates concern several modifications to various technical reporting elements in addition to some very minor updates/clarifications applied to prior language in a couple of areas.


The below provides a general overview of these updates, along with the authors’ comments related thereto, as follows:


## 1. Early File Submission and Late File Submission alerts and e-mails are discontinued.


CMS has discontinued these alerts and e-mails “to reduce time spent investigating non-issues, and to reduce confusion and incorrect escalations.”\[1\]


**Comments**


**Early File Submission alerts**


Historically, CMS has sent an Early file Submission alert when an RRE had submitted a file “early” (within 14 days prior to an RRE’s assigned quarterly submission period). In this instance, CMS would issue a threshold error and the file would be placed on hold and CMS would send an automated email to the RRE’s Account Manager informing them as such. In response, absent a request from the Account Manager that the file be released, CMS would “hold” processing the file until the first day of the RRE’s assigned submission period.


Based on CMS’s newly published updates, it appears that CMS will no longer be generating any further notifications in relation to these early submission scenarios. However, what remains unclear from CMS’s update is whether the files in question will no longer be placed on hold until the first day of the RRE’s assigned submission period or will now be processed upon receipt. Further, while CMS notes it is eliminating the early file submission (more specifically referenced as the “File submitted prior to assigned submission period” threshold within CMS’s NGHP User Guide documentation), the authors note that within Chapter IV, in Section 7.3.2 on threshold errors, there remains an entry in Table 7-2 which still references this specific threshold error to which no new updates have been applied.\[2\] From the author’s perspective this would appear to have been a potential oversight on CMS’s part and is a point which may warrant additional clarification.


**Late File Submission alerts**


Historically, CMS has issued these alerts in scenarios where a file had not been submitted within 7 days after the end of the RRE’s assigned quarterly submission period. These alerts and associated emails were essentially just a means for CMS to notify the RRE’s Account Manager that they may have missed submission of their scheduled quarterly claim input file submission. In the big picture, it is not uncommon for smaller RREs to have no new coverage reports, or updates to prior coverage reports, which would require submission in any given quarter. As such, from the authors’ perspective, these alerts and emails seemed unnecessary and, per CMS’s comments noted above, it appears that the agency may have also come to a similar conclusion in deciding to eliminate these alerts.


## 2. Files in MSP HOLD (TIN) Error status or Severe Error status will automatically be deleted after 35 days **.**


CMS notes that it will be deleting Section 111 file submissions sitting in these particular statuses after 35 days “to improve efficiency.”\[3\] Overall, for the reasons noted below, this appears to be a sensible update and will likely be viewed favorably by RREs as files remaining in either of these error statuses historically could prevent subsequent file submissions from processing successfully until/unless those files were deleted by the BCRC.


**Comments**


By way of background, when an RRE submits a file to CMS where that file submission triggers either an MSP Hold (TIN) Error or a Sever Error, these are generally scenarios in which the file in question cannot be processed by CMS and where a new file submission will typically be required of the RRE. An RRE is expected to reach out to their assigned BCRC EDI Representative when such a file level error is triggered, and the EDI Rep will then typically end up deleting the associated file so that that a new file could be submitted in its place. Based on CMS’s new update, if that outreach and subsequent manual BCRC deletion of the file in question does *not* occur, CMS’s process should automatically delete the files in question after a period of 35 days. From the authors’ perspective, this is a sensible update as files remaining in either of these error statuses could historically prevent subsequent file submissions from processing successfully until/unless those files were deleted by the BCRC. Deleting those files automatically after 35 days, especially since processing such files is not typically an option, should be helpful in preventing subsequent files from being held up from processing unnecessarily.


## 3. The HEW software version language has been updated. ****


CMS has also updated the HEW software language in Chapter IV (Section 8.1) by deleting references to an outdated version of their HEW (HIPPA Eligibility Wrapper) software in a passage discussing the associated file layouts for the query file process as outlined more fully in the endnote to this sentence.\[4\]


## 4. Error code CW08 has been clarified to note that if the Claim Input File Detail Record is provided and does not match the existing WCMSA case, the record will be rejected (Chapter V, Appendix G).


CMS has added a specific note clarifying that if a Case Control Number value is provided within the Claim Input File Detail Record and the value provided does not match the existing WCMSA case, the record will be rejected.\[5\]


**Comments**


This is simply some additional clarifying language added to the Record Layout Field Description within CMS’s Claim Response File Error Code Resolution Table (Chapter V, Appendix G, Table G-4) and not indicative of a new change.\[6\] An incorrect or mismatching Case Control Number has always resulted in a rejection since the field was implemented with CMS’s new WCMSA reporting requirements which took effect as of April 4, 2025. From the authors’ perspective this is somewhat of a puzzling (and perhaps unnecessary) update as all errors within the aforementioned table, unless specifically noted otherwise, will result in a rejection of the associated coverage record.


## 5. Effective January 2027: New Disposition Code “GL” will result for a Claim Response File when a record is not accepted by the BCRC because it is part of a Global Resolution Settlement (Appendix G).


CMS has indicated that, as of January 2027, claims which it has specifically identified as being related to resolution of a global settlement will be rejected with a newly introduced disposition code of “GL”.\[7\]


**Comments**


From the authors’ view, this is a particularly interesting update as it appears to reference an alternate non-Section 111 related process CMS utilizes which involves working with RREs, and associated third party vendors, to resolve conditional payment recovery claims regarding global resolution settlement scenarios, which typically involve numerous involved plaintiffs and defendant parties.


While some RREs are likely aware of this process, CMS has never officially published any specific documentation about how RREs and/or other involved third parties may enter into such an agreement with CMS in global resolution settlement scenarios, although CMS has made general acknowledgements of this process in response to questions during prior webinar sessions.


With this background noted, CMS presumably implementing a new disposition code to indicate rejection of a Section 111 report for a claim which has been resolved via this alternate process would seem a bit unusual and, as such, may likely be confusing to many RREs who may not have any specific knowledge about such a process even existing.


In this regard, one may question whether CMS should first officially publish documentation regarding this alternative process before implementing changes that would lead to a rejection of Section 111 claim submissions. Accordingly, without any official documentation from CMS regarding the parameters of this alternative process, the authors anticipate that this newly introduced change may generate questions and confusion for many RREs.


With all that being said, CMS *has* previously confirmed, albeit in casual non-officially published fashion, that claims resolved via this alternative global resolution process should not be separately reported via the Section 111 process, as doing so may lead their process to automatically initiate duplicative conditional payment recovery processes.


From the authors' experience, there are some third-party vendors, with apparent CMS approval, who after entering into these alternative global resolution agreements with CMS, provide RREs with written notification regarding specific claims’ resolution via this process, with instructions *not* to submit these claims via the Section 111 process.


Taking all of the above into consideration, as best the authors can discern, based on the minimal information provided via this current CMS User Guide update, it appears that CMS will now look to block the reporting of these claims via the Section 111 process in order to avoid the aforementioned potential for inappropriate duplicative recovery efforts being triggered via their automated processes. This may be another area where further CMS guidance and clarification may be helpful.


## 6. To reduce errors in address checks, it is no longer advisable to enter all zeros when entering a ZIP code when using the “FC” code (Appendices A, B, D, and G).


CMS has changed its guidelines for the default values it has historically suggested RREs provide regarding the “Zip Code and Zip + 4” address fields in scenarios linked to a foreign address, for which an “FC” value would be populated as the associated State Code. Based on the limited information CMS has provided in its summary of chapter updates, this is apparently in relation to issues zero filled zip code values may cause with elements of CMS’s address validation (“address checks”) processes.\[8\]


**Comments**


The general premise of this change is relatively straight forward. Historically, in situations where a foreign address had been applicable in relation to certain representative, additional claimant, or RRE address fields within the Section 111 Claim Input File or TIN Reference File, CMS noted that associated Zip Code or Zip + 4 fields should either be zero filled by default or, in some scenarios, suggested that the field should be populated with *either* zeroes or spaces. Now, due to certain “address checks”/address validation related issues, CMS is now instructing RREs to populate both Zip Code and Zip + 4 fields only with spaces when an “FC” is populated as the State Code indicating a foreign address.


That being said, CMS’s new change made in Version 8.5 actually impacts as many as 20 different Zip Code and Zip +4 data elements within the aforementioned files each with its own corresponding error code related update in the Claim Response File Error Code Resolution Table (Chapter V, Appendix G, Table G-4). Unfortunately, per the authors’ careful review of the applicable User Guide sections, CMS has apparently missed an appropriate update to the description field of one Claim Input File Zip + 4 Description field (Claimant 1 Zip + 4 for which the Zip Code field for the same address *was* updated accordingly) within the Claim Input File Layout. Similarly, CMS has missed approximately 7 Possible Cause field updates within the Claim Response File Error Code Resolution Table. With that in mind, further subsequent updates would seem to be warranted and hopefully CMS will appropriately address these missing updates to their file layouts and error code tables in a future User Guide update. For a full list of specific impacted Claim Input File and TIN Reference File data elements in addition to a full list of impacted error codes (with the aforementioned missed updates also specifically noted) please see the endnote to this sentence below.\[9\]


## 7. Verbiage has been added to clarify that a member’s most current Medicare ID will be provided when a match is found (Appendix C and Appendix E).


Here CMS made very minor clarifying updates to two Description fields within the Claim Response File (Appendix C) and Query Response File (Appendix E) layouts in Chapter V. In the Description for the Applied Injured Party Medicare ID field within the Claim Response File layout (Appendix C), CMS simply updated the prior language which indicated “Current Medicare Beneficiary Identifier” to read “Most Current Medicare Beneficiary Identifier.”\[10\] Similarly, in the Description for the Medicare ID within the Query Response File layout (Appendix E), CMS updated the prior language, which reflected “Current Medicare Beneficiary Identifier,” to read “Most Current Medicare Beneficiary Identifier.”\[11\]


## Questions?


Please do not hesitate to contact the authors if you have any questions, or would like to learn more about how Verisk can help you with your Section 111 reporting obligations.


---


\[1\] CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapters I, II and IV, Chapter 1: Summary of Version 8.5 Updates. These updates impact Chapter I, (Chapter 7), Chapter II (Section 4.3.2 and Chapter 6), and Chapter IV (Chapter 12).


\[2\] CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapter IV, Section 7.3.2, Table 7-2.


\[3\] CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapter II and Chapter IV, Section 1: Summary of Version 8.5 Updates. These updates impact Chapter II, (Chapter 6) and Chapter IV (Sections 6.3.3, 7.3.1, and 8.3).


\[4\] On this point, CMS’s prior language stated as follows:


“The Query Input and Response Files are transmitted using the ANSI X12 270/271 Entitlement Query transaction set (currently using version 5010A1). However, the BCRC will supply software (the HIPAA Eligibility Wrapper or “HEW software”) to translate flat files to and from the X12 270/271 formats. The file layouts that serve as input and output for ***Version 4.0.0*** of the HEW software are documented in the NGHP User Guide Appendices Chapter V (authors’ emphasis).” CMS’s Section 111 NGHP User Guide (Version 8.4, April 13, 2026), Chapter IV, Section 8.1.


With the current NGHP User Guide Version 8.5 update, CMS has updated the above referenced passage to simply remove any specific version reference for the HEW software, presumably to avoid the need for subsequent updates if/when a newer version of the software is published. The updated verbiage now reads as follows:


“The Query Input and Response Files are transmitted using the ANSI X12 270/271 Entitlement Query transaction set (currently using version 5010A1). However, the BCRC will supply software (the HIPAA Eligibility Wrapper or “HEW software”) to translate flat files to and from the X12 270/271 formats. The file layouts that serve as input and output for the HEW software are documented in the NGHP User Guide Appendices Chapter V.” CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapter IV, Section 8.1.


\[5\] CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapter V, Section 1: Summary of Version 8.5 Updates.


\[6\] CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapter V, Appendix G, Table G-4.


\[7\] CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapter V, Section 1: Summary of Version 8.5 Updates.


\[8\] Id.


\[9\] CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapter V, Appendices A, B and G. While CMS referenced updates to Appendix D in its Summary of Version 8.5 Updates, per the authors’ review, Appendix D had no updates and associated updates would not apply to this appendix as Appendix D is the TIN Reference Response File layout and the changes outlined here are only applicable to CMS’s input files submitted by RREs and not the corresponding response files returned by CMS.


The associated Claim Input File and TIN Reference File fields, as well as the associated error codes from the Claim Response File Error Code Resolution Table, are outlined below, with **bold type face** denoting those fields or error codes which CMS has *not* updated as follows:


- Claim Input File Detail Record:


- Representative Mail Zip Code (Field 80)
- Representative Mail Zip + 4 (Field 81)
- Claimant 1 Zip (Field 101)
- **Claimant 1 Zip + 4 (Field 102)**
- C1 Representative Zip (Field 115)
- C1 Representative Zip + 4 (Field 116)


- Claim Input File Auxiliary Record:


- Claimant 2 Zip (Field 17)
- Claimant 2 Zip + 4 (Field 18)
- C2 Representative Zip (Field 31)
- C2 Representative Zip + 4 (Field 32)
- Same updates would also be applicable to the following fields:


- Claimant 3 Zip (Field 46)
- Claimant 3 Zip + 4 (Field 47)
- C3 Representative Zip (Field 60)
- C3 Representative Zip +4 (Field 61)
- Claimant 4 Zip (Field 75)
- Claimant 4 Zip + 4 (Field 76)
- C4 Representative Zip (Field 89)
- C4 Representative Zip + 4 (Field 90)


- TIN Reference File (Appendix B)


- TIN/Office Code Zip (Field 10)
- TIN/Office Code Zip + 4 (Field 11)


- Associated Error Codes from Appendix G:


- Claim Input Detail Record


- CC10 (Claimant 1 Zip – Field 101)
- CC11 (Claimant 1 Zip + 4 – Field 102)
- CR10 (Representative Mail Zip Code – Field 80)
- CR11 (Representative Mail Zip + 4 – Field 81)
- CR30 (Claimant 1 Representative Zip – Field 115)
- CR31 (Claimant 1 Representative Zip + 4 – Field 116)


- Claim Input Auxiliary Record


- CC30 (Claimant 2 Zip – Field 17)
- CC31 (Claimant 2 Zip + 4 – Field 18)
- **CC50 (Claimant 3 Zip – Field 46)**
- **CC51 (Claimant 3 Zip + 4 – Field 47)**
- **CC70 (Claimant 4 Zip – Field 75)**
- **CC71 (Claimant 4 Zip + 4 – Field 76)**
- CR50 (Claimant 2 Representative Zip – Field 31)
- CR51 (Claimant 2 Representative Zip + 4 – Field 32)
- CR70 (Claimant 3 Representative Zip – Field 60)
- **CR71 (Claimant 3 Representative Zip + 4 – Field 61)**
- CR90 (Claimant 4 Representative Zip – Field 89)
- CR91 (Claimant 4 Representative Zip + 4 – Field 90)


- TIN Reference File Detail Record


- **TN08 (TIN/Office Code Zip – Field 10)**
- **TN09 (TIN/Office Code Zip + 4 – Field 11)**


\[10\] CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapter V, Appendix C.


\[11\] CMS’s Section 111 NGHP User Guide (Version 8.5, July 13, 2026), Chapter V, Appendices C and E.
