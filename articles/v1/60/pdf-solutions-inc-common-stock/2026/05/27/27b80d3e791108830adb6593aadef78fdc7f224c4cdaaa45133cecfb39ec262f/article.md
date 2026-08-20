---
schema_version: "1.0.0"
document_id: "27b80d3e791108830adb6593aadef78fdc7f224c4cdaaa45133cecfb39ec262f"
company_key: "pdf-solutions-inc-common-stock"
company: "PDF Solutions Inc."
source_id: "pdf-solutions-inc-common-stock-rss-c6b67875c893"
canonical_url: "https://www.pdf.com/north-america-information-control-committee-spring-2026-update/"
published_at: "2026-05-26T18:46:06+00:00"
first_seen_at: "2026-07-25T01:12:24.037302+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:f6142db74d49357b06a2aaecdbbb5a4b14df66410a9c29e20c60034cd2b9a4c4"
---

# North America Information & Control Committee Spring 2026 Update

**Categories:**


##### [Industry Standards](https://www.pdf.com/category/industry-standards/)


**Tags:**


##### [Subject Matter Experts](https://www.pdf.com/tag/subject-matter-experts/)


,


##### [SEMI Standards](https://www.pdf.com/tag/semi-standards/)


,


##### [SECS/GEM](https://www.pdf.com/tag/secs-gem/)


,


##### [GEM 300](https://www.pdf.com/tag/gem-300/)


Posted on May 26, 2026


by[Brian Rubow](https://www.pdf.com/author/brianrubow/)


## Background


The SEMI North America Information & Control Committee Spring meetings took place in conjunction with the Advanced Semiconductor Manufacturing Conference (ASMC), a leading international technical conference focused on solutions that improve semiconductor manufacturing expertise. The event was held at the Hilton in Albany, New York.


As usual, the first two days focused on task force meetings, including[GEM 300](https://www.pdf.com/standards/semi-gem-300-standards/) , ABFI (Advanced Backend Factory Integration), GUI, CDS (Fab & Equipment Computer & Device Security), DDA (Diagnostics Data Acquisition), and the Digital Twin task force, all co-led by PDF Solutions. The third day hosted the committee meeting, where members made final decisions based primarily on task force recommendations. This summary highlights key developments from selected task force sessions and the committee meeting.


All ballots approved by the committee remain subject to final review by the global SEMI Audit & Review Committee. While rare, ballots can still fail if they do not strictly follow SEMI procedures and regulations.


A SNARF (Standards New Activity Report Form) must receive technical committee approval before a task force can submit a ballot proposing a new standard or changes to an existing one. SEMI maintains a comprehensive website with detailed information on global committee activities:


🔗[SEMI Standards Development](https://www.semi.org/en/products-services/standards/developing-technical-standards)


## Digital Twin Task Force


The task force is preparing to submit its first ballot defining digital twin terminology and establishing minimum requirements for digital twins in the semiconductor industry. Over time, the group plans to develop one or more interfaces for interacting with digital twins, although that work remains undefined and scheduled for a future phase.


## GEM 300 Task Force


During both the task force and committee meetings, members passed ballot 7447 addressing a well-known variable issue in E40 (Process Job Management).


Another ballot 7427 proposed updates to E172 (Specification for SECS Equipment Data Dictionary):


- The first line item restricts the Well-Known Name field to SEMI-defined names and introduces a new field for custom use. This item advanced to a ratification ballot to incorporate technical adjustments ensuring that the custom field supports any use agreed upon by the equipment supplier and end user.
- The second line item updates the Equipment Characterization matrix to enable proper carrier location identification. This item passed.


**Ballot 7345B (E90—Specification for Substrate Tracking)** failed due to a defect in the ballot.


A batch represents a group of substrates processed simultaneously. Different substrate handling methods have led to disagreement on when to create and delete a batch. This ballot aimed to resolve two issues:


1. A contradiction exists in the Batch Location state model between state definitions and the transition table. The standard defines the UNOCCUPIED state as containing no substrates, yet the transition table allows transitions to OCCUPIED only when all substrates have moved into the batch location.
2. The task force aligned on a solution after previously evaluating three competing proposals.


The ballot failed due to minor technical errors and requests for additional clarifications. However, the updated version represents a significant improvement over prior iterations. The task force expects the next revision to pass unless new issues arise.


**Ballot 7428A** , which proposes standardizing secure HSMS communication, remains the most impactful ballot under development. It defines a three-stage communication model:


1. TCP/IP
2. TLS (Transport Layer Security)
3. HSMS-SS


The ballot failed on a minor SEMI procedural issue. Despite this, the group reached strong consensus on the overall technical direction. However, members continue to debate implementation details, including whether the specification should mandate specific error-handling behaviors during TLS authentication beyond what RFC 8446 requires.


**Ballot 7447** proposes an E40 update to a well-known name and passed without controversy.


The committee approved several new activities for ballot development:


- A proposed update to[E5](https://www.pdf.com/standards/semi-e5/) and[E30](https://www.pdf.com/standards/semi-e30/) would improve report creation and event linkage. Currently, S1F23/S1F24 messages define associations between data variables and collection events, but host software can link any variable to any event. The proposal introduces a setting requiring equipment to flag errors when unassociated variables are linked improperly. This change would address a common GEM interface issue.
- A second activity clarifies internal buffer implementation and port usage for equipment that stores carriers and transfers substrates.
- A third activity corrects another well-known name in E40.


## DDA (Diagnostics Data Acquisition) Task Force


The DDA task force continues finalizing the Freeze 3 version of EDA/Interface standards based on gRPC technology. Two ballots were reviewed during the latest cycle:


- **Ballot 7422 (E164)** failed. Because Freeze 3 depends on E164, this standard must pass before Freeze 3 can be declared complete. The task force will revise and re-ballot it. The primary challenge is completing subordinate standards that implement GEM 300 functionality.
- **Ballot 7419A** advanced to a ratification ballot for final technical updates. This ballot includes updates to E120, E125, E132, E134, and E179. If ratified, these standards will form the core of EDA Freeze 3.


## CDS (Fab & Equipment Computer & Device Security) Task Force


No ballots were adjudicated for the CDS task force. The group continues to evaluate the future of SEMI E188 (equipment cybersecurity), especially since end users have withdrawn support.


The task force reviewed **Ballot 7426** , which proposes major enhancements to[SEMI E191](https://www.pdf.com/standards/semi-e191/) (computing device cybersecurity status reporting). Current reporting requirements include:


- **E191-1024 (current version):** ComputingDeviceIdentifier, OSManufacturer, OSName, OSVersion, OSBuild
- **Ballot 7311A additions:** OSInstalledUpdates, OSInstalledComponents
- **GEM (SEMI E30):** MDLN, SOFTREV, EqSerialNum, E30EquipmentSupplier, EqpName


With the recent publication of updates from Ballot 7311A, work on Ballot 7426 is progressing. This proposal adds:


- Lists of open and established ports with associated processes
- Running processes
- Local user accounts
- Firewall status
- Device driver versions
- BIOS information


If adopted, these additions would significantly enhance a factory’s ability to assess cybersecurity risks on equipment systems. However, the task force must refine several details, and overall support remains uncertain.


## Next Steps


The North America Information & Control Committee will use:


- **Cycle 5** for ratification ballots (June 2 – July 2, 2026)
- **Cycle 7** for general ballots (submission deadline: July 31, 2026; voting window: August 19 – September 18, 2026)


The committee will adjudicate these ballots at the North America Fall SEMI Standards Meetings, held October 13–15, 2026, during SEMICON West at the Moscone Center in San Francisco.


**Author Credentials:**


SEMI North America


Regional Standards Committee Co-Chair


Information & Control Committee Co-Chair


GEM 300 Task Force Leader


DDA Task Force Leader


Advanced Backend Factory Integration Task Force Leader


Digital Twin for Manufacturing Task Force Leader


[For more information on SEMI Standards, check out the Standards section on PDF.com.](https://www.pdf.com/standards/)
