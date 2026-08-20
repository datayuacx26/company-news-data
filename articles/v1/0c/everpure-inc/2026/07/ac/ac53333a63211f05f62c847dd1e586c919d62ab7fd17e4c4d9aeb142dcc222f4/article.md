---
schema_version: "1.0.0"
document_id: "ac53333a63211f05f62c847dd1e586c919d62ab7fd17e4c4d9aeb142dcc222f4"
company_key: "everpure-inc"
company: "Everpure Inc."
source_id: "everpure-inc-rss-a7fca946ec64"
canonical_url: "https://blog.everpuredata.com/perspectives/rubrik-for-volumes-rapid-ehr-recovery/"
published_at: "2026-07-15T13:00:00+00:00"
first_seen_at: "2026-07-25T03:30:11.662383+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:ad5acdb9b176dfc16585b66c55e7b117ff6fdffe22ba3245db36971c3da21623"
---

# Rubrik for Volumes: Rapid EHR Recovery on Everpure FlashArray

### Summary


Rubrik for Volumes integrates Everpure FlashArray with Rubrik Security Cloud to provide unified data protection and rapid ransomware recovery for critical hospital EHR systems.


For a hospital, the electronic health record (EHR) is the one system that cannot go down. The record is where a nurse checks a patient’s allergies before giving a dose and a physician reads the latest labs before deciding how to treat a patient. When ransomware reaches it, all of that stops. The emergency department falls back to paper, and staff cannot look up a patient’s history. What happens next comes down to one thing: how fast the record comes back, and whether the copy it comes back from is clean.


When that answer comes slowly, patients feel it first. Ransomware at major health systems has cancelled tens of thousands of appointments, diverted ambulances, and put nurses back to timing medications by hand. One of our healthcare customers called a cyber attack “a potentially life-threatening event,” and as a result, built its architecture accordingly before an attack ever occurred.


When the 2024 attack on Change Healthcare halted claims and pharmacy processing across much of the country, its parent company paid a ransom of about[$22 million](https://www.cnbc.com/2024/05/01/unitedhealth-ceo-says-company-paid-hackers-22-million-ransom.html) . Overall, the attack cost the organization roughly[$2.3 billion](https://www.forbes.com/sites/brucejapsen/2024/07/16/unitedhealth-group-cyberattack-costs-to-eclipse-23-billion-this-year/) . The ransom is the smallest part of the cost. The real damage is in the days when a hospital cannot see patients.


## **The system under the EHR**


Every patient record depends on the operational database that runs Epic. Many hospitals run that database on high-performance Everpure™[FlashArray](https://www.everpuredata.com/products/block-file-object-storage.html) ™ because a patient record needs sub-millisecond response times and cannot wait behind anything slower. That block storage, though, has usually sat apart from the platform that protects everything else. A hospital governs its virtual machines, databases, and cloud workloads as one estate. The block storage lives on its own island, with its own console and its own rules.


So protection takes the long way around. That path routes through the hypervisor, taxes the production system, and forces a hospital to choose between taking copies often enough and keeping the record fast enough for patient care. And a local snapshot, on its own, cannot tell anyone which copy is clean. The middle of an attack is the worst possible time to be guessing.


## **What Rubrik for Volumes does**


Rubrik for Volumes closes that gap. Everpure FlashArray volumes and protection groups become managed objects inside Rubrik Security Cloud, carrying the same policies, roles, and reporting as the virtual machines and databases the hospital already protects there. The storage team and the backup team stop working from two consoles, and the compliance auditor sees one record of what is protected and how.


Rubrik drives the snapshots straight through the array’s own API, outside the production data path, so the record runs at full speed while Rubrik protects it. The engine reads only the blocks that have changed. This keeps backup windows flat as a decade of patient data accumulates. It can also protect as much as 250 terabytes in a single day. That’s what cyber resilience at flash speed means in practice. Protection stops being something a hospital has to schedule around.


## **Depth is what holds**


The architecture holds because it works in layers, each one assuming the layer above it has failed. On the array, Everpure[Auto-On SafeMode](https://blog.everpuredata.com/purely-technical/auto-on-safemodetm-now-available-in-purity-6-4-10/) ™ takes snapshots no one can change or delete, not even an attacker with stolen storage-admin credentials. Off the array, Rubrik replicates those copies into an isolated, air-gapped vault that an intruder cannot reach through the ordinary network. When it’s time to come back online, Rubrik restores a clean, verified copy onto the array. The team starts from data it already knows is good and can get the hospital back up and running in hours.


While detection can tell a hospital that the attacker is inside, it does not bring the record back. What brings it back is the architecture the hospital put in place before the attack, the kind of work you finish when things are calm. Rubrik for Volumes puts that work where the record already lives. The solution will ship this month.


## **Start your 30-day free trial**


Reduce Azure VM storage costs, simplify operations, and run business-critical workloads on fully managed, Azure-native block storage.


[Start your 30-day free trial](https://marketplace.microsoft.com/en-us/product/saas/purestoragemarketplaceadmin.psc_contact_me?tab=Overview&utm_theater=all&utm_region=all&utm_campaign=oth&utm_medium=qrcode&utm_source=blog&utm_content=other)
